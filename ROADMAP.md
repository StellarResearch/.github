# ROADMAP.md

## FLATLINEDSTAR Ecosystem Development Roadmap

The roadmap is organised into **phases** that roughly follow the dependency order defined in `ARCHITECTURE.md`.  Each phase contains a short description, major deliverables, and the primary owners (repository).  Issues in the top‑priority list (see `TOP_PRIORITY_ISSUES.md`) map to the items below.

| Phase | Repositories Involved | Milestone / Deliverable | Target Completion |
|------|-----------------------|------------------------|-------------------|
| **0 – Foundations** | `onion-sdk` | Core data contracts, versioned Go module, language bindings (Python, TypeScript) | Q4 2026 |
| **1 – Validation Layer** | `onion-validator` | Schema and security‑policy validation library, configurable YAML rules | Q1 2027 |
| **2 – Crawling** | `onion-crawler` | Tor‑aware bounded crawler, same‑origin enforcement, metrics | Q1 2027 |
| **3 – Fingerprinting** | `tech-fingerprint` | Header/TLS/HTML fingerprint engine, rule database, CI for rule updates | Q2 2027 |
| **4 – Entity Extraction** | `entity-extractor` | Mapping fingerprints to high‑level entities, optional ML hook | Q2 2027 |
| **5 – Change Detection** | `change-detector` | Diff engine producing `ChangeEvent` objects, test suite | Q3 2027 |
| **6 – Query Engine** | `query-engine` | In‑memory index + JSON API for search & filters | Q3 2027 |
| **7 – CLI Orchestration** | `onion-cli` | End‑to‑end command line driver that wires the pipeline, SQLite persistence | Q4 2027 |
| **8 – Platform Service** | `onion-platform` | Dockerised API server, Helm chart, web dashboard, health checks | Q4 2027 |
| **9 – Intelligence Engine** | `onion-intelligence` | Rule‑based reasoning over normalized data, risk scoring, insights API | Q1 2028 |
| **10 – Labs / Research** | `onion-labs` | Experimental prototypes, dataset collection, benchmark harnesses | Ongoing |
| **Legacy** | `OnionScan` | Keep as reference implementation; optionally migrate useful components to new repos | – |

### How to Use This Roadmap
1. **Pick an issue** from `TOP_PRIORITY_ISSUES.md` that aligns with the current phase.
2. **Open a PR** with the label `priority:p0` (or appropriate) and the matching `area:` label.
3. **Track progress** via the GitHub Project board (automatically generated from issues with these labels).

### Updating the Roadmap
When a phase is completed, open a PR to modify this file and bump the target dates.  Contributors are encouraged to propose extensions in `onion-labs`.
