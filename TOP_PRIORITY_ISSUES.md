# TOP PRIORITY ISSUES

## 1️⃣ Define core data contracts in **onion-sdk**

**Problem**
The ecosystem lacks a single source of truth for shared types (e.g., `OnionTarget`, `OnionObservation`, `Technology`, `Entity`, `ChangeEvent`, `SearchQuery`, `SearchResult`). Without these, each repository ends up duplicating definitions, leading to version drift and circular dependencies.

**Goal**
Create a Go module that defines all core structs, interfaces, and JSON‑serialisation helpers. Publish the module and generate language bindings.

**Scope**
- Add `types.go` with all structs and validation tags.
- Add `go.mod` (module `github.com/FLATLINEDSTAR/onion-sdk`).
- Write unit tests for (un)marshalling.
- Add CI step that runs `go test ./...`.

**Proposed approach**
1. Draft the structs based on the architecture diagram.
2. Implement `Validate()` methods for each type.
3. Add a `make generate` target that runs `gopy` (Go→Python) and `ts-proto` (Go→TS).

**Acceptance criteria**
- All structs compile and pass `go vet`.
- Python wheel and npm package can be built from the generated artifacts.
- CI reports 100 % test coverage for the module.

**Tests**
- Table‑driven tests for JSON round‑trip of each struct.
- Integration test that imports the generated Python package and creates an instance.

**Dependencies**
- None (foundation layer).

**Contributor notes**
- Files: `types.go`, `types_test.go`, `go.mod`.
- Difficulty: **Medium**.
- Priority: **P0**.

---

## 2️⃣ Implement validation layer in **onion-validator**

**Problem**
Raw observations from the crawler are not checked for schema correctness or security policy violations, risking downstream errors.

**Goal**
Provide a reusable library that validates `OnionObservation` objects against the SDK schema and runs security‑policy checks.

**Scope**
- Add `validator.go` with `ValidateObservation(obs onion-sdk.OnionObservation) error`.
- Implement header sanitisation and TLS policy checks.
- Add configurable rule set (YAML).

**Proposed approach**
1. Import data models from `onion-sdk`.
2. Use `go-playground/validator` for struct validation.
3. Write custom validators for security rules.

**Acceptance criteria**
- `ValidateObservation` returns an error on missing fields or disallowed values.
- Unit tests cover all rule branches.
- CI runs `go test -cover`.

**Tests**
- Table‑driven tests with valid/invalid observations.
- Fuzz test on header map.

**Dependencies**
- Requires `onion-sdk` (already defined).

**Contributor notes**
- Files: `validator.go`, `validator_test.go`.
- Difficulty: **Medium**.
- Priority: **P0**.

---

## 3️⃣ Build bounded Tor crawler in **onion-crawler**

**Problem**
There is currently no component that can fetch pages from `.onion` services safely and within defined limits.

**Goal**
Implement a Tor‑aware HTTP client and a breadth‑first crawler with configurable limits (max pages, max body size, time budget).

**Scope**
- Add `crawler.go` exposing `Crawl(target onion-sdk.OnionTarget, opts CrawlOptions) (CrawlResult, error)`.
- Implement Tor SOCKS5 dialer.
- Enforce same‑origin policy.

**Proposed approach**
1. Use `golang.org/x/net/proxy` for SOCKS5.
2. Create a queue with concurrency limiter.
3. Store raw responses in a SQLite store (reuse `onion-sdk` types).

**Acceptance criteria**
- Successful crawl of a test `.onion` service (provided via CI mocks).
- No DNS leaks (verified via mock).
- Rate limits respected.

**Tests**
- Unit tests with a local mock Tor proxy.
- Integration test crawling a fixture site.

**Dependencies**
- `onion-sdk` for target definition.
- `onion-validator` for post‑crawl validation.

**Contributor notes**
- Files: `crawler.go`, `crawler_test.go`.
- Difficulty: **High**.
- Priority: **P0**.

---

## 4️⃣ Implement technology fingerprinting in **tech-fingerprint**

**Problem**
The pipeline lacks a way to turn raw observations into concrete technology identifiers.

**Goal**
Create a rule‑based fingerprint engine that extracts `Technology` structs from HTTP headers, TLS data, and HTML meta tags.

**Scope**
- Add `fingerprint.go` with `Fingerprint(obs onion-sdk.OnionObservation) ([]Technology, error)`.
- Load fingerprint rules from a YAML file.

**Proposed approach**
1. Define a simple rule schema (field, regex, confidence).
2. Implement matcher that iterates over rules.
3. Cache compiled regexes.

**Acceptance criteria**
- At least 10 common technologies (e.g., nginx, Apache, WordPress) are correctly identified in CI fixtures.
- Unit tests achieve 90 % rule coverage.

**Tests**
- Table‑driven tests for header‑only and HTML‑only observations.

**Dependencies**
- `onion-sdk` for `Technology` type.

**Contributor notes**
- Files: `fingerprint.go`, `rules.yaml`, `fingerprint_test.go`.
- Difficulty: **Medium**.
- Priority: **P1**.

---

## 5️⃣ Build entity extraction in **entity-extractor**

**Problem**
Higher‑level entities (CMS, frameworks) are not derived from fingerprint data.

**Goal**
Map `Technology` results to `Entity` objects using a rule set.

**Scope**
- Add `extractor.go` with `ExtractEntities(obs onion-sdk.OnionObservation) ([]Entity, error)`.
- Provide a default rule file (`entities.yaml`).

**Proposed approach**
1. Reuse fingerprint output.
2. Apply mapping rules (e.g., `WordPress` → `CMS:WordPress`).

**Acceptance criteria**
- Entities for at least 5 common stacks are extracted from CI test data.

**Tests**
- Table‑driven tests for various fingerprint combos.

