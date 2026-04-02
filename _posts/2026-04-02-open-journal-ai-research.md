---
layout: post
title: "An Open-Access Journal for AI-Generated Research"
date: 2026-04-02
---

![Flow 1 — Typical Paper Publish Roadmap](/assets/posts/open-journal/flow1.png)

![Flow 2 — Review & Publishing Pipeline](/assets/posts/open-journal/flow2.png)

# The Problem

Traditional academic publishing moves slowly. Papers take months to go from idea to publication, and the process is gatekept by a small number of high-impact journals. Meanwhile, AI systems are increasingly capable of generating novel research — code, data, analysis — but there's no credible venue for publishing AI-generated work with proper transparency and rigor.

# Typical Paper Publish Roadmap

**Stage 1 — Results**

Come up with meaningful results.

**Stage 2 — Submission**

From idea to published. Submit a request to be published.

**Stage 3 — Verification**

- Are results meaningful?
- Are they reproducible?

**Target requirements:**

1. Code only (open-source)
2. Data deposited on Zenodo

If any of these three things are missing — code, data, or reproducibility — **the paper is retracted, corrected, or deleted.**

# Stage 4 — Humans Involved

1. **Person making a research contribution** receives credit.
2. **Reviewing** — initially unpaid, but credited.

## Logistical Considerations

- Hosting
- PDF generation
- Summaries
- Citations
- Typical infrastructure handled by a journal/publisher
- Start as a blog and let it grow organically

## Critical Questions

- Who decides which paper gets filtered?
- What should be important?
- What upgrades need to be made?
- How are changes and amendments processed?

# Implementation

```
SCR → Journal/Manager → Review
```

### Traditional Model (Example: Nature)

```
Nature → Publisher → Editor → Reviewers (1, 2, 3)
                              ↓
Paper → Citation → Revisions → Final Output
```

# Core Ideas

- **Open-source research** — everything public, everything auditable
- **Mandatory reproducibility** — no exceptions
- **Code + data requirement** — both must be deposited and accessible
- **Transparent review** — reviews are public and attributed
- **Human + AI collaboration** — clear delineation of roles
- **Decentralized credit** — contributions tracked and attributed fairly

# Authorship & Attribution

The AI system (model name, version, date) is listed as author — e.g., *"Claude 3.7 Sonnet, operated by J. Patel."*

Human contributions are transparently disclosed in a **Contribution Footnote**: *"Prompted and guided by...", "Data curated by...", "Experimental setup by..."*

No human receives citation credit or co-authorship for AI-generated content — this is non-negotiable and enforced by editorial policy.

# Review System

- PhD students and postdocs serve as **Certified Reviewers**
- Reviewers receive a named **Review Credit** in the paper and on their public reviewer profile page — useful for CVs and tenure dossiers as a new category of academic contribution
- Reviews are structured and scored for quality, creating a **reviewer reputation system**

# Hosting & Infrastructure

- Fully open-source platform (think arXiv meets GitHub)
- DOIs issued for each paper
- Machine-readable metadata so AI authorship is trackable across databases
- API access so researchers can study the corpus
