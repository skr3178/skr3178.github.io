---
layout: post
title: "Odin — AI-Powered Job Platform"
date: 2026-03-30
---

The job market is broken. 93% of jobs are ghost postings with no real intent to hire. 75% of resumes are rejected by ATS before a human sees them. The average hire rate from job boards is 0.5%. Candidates can't get responses; employers can't find quality candidates.

I built **Odin** to fix this — two systems working together: a personal job seeker CLI that automates the grind, and a marketplace platform that rewires how hiring actually works.

![Candidate dashboard](/assets/posts/odin-ai-job-platform/Candidates_page.png)

![Recruiter dashboard](/assets/posts/odin-ai-job-platform/Recruiter_page.png)


## Part 1: Job Seeker CLI


The CLI automates the full job application pipeline: fetching listings, scoring each role against your profile, generating tailored cover letters, submitting applications, and coaching you through interviews. It also tracks every application from first seen to offer.

### How scoring works

Scoring happens in three stages — no wasted API calls:

1. **Pre-filter** — 3,500+ jobs down to ~500 engineering-relevant ones via fast keyword match
2. **GitHub verification** — maps your repos to skills with evidence URLs
3. **AI scoring** — Claude reads the full job description + candidate profile + GitHub evidence, then returns a score 0–100 with matched skills, missing skills, gap steps, and a summary

Robotics/ML/humanoid titles are prioritized to surface the most relevant roles first.

### Cover letter intelligence

Each cover letter picks the 2–3 most relevant GitHub projects based on skill overlap with the job, then references them by name with repo URLs. Claude writes in a direct, technical tone, citing actual keywords from the job description — not generic platitudes.

---

## Part 2: Marketplace Platform


An Alibaba-style freelance marketplace connecting clients with freelancers globally. AI-powered matching, escrow payments, and two-way accountability baked in from the start.

### Key features

- **AI Matching** — Scores freelancer-project fit, suggests prices based on scope
- **Secure Escrow** — Stripe Connect holds funds until the client approves work
- **Mutual Accountability** — Two-way reviews, response time tracking on both sides
- **Project Scoping** — AI breaks project descriptions into concrete milestones
- **Verified Postings** — Employers prove active hiring intent before listing

### Escrow payment flow

1. Client creates project and accepts AI-suggested price
2. Freelancer submits proposal → client accepts
3. Project is split into milestones → client pays upfront (Stripe holds)
4. Freelancer delivers → client reviews
5. Client approves → Stripe releases to freelancer
6. If disputed → admin resolution flow
7. Platform takes 5–10% fee per transaction

### Build phases

- **Phase 1** ✅ — Foundation (auth, profiles, DB, landing page)
- **Phase 2** — Core marketplace (project posting, freelancer profiles, proposals)
- **Phase 3** — AI layer (matching, pricing, scoping)
- **Phase 4** — Payments and escrow (Stripe integration, payouts)
- **Phase 5** — Trust and polish (reviews, messaging, disputes)

---

## Why this matters

The root problems in hiring aren't about effort — candidates apply to hundreds of jobs, recruiters screen thousands of resumes — they're structural. Ghost jobs waste everyone's time. ATS keyword filters reject qualified people. There's no accountability on either side.

The CLI tackles the candidate side: reduce friction, surface the right roles, and stop wasting time on applications that never get read. The marketplace tackles the structural side: verified postings, skill-first matching, and mutual accountability so both sides have skin in the game.

Neither is a perfect fix. But together they're a start toward hiring that actually works.