**Dependencies**
- `tech-fingerprint` and `onion-sdk`.

**Contributor notes**
- Files: `extractor.go`, `entities.yaml`, `extractor_test.go`.
- Difficulty: **Medium**.
- Priority: **P1**.

---

## 6️⃣ Implement change detection in **change-detector**

**Problem**
There is no mechanism to compute diffs between successive observations.

**Goal**
Provide a library that, given two sets of entities/technologies, produces `ChangeEvent` objects (Added, Removed, Modified).

**Scope**
- Add `detector.go` with `DetectChanges(old, new []Entity) []ChangeEvent`.
- Export JSON‑serialisable diff reports.

**Proposed approach**
1. Index entities by unique ID.
2. Compare sets and generate events.

**Acceptance criteria**
- Correctly detects additions/removals in CI fixtures.

**Tests**
- Unit tests with mock old/new data.

**Dependencies**
- `entity-extractor` and `onion-sdk`.

**Contributor notes**
- Files: `detector.go`, `detector_test.go`.
- Difficulty: **Medium**.
- Priority: **P2**.

---

## 7️⃣ Provide search & query API in **query-engine**

**Problem**
Consumers have no way to query stored observations, entities, or change events.

**Goal**
Implement a simple in‑memory index with a REST endpoint (or Go library) exposing `Search(query SearchQuery) ([]SearchResult, error)`.

**Scope**
- Add `engine.go` with indexing functions.
- Expose JSON API via `net/http` (optional).

**Proposed approach**
1. Use a map‑based index for MVP.
2. Add unit tests for query semantics.

**Acceptance criteria**
- Queries return correct results for seeded test data.

**Tests**
- Table‑driven tests for various query filters.

**Dependencies**
- `onion-sdk` for query structs.

**Contributor notes**
- Files: `engine.go`, `engine_test.go`.
- Difficulty: **Medium**.
- Priority: **P2**.

---

## 8️⃣ Add CLI orchestration in **onion-cli**

**Problem**
End users cannot run end‑to‑end scans without writing custom scripts.

**Goal**
Create a command‑line tool that chains crawler → validator → fingerprint → extractor → change‑detector → query, storing results in SQLite.

**Scope**
- Add `main.go` with sub‑commands (`scan`, `report`, `diff`).
- Use `cobra` for flag parsing.

**Proposed approach**
1. Wire together existing libraries.
2. Persist results via `onion-sdk` models.

**Acceptance criteria**
- `onion-cli scan` completes a full pipeline on a test target.
- `onion-cli report` prints a summary.

**Tests**
- End‑to‑end test using a mock `.onion` service.

**Dependencies**
- All downstream libraries.

**Contributor notes**
- Files: `cmd/root.go`, `cmd/scan.go`, etc.
- Difficulty: **High**.
- Priority: **P1**.

---

## 9️⃣ Build full platform service in **onion-platform**

**Problem**
There is no production‑ready deployment artifact (Docker image, Helm chart, API server).

**Goal**
Provide a containerised API service that runs scheduled scans, stores data, and serves a web UI.

**Scope**
- Add `cmd/server.go` (Echo/Fiber HTTP server).
- Add `Dockerfile` and `helm/` chart directory.
- Integrate `onion-intelligence` for analysis.

**Proposed approach**
1. Define OpenAPI spec.
2. Implement handlers that call the SDK.
3. Write CI to build Docker image.

**Acceptance criteria**
- Docker image builds and starts with default config.
- Health endpoint returns 200.

**Tests**
- Integration test using `docker compose` (API + SQLite).

**Dependencies**
- All libraries + `onion-intelligence`.

**Contributor notes**
- Files: `cmd/server.go`, `Dockerfile`, `helm/values.yaml`.
- Difficulty: **High**.
- Priority: **P1**.

---

## 🔟 Define core reasoning engine in **onion-intelligence**

**Problem**
No concrete implementation of the intelligence layer exists.

**Goal**
Create a rule‑engine that consumes normalized observations and emits risk scores / recommendations.

**Scope**
- Add `engine.go` with `RunInference(data []onion-sdk.OnionObservation) ([]Insight, error)`.
- Provide a simple rule DSL (YAML).

**Proposed approach**
1. Load rules from `rules.yaml`.
2. Apply them to observations.

**Acceptance criteria**
- At least 3 sample rules produce deterministic insights on test data.

**Tests**
- Unit tests covering each rule.

**Dependencies**
- `onion-sdk` and upstream data.

**Contributor notes**
- Files: `engine.go`, `rules.yaml`, `engine_test.go`.
- Difficulty: **High**.
- Priority: **P2**.

---

## 📚 Organization‑wide contribution docs & label taxonomy (in `.github`)

**Problem**
Contributors lack a consistent set of templates and label definitions, leading to inconsistent triage.

**Goal**
Add `CONTRIBUTING.md`, `LABEL_TAXONOMY.md`, issue/PR templates, and a high‑level `ROADMAP.md`.

**Scope**
- Create `.github/CONTRIBUTING.md`.
- Create `.github/LABEL_TAXONOMY.md` (areas, types, difficulty, priority).
- Create `.github/ISSUE_TEMPLATE/issue_template.md` and `.github/PULL_REQUEST_TEMPLATE/pull_request_template.md`.
- Add `.github/ROADMAP.md` summarising phases 0‑12.

**Proposed approach**
1. Draft markdown files with clear headings.
2. Commit them to the `.github` repo.

**Acceptance criteria**
- GitHub automatically shows the issue and PR templates.
- Labels can be created manually following the taxonomy.

**Dependencies**
- None.

**Contributor notes**
- Files: see above.
- Difficulty: **Easy**.
- Priority: **P0**.

---

*End of top‑priority issue list.*
