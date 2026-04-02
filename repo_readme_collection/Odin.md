# Odin — AI-Powered Job Platform

A two-part system: an automated job seeker for personal use, and a marketplace platform for connecting freelancers with clients globally.

## Part 1: Job Seeker Tool (Python CLI)

Automated job seeker, scorer, applier, and interview coach. Built around your profile.

### What it does

| Command | What happens |
|---|---|
| `seek` | Fetches jobs, analyzes your GitHub, AI-scores each role 0–100 with gap analysis |
| `apply <job_id>` | Generates a tailored cover letter referencing your GitHub projects, then submits |
| `prep <job_id>` | Generates 10 technical + 5 behavioral interview Q&A specific to the role |
| `status` | Shows your full application tracker (seen → applied → interview → offer) |
| `autopilot` | Full pipeline: seek → score → cover letter → confirm → submit |

### Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export ANTHROPIC_API_KEY=your_key_here
```

### Usage

```bash
source .venv/bin/activate

# Refresh job listings from robotics/AI companies + score
python cli.py seek --refresh --rescore

# Show top remote jobs only
python cli.py seek --remote-only --min-score 60

# Filter by UK location
python cli.py seek --location UK

# Apply to a job (dry-run to preview cover letter first)
python cli.py apply 7546284 --source greenhouse --dry-run
python cli.py apply 7546284 --source greenhouse

# Generate interview prep pack
python cli.py prep 7546284 --source greenhouse

# View application tracker
python cli.py status
python cli.py status --filter applied

# Update status when you hear back
python cli.py update 7546284 interview --source greenhouse --note "Call Tue 10am"

# Full autopilot: score → cover letter → confirm → submit
python cli.py autopilot --min-score 70 --limit 3
python cli.py autopilot --min-score 65 --dry-run
```

### Architecture

```
/
  cli.py                     ← main CLI (Click)
  config.py                  ← API keys, paths, constants
  candidate/
    profile.json             ← your structured resume (edit to update)
    github_analyzer.py       ← fetches github.com/skr3178, verifies skills
    github_cache.json        ← 1-hour cache (auto-refreshes)
  jobs/
    matcher.py               ← Claude AI scoring + description fetcher
    scored_jobs.json         ← cached scored results
  apply/
    cover_letter.py          ← Claude cover letter generator
    auto_apply.py            ← Greenhouse/Lever API submission
    tracker.py               ← SQLite application tracker
  interview/
    prep.py                  ← Claude interview Q&A generator
    prep_*.json              ← saved prep packs per job
  scraper/
    fetch_jobs.py            ← fetches Greenhouse/Lever/Ashby APIs
    companies.py             ← target companies (edit to add more)
    jobs.json                ← raw job cache
```

### How scoring works

1. **Pre-filter** — 3,500+ jobs → ~500 engineering-relevant ones (fast keyword match, no API)
2. **GitHub verification** — fetches `github.com/skr3178` live (caches 1hr), maps repos to skills with evidence URLs
3. **AI scoring** — Claude reads full job description + candidate profile + GitHub evidence → returns score 0–100, matched skills, missing skills, gap steps, summary
4. **Priority sort** — robotics/ML/humanoid titles are scored first within the limit

### Cover letter intelligence

Each cover letter picks the 2–3 most relevant GitHub projects based on skill overlap with the job, then references them by name with repo URLs. Claude writes in a direct, technical tone citing actual keywords from the job description.

### Adding more companies

Edit `scraper/companies.py` and run `python cli.py seek --refresh --rescore`.

Find slugs by visiting: `https://boards.greenhouse.io/{slug}`, `https://jobs.lever.co/{slug}`, or `https://jobs.ashbyhq.com/{slug}`.

---

## Part 2: Marketplace Platform (Next.js)

An Alibaba-style freelance marketplace connecting clients with freelancers globally. Features AI-powered matching, escrow payments, and two-way accountability.

### Key Features

- **AI Matching** — Smart algorithm scores freelancer-project fit, suggests prices
- **Secure Escrow** — Stripe Connect holds funds until client approves work
- **Mutual Accountability** — Two-way reviews, response time tracking
- **Project Scoping** — AI breaks down project descriptions into milestones
- **Global Talent** — Connect with freelancers in Tech, Creative, Legal, and other categories

### Setup

```bash
cd marketplace
npm install

# Set up environment
cp .env.local.example .env.local
# Edit .env.local with your Stripe, Claude, and database credentials

# Set up database
npx prisma migrate dev
```

### Run

```bash
cd marketplace
npm run dev
# Open http://localhost:3000
```

### Tech Stack

- **Frontend + Backend** — Next.js 14, TypeScript, Tailwind CSS
- **Database** — PostgreSQL + Prisma ORM
- **Auth** — NextAuth.js
- **Payments** — Stripe Connect (escrow, transfers, payouts)
- **AI** — Claude API (matching, pricing, scoping)

### Database Schema

| Model | Purpose |
|-------|---------|
| `User` | Clients & freelancers (role-based) |
| `FreelancerProfile` | Skills, rates, portfolio, ratings |
| `Project` | Posted by clients with budget & requirements |
| `Proposal` | Freelancer applications with AI match scores |
| `Milestone` | Project stages with escrow payments |
| `Escrow` | Stripe payment intent tracking |
| `Review` | Mutual ratings (2-way accountability) |
| `AIMatchResult` | Matching scores & price suggestions |

### Escrow Payment Flow

1. Client creates project & accepts AI-suggested price
2. Freelancer submits proposal → client accepts
3. Project split into milestones → client pays upfront (Stripe holds)
4. Freelancer delivers → client reviews
5. Client approves → Stripe releases to freelancer
6. If disputed → admin resolution flow
7. Platform takes 5-10% fee per transaction

### Build Phases

- **Phase 1** ✅ — Foundation (auth, profiles, DB, landing page)
- **Phase 2** — Core marketplace (project posting, freelancer profiles, proposals)
- **Phase 3** — AI layer (matching, pricing, scoping)
- **Phase 4** — Payments & escrow (Stripe integration, payouts)
- **Phase 5** — Trust & polish (reviews, messaging, disputes)

See `freelance_market_platform_plan.md` for full implementation plan.

---

## Project Vision

**Job Seeker Tool** — A personal productivity tool that automates the job application process, scores roles intelligently, and coaches you through interviews.

**Marketplace Platform** — A next-generation hiring platform that solves:
- 93% of jobs are "ghost jobs" with no real intent to hire
- 75% of resumes are rejected by ATS before a human sees them
- 0.5% hire rate from job boards
- Candidates can't get responses; employers can't find quality candidates

The marketplace fixes this with:
- Verified job postings (employers prove active hiring)
- Skill-first matching (not resume keywords)
- Two-way accountability (both sides rated)
- AI gap analysis (coaching toward fit, not rejection)

---

## About

Built by Sangram Rout. Questions? Check `product.md` for market research and product thinking.
