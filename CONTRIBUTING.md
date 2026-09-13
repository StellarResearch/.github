# CONTRIBUTING.md

## How to Contribute
We welcome contributions from anyone interested in strengthening the FLATLINEDSTAR ecosystem. Please follow these steps:

1. **Read the docs** – Start with the `README.md` of the repository you want to work on and the organization‑wide documentation in `.github/` (especially `ARCHITECTURE.md` and `ROADMAP.md`).
2. **Pick an issue** – Look for issues labelled `good first issue` or `help wanted`.  The top‑priority list lives in `.github/TOP_PRIORITY_ISSUES.md`.
3. **Fork & clone** – Fork the repository, then clone your fork locally.
4. **Create a branch** – Use a descriptive branch name, e.g. `feat/sdk‑data‑contracts`.
5. **Develop** – Follow the language‑specific style guides (see `DEVELOPMENT.md` if present).  Run the test suite (`go test ./...` for Go repos, `pytest` for Python, `npm test` for TypeScript) before committing.
6. **Commit messages** – Write clear, atomic commit messages.  Follow the conventional commits format (`feat:`, `fix:`, `docs:` …).
7. **Open a Pull Request** – Push your branch to your fork and open a PR against the `main` branch of the upstream repo.
8. **PR Checklist** – Fill out the PR template (see `PULL_REQUEST_TEMPLATE.md`).  Ensure:
   - All CI checks pass.
   - New code has unit tests with ≥ 80 % coverage.
   - Documentation is updated if needed.
9. **Review** – Maintainters will review, request changes, and merge when ready.

## Code of Conduct
Please read and follow the `CODE_OF_CONDUCT.md` located in the root of each repo.

## Reporting Security Issues
See `SECURITY.md` for the responsible disclosure process.
