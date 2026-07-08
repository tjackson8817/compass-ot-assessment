[README.md](https://github.com/user-attachments/files/29807518/README.md)
# Compass — OT Cybersecurity Risk Assessment

A self-contained, client-side tool for assessing OT cybersecurity maturity
against NIST SP 800-82 Rev. 3 or IEC 62443, mapped to NIST CSF 2.0, with
per-site and multi-site (team, trend, rollup) reporting.

## What's in this repo

- `index.html` — the entire tool. One file, no build step, no backend,
  no API keys. Everything runs in the browser.

## How it works

- Assessments are scored and held in memory while the page is open.
- "Save assessment" downloads the current assessment as a `.json` file.
- "Load saved assessment" reloads a previously saved file.
- The "Team, Trends & Rollups" tab combines multiple saved `.json` files
  for team averaging, trend-over-time, or multi-site rollup views.
- Nothing is stored on a server. There is no login and no database —
  the saved `.json` files ARE the data. Keep them somewhere sensible
  (a shared drive, a folder per facility, etc.).

## Known limitations

- The control set is representative and illustrative, not a verbatim
  reproduction of either standard — see the in-app disclaimer.
- The NIST CSF 2.0 mapping and the MITRE ATT&CK for ICS technique
  mappings are this tool's own design, not an official published
  crosswalk from NIST, ISA, or MITRE.
- Evidence attachments (photos, PDFs) are embedded directly into the
  saved `.json` file. Attaching many large files will make that file
  bigger — fine for a handful of items per assessment, worth watching
  if attaching dozens.
- Industry-specific wording is a light, conservative substitution (a
  handful of nouns like "OT systems" or "control room" swapped for
  industry equivalents) — not a full per-industry rewrite of all 61
  controls. Compound phrases like "OT-specific" or "OT/ICS" are left
  as-is deliberately, since swapping those tends to produce awkward
  grammar.

## Adding industry benchmarks

Benchmarks are intentionally left blank — there's no reliable published
source giving a clean "average first-time maturity score by industry on
a 1-5 CSF-mapped scale," and inventing numbers would be worse than
leaving this empty. To add your own:

1. Open `index.html` and find `const INDUSTRY_BENCHMARKS = {` near the
   top of the `<script>` section.
2. For the industry you want, replace `null` with a number 1-5 for each
   CSF function, e.g.:
   ```js
   oilgas: { GOVERN:2.1, IDENTIFY:2.4, PROTECT:2.6, DETECT:2.0, RESPOND:1.9, RECOVER:2.2 },
   ```
3. Save and re-deploy. Once any value is non-null for an industry, the
   final report automatically overlays it on the CSF radar chart and
   comparison table for assessments using that industry.

Where to get defensible starting figures: SANS's annual State of ICS/OT
Security Survey and Fortinet's State of Operational Technology and
Cybersecurity Report both publish maturity-adjacent findings, though
neither maps directly to CMMI 1-5 or CSF 2.0 — translating them is a
judgment call worth documenting for whoever uses this later. Note also
that first-time self-assessments tend to run optimistic; Fortinet's own
data showed organizations self-correcting sharply downward (level-4
self-ratings dropped from 49% to 17% year over year) once they measured
more carefully — worth keeping in mind when setting a benchmark others
will be compared against.
