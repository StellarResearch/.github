# LABEL_TAXONOMY.md

## Label Taxonomy for FLATLINEDSTAR Repositories

We use a small, consistent set of labels across all repositories.  Labels are **case‑sensitive** and should be created exactly as listed.

### Area (Component)
- `area:sdk`
- `area:validator`
- `area:crawler`
- `area:fingerprint`
- `area:extractor`
- `area:change-detector`
- `area:query-engine`
- `area:cli`
- `area:platform`
- `area:intelligence`
- `area:labs`
- `area:legacy`  (for OnionScan)

### Type
- `type:bug`
- `type:feature`
- `type:enhancement`
- `type:documentation`
- `type:performance`
- `type:security`

### Difficulty
- `difficulty:easy`
- `difficulty:medium`
- `difficulty:hard`

### Priority
- `priority:p0` – must be done before any other work (foundation layers).
- `priority:p1` – high priority, next after p0.
- `priority:p2` – medium priority, can be done in parallel.
- `priority:p3` – low priority or nice‑to‑have.

### Good First Issue / Help Wanted
- `good first issue`
- `help wanted`

### Usage Guidance
- Every new issue must have **exactly one** `area:` label.
- Every issue must have at least one `type:` label.
- Add **one** `priority:` label.
- Include a `difficulty:` label to help contributors assess effort.
- Add `good first issue` or `help wanted` when the issue is suitable for newcomers.

These labels enable clear triage, automation (e.g., project boards), and easy filtering for contributors.
