# Search Queries for Job Scraper

<!-- SETUP: Customize these queries based on your skills, target roles, and location -->

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** write every query category in every language listed in your CLAUDE.md Languages table (typically 1-2, sometimes more). A posting requiring a language you have *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than you declared in a language you *do* work in is flagged for your own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate, the single source of truth for this rule. Translate each category's keywords rather than machine-translating word-for-word (e.g. "Frontend Developer" -> "Desarrollador Frontend", not a literal word-for-word translation) if you work in more than one language.

## Search Sites

Primary (your market's job boards - scaffold one with `/add-portal`):
- **seek.com.au / seek.co.nz** - largest general job board for Australia/New Zealand. Not yet scaffolded as a CLI skill - **priority candidate for `/add-portal`**. Until then, covered via the `site:` WebSearch fallback below.
- **linkedin.com/jobs** - LinkedIn job listings (filter: Australia/New Zealand, Melbourne/Auckland/Sydney/Brisbane); covered by the `linkedin-search` CLI
- **jora.com.au** - secondary AU/NZ aggregator (optional, WebSearch fallback only)

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies

## Query Categories

Queries are grouped by priority. Write **each category in every language from your Languages table** (see Language scope above). Combine each query with your location terms (e.g. your city, region, or metro area) where the site supports it.

### Priority 1: AI / Machine Learning Engineer

These match the strongest and most desired career direction (top priority).

```
site:seek.com.au "AI Engineer" OR "Machine Learning Engineer" Melbourne
site:seek.com.au "Machine Learning Engineer" graduate OR junior Melbourne
site:linkedin.com/jobs "Machine Learning Engineer" Australia
site:linkedin.com/jobs "AI Engineer" graduate Australia OR "New Zealand"
```

### Priority 2: Software Engineer

Second priority - general graduate/junior software engineering roles.

```
site:seek.com.au "Software Engineer" graduate OR junior Melbourne
site:seek.co.nz "Software Engineer" graduate OR junior
site:linkedin.com/jobs "Graduate Software Engineer" Australia
```

### Priority 3: Data Engineer / Data Scientist

Third priority.

```
site:seek.com.au "Data Engineer" OR "Data Scientist" graduate OR junior Melbourne
site:linkedin.com/jobs "Data Engineer" OR "Data Scientist" graduate Australia
```

### Priority 4: Broader Technical / Internships & Grad Programs

Wider net, including internships and structured graduate programs.

```
site:seek.com.au "graduate program" software OR data OR engineering Melbourne
site:linkedin.com/jobs "software engineer intern" OR "graduate program" Australia
site:seek.com.au Python developer Melbourne
```

## Location Filter

When evaluating results, verify the job location is within reasonable range. Define acceptable areas:
- Melbourne, VIC and surrounding areas (ideal)
- Auckland / New Zealand nationwide (ideal - equally acceptable to Melbourne)
- Sydney, NSW (acceptable - lower priority)
- Brisbane, QLD (acceptable - lower priority)
- Other Australian/NZ cities (borderline - flag for discussion)
- Outside Australia/New Zealand (too far)

## Language Filter

Your working languages and levels are in CLAUDE.md's Languages table. When filtering scraped results, apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language you haven't declared at all is excluded; a posting requiring a higher level than you declared in a language you do work in is not excluded, flag it clearly instead (see `job-scraper/SKILL.md`'s Step 3 "Quick Fit Assessment" for how the flag surfaces in `/scrape` output). Postings simply *written* in a language you don't work in, that don't require it on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
