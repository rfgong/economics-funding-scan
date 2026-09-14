# Weekly Economics Fellowships & Grants Scan

**Cadence:** Monday, 08:00 America/New_York  
**Objective:** maximize expected research value, not completeness. The researcher-facing report should take about 30 seconds to read.

## 0. Inputs

Read the current repository versions of `funding_roster.json` and `funding_state.json` first. Treat them as read-only canonical inputs for this run. Do not reconstruct missing state from chat memory and do not modify repository state.

Known programs are the union of roster opportunities and `funding_state.programs`. Use the roster's `applicant_profile`, `yale_funding_policy`, and `application_timing_policy` as canonical. Hard-filter current-cycle eligibility.

Official current-cycle evidence overrides cached state whenever a recheck is due or a new candidate is discovered.

## 1. Classify and rank

Assign one primary purpose:

- **A. Protected research time / teaching relief** — stipend/completion/residential fellowships or equivalent arrangements that can plausibly remove or defer a remaining eligible teaching obligation or create comparable protected research time.
- **B. Research funding & access** — incremental data, RAs, surveys, fieldwork, compute, software, restricted/admin data, or other research inputs.
- **C. Flexible / unrestricted support** — broad dissertation/student support that does not clearly produce teaching relief.
- **D. Travel, presentation & training** — conference travel, research visits, workshops, summer schools, or training.

Apply Yale funding rules mechanically. Do not treat stipend support as additive cash when Yale offsets it. Research/travel/equipment support is generally additive. Give teaching-relief value only when an award can reduce a remaining financial/waivable teaching term; if unclear, mark the effect `UNRESOLVED`.

Rank actionable candidates globally by expected research value using: protected time, incremental usable funding/access, project fit, credible competitiveness evidence, application burden/material reuse, and career/network value. Do not fabricate numerical EV scores or acceptance probabilities.

Main list: at most **5** opportunities total. Alternates: at most **2** genuine near-misses.

## 2. Action window

For each viable opportunity:

`operative_deadline = internal Yale deadline if required and known; otherwise sponsor deadline`

If any required third-party recommendation/support/nomination action applies, use an action date at least **2 calendar months before the operative deadline**. This rule overrides any shorter roster or generic lead time; use a longer lead if current-cycle requirements warrant it.

Otherwise:

`action_date = operative_deadline - lead_weeks`

`lead_weeks` precedence: verified current-cycle requirements > roster `default_lead_weeks` > generic prior:

- full proposal + budget/institutional routing: 8 weeks
- fellowship + statement but no recommender: 5 weeks
- short dissertation/flexible grant: 2 weeks
- travel/training: 1 week

If limited submission or internal nomination applies, find the Yale deadline when possible; otherwise flag `INTERNAL DEADLINE UNVERIFIED` and act conservatively.

Surface an item when its action date is within the next **3 weeks**, it is rolling and actionable, or it just opened/materially changed and merits attention. Mark `LATE` when already inside the preferred preparation window.

## 3. State and search workflow

Before browsing:

1. If an `open` program's operative deadline has passed, force a recheck regardless of `recheck_after`; if no new cycle exists, treat it as closed for the current run.
2. For an open program, treat any cached `recheck_after` later than its operative deadline as stale and force a recheck by the deadline.
3. Use `recheck_after` as a search-suppression prior only. If it is due, recheck the program for the current run. Do not persist a new `recheck_after`; repository maintenance is outside this automation.
4. If an open program is still valid, its deadline has not passed, `recheck_after > today`, and enough information exists for this week's decision, **do not search it again**; compute from state.
5. Suppress `closed_recurring`, `dormant`, and `discontinued` programs until `recheck_after` unless new official evidence indicates a change.

Default recheck timing priors when no better timing is known:

- open: +14 days; +7 days if deadline is within 30 days; clamp to operative deadline
- closed recurring: +60 days or expected opening, whichever is earlier
- dormant: +90 days
- discontinued: +180 days
- unverified: +14 days

These priors govern this run's search decisions only; do not write them back to repository state.

### Discovery budget

After due state rechecks, use `discovery_sources` in priority order.

- Normal weekly run: check at most **10 discovery pages** total.
- Scan `weekly` sources first; do not compensate with generic web searching after the budget is exhausted.
- Scan quarterly seed pages on the first successful run in January, April, July, and October if `funding_state.discovery_checks.quarterly_last_run` is not already in the current quarter; check at most **8** quarterly pages. Treat the stored value as a prior only; do not update repository state.
- Aggregators are discovery-only. Verify any surfaced candidate on the sponsor's official page.
- Do not keep searching merely to fill categories.

If a newly discovered program reaches the main list or ALTERNATES, it may appear in the researcher-facing report when verified and sufficiently valuable. Do not modify `funding_state.json` or `funding_roster.json`.

## 4. Applications and batching

Use `funding_state.applications` to enforce exhausted eligibility/reapplication rules and recognize reusable materials.

Treat stored application status as read-only. If the user has stated an intention to apply, or a `THIS WEEK` action explicitly commits to application preparation, use that information for the current report but do not persist it to repository state.

Keep public output minimal and non-sensitive. Never include proposal text, letters, credentials, private correspondence, reviewer feedback, or sensitive personal information.

When two or more viable applications substantially reuse materials, mention one short `WRITING SPRINT` and account for the lower joint burden before ranking.

## 5. Verification

Use official sponsor/program pages for current-cycle eligibility, deadlines, award terms, and requirements. Never infer that an annual program is active because it ran previously. An important candidate whose current cycle cannot be verified may appear only as `UNVERIFIED`, never as open.

For data/access awards, check Yale substitutes only when they could materially change ranking. Use the stored Yale Combined Award Policy unless its scheduled recheck is due or contrary official evidence appears.

**Every named opportunity in the researcher-facing report must include a clickable link to its official source.**

## 6. Researcher-facing output

No preamble or opening quote. Always show all four purpose sections A–D.

If a section has nothing actionable, write one short line: `None actionable this week.` You may append one especially important watch item in the same line if useful.

For each surfaced opportunity, use **at most 2 concise sentences**:

`**#N [Opportunity](official URL)** — amount/value; deadline; **act by DATE**.`  
Then one sentence with fit, key requirement, or ranking rationale.

Then, only if useful:

### CHANGES
At most **3 one-line** new openings, material changes, or important watch items.

### THIS WEEK
At most **3 short actions**. If none are worthwhile, write: `Nothing is sufficiently high-EV and actionable this week.`

Do not show implementation details, state JSON, roster JSON, repository-maintenance suggestions, or debugging in the researcher-facing report.
