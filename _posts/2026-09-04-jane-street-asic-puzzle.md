---
layout: post
title: "Reverse engineering an ASIC: Jane Street's puzzle"
date: 2026-09-04
---

Jane Street's [Can you reverse engineer an ASIC?](https://blog.janestreet.com/can-you-reverse-engineer-an-asic/)
hands you a `puzzle.gds` file — a physical chip layout, no source, no netlist — and asks
three things: recover the netlist, figure out what the circuit actually does, then use that
understanding to find the input it is waiting for.

Everything below is in the repo:
[skr3178/Jane_Street_ASIC_puzzle](https://github.com/skr3178/Jane_Street_ASIC_puzzle).

## 1. Layout to netlist

`puzzle.gds` went through KLayout's LayoutToNetlist to give **728 cells and 92 flip-flops**.
Extraction is the step where a quiet error poisons everything downstream, so I validated it
six independent ways: an EQY equivalence check against the warm-up (whose source is given),
a negative control, Magic and HAL cross-extractions, structural sanity invariants, and a
golden trace reproducing the chip's default `TRY AGAIN` output.

## 2. What the chip does

The interesting part is that this is recoverable from the netlist alone, in three deepening
passes:

- **Structure.** No shift register, no counter, no clean FSM — one entangled 92-bit state,
  with a flop dependency graph of 801 edges and average fan-in 8.7.
- **Linearity.** A delta-input superposition simulation showed the state fold is *non-linear*
  in 30 of 30 trials, with the non-linearity concentrated in a 57-bit hash core while a
  low-weight sequencer drives the display.
- **Exact recurrence.** Symbolic ANF derived from the netlist plus Liberty models gave the
  literal per-bit update equations, and those named the circuit.

It is a **two-star Star Battle checker on an 11&times;11 grid**. The 122-bit serial input is
the grid, row-major, `1` = star. Each rule maps onto identifiable hardware — "exactly 2 stars
per column" is eleven independent 2-bit saturating counters, each stepped only at positions
`p ≡ c (mod 11)` — and position is tracked by a 9-flop non-linear 122-state counter, proven
by ANF brute force. The layout is arranged to hint at this, which is the puzzle's nicest touch.

## 3. The key

With the netlist in hand, finding the accepting input is a bounded model checking problem:
SymbiYosys `cover(success)` with bitwuzla synthesized a 122-bit sequence in about 12 seconds.
The key is exactly the Star Battle puzzle's unique solution, which I then re-derived
independently with a plain solver as a cross-check.

Replaying it through the sky130 reference models: `success` goes high at cycle 125 and stays
high, and the output generator spells `(* TWO STARS *)` — written as an OCaml comment, which
is a fitting way for Jane Street to sign it.

Full writeup, extraction scripts, simulation harnesses and the ANF work are in
[SKR_solution/](https://github.com/skr3178/Jane_Street_ASIC_puzzle/tree/main/SKR_solution).
