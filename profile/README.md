# FLATLINED

**Open-source security research, network privacy instrumentation, and verifiable systems engineering.**

FLATLINED builds inspectable, evidence-based security software and measurement tooling. We focus on tools that help operators and researchers evaluate, harden, and defend decentralized and privacy-preserving network infrastructure.

---

## Mission

Our objective is to deliver dependable, transparent, and deterministic software for security engineers and system administrators. In security and privacy, vague metrics and single regex hits lead to false alarms and misallocated resources. FLATLINED builds systems that treat observations as raw evidence, enforce strict confidence scoring, and prioritize correctness over hype.

---

## What We Build

We focus on engineering domains that require rigor, privacy awareness, and deterministic execution:

- **Security Observability & Auditing**  
  Automated analyzers and scanners that detect operational security leaks, misconfigurations, and cryptographic drift in network services.

- **Privacy & Anonymity Network Instrumentation**  
  Defensive tools designed to work safely across overlay networks such as Tor, verifying service privacy without risking traffic analysis or unnecessary deanonymization vectors.

- **Verifiable Systems & Tooling**  
  Lightweight command-line utilities, daemons, and storage engines designed with minimal external dependencies, single-binary distribution, and verifiable local persistence.

- **Applied Open-Source Research**  
  Empirical investigations into service topology, metadata correlation, and defensive architecture for decentralized protocols.

---

## Featured Projects

### [OnionScan (OnionSec)](https://github.com/FLATLINEDSTAR/OnionScan)

> Security observability and multi-vector correlation engine for Tor onion services.

- **What it does:** Crawls authorized `.onion` services through a local Tor SOCKS5 proxy, executes modular analyzers (HTTP headers, OPSEC identifiers, TLS configuration, robots/sitemaps, JavaScript source maps, API routes, and image EXIF metadata), correlates multi-point evidence into confidence-weighted findings, and tracks configuration drift over time in a local SQLite store.
- **Who it is for:** Operators of Tor hidden services, privacy researchers, and defensive security teams performing authorized audits.
- **Why it exists:** Traditional scanners often report raw regex matches as definitive vulnerabilities. OnionScan treats every observation as evidence that must be correlated to produce actionable, scored findings with minimal noise.
- **Current status:** **Active Development** (MVP Hardening & API/UI Integration).
- **Technology Stack:** Go 1.22+, SQLite3, Next.js / TypeScript, Cytoscape.js, native dependency-free SOCKS5 implementation.
- **Contribution Opportunities:** Implementing new modular analyzers, enhancing crawler boundary rules, refining correlation algorithms, and expanding test coverage.

---

## Engineering Principles

1. **Correctness Before Complexity**  
   We favor clear, testable, and auditable code over elaborate abstractions. If a component cannot be verified simply, its design must be simplified.

2. **Security by Design**  
   All network-interacting tools enforce strict boundary checks, timeouts, memory bounds, and same-origin protections. We treat untrusted network inputs with extreme skepticism.

3. **Evidence Over Assumption**  
   Raw observations are indicators, not conclusions. Security findings must cite verifiable evidence artifacts with transparent confidence scoring.

4. **Minimal External Dependencies**  
   We avoid pulling in sprawling dependency trees where standard library primitives suffice. Fewer dependencies mean a smaller attack surface and more deterministic builds.

5. **Deterministic Reproducibility**  
   Builds, tests, and scans must produce consistent, reproducible outcomes across platforms without hidden global state or environment assumptions.

6. **Documentation as an Engineering Requirement**  
   Code is incomplete without comprehensive architecture documentation, configuration specifications, and usage examples.

7. **Defensive Mandate**  
   All tools produced by FLATLINED are strictly intended for defensive evaluation, authorized operator auditing, and reproducible research. We do not develop or host offensive exploitation tooling.

---

## Project Status Taxonomy

We maintain a strict status classification across all repositories to set clear expectations:

| Status | Definition |
| :--- | :--- |
| **Stable** | Production-grade software with stable APIs, full test coverage, and semantic versioning. |
| **Active Development** | Functional software under active enhancement; APIs may evolve with documented deprecations. |
| **Experimental** | Exploratory research prototypes and proof-of-concepts; not ready for production deployment. |
| **Archived** | Read-only repositories retained for historical reference or research documentation. |

---

## Open Source & Contributing

We welcome contributions from engineers and researchers of all experience levels:

- **Bug Reports:** Detailed reports with reproduction steps, environment details, and execution logs.
- **Analyzers & Detectors:** Modular scanning logic adhering to the `internal/analyzer.Analyzer` interface.
- **Core Engine Improvements:** Performance optimizations in crawler memory management, connection pooling, and graph generation.
- **Frontend & Visualization:** Enhancements to interactive network graphs, responsive data tables, and accessibility.
- **Documentation & Testing:** Expanding test coverage for edge cases, network timeouts, and forensic reporting.

To get started, browse the [good first issue](https://github.com/FLATLINEDSTAR/OnionScan/labels/good%20first%20issue) label on active repositories and review our [Contributing Guide](https://github.com/FLATLINEDSTAR/OnionScan/blob/main/CONTRIBUTING.md).

---

## Security & Responsible Disclosure

Security is fundamental to our work. We encourage responsible disclosure of any potential vulnerabilities discovered in our projects.

- Please review each repository's `SECURITY.md` for specific reporting procedures and response timelines.
- Never file public issues containing unpatched vulnerability details or exploit code.
- Report vulnerabilities privately through [GitHub Private Vulnerability Reporting](https://github.com/FLATLINEDSTAR/OnionScan/security/advisories/new) where enabled, or contact maintainers through repository security advisories.

---

## License

Each project under the FLATLINED organization is licensed independently under standard OSI-approved licenses (primarily the MIT License). Refer to the `LICENSE` file within individual repositories for governing terms.
