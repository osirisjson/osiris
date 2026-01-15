# CONTRIBUTING.md
Thanks for your interest in contributing to OSIRIS! This project is maintained by volunteers; please help keep the signal high and the workload sustainable.

## Quick principles
- **Quality > volume.** A few well-scoped contributions beat many low-impact issues/PRs.
- **Context matters.** Explain *why* a change is needed, not only *what* you changed.
- **Be kind and patient.** As volunteers reviews, responding and closing issues are conducted alonside regular job mostly over night or weekend.

---

## Ways to contribute
### 1. Improve docs (high impact)
- Fix unclear sections that block users
- Add examples that are minimal, correct and validated
- Add diagrams or troubleshooting notes

### 2. Report bugs (actionable issues only)
Before opening an issue:
- Search existing issues/PRs first
- Confirm it’s reproducible on the latest version (or `main`)
- Reduce to a minimal reproducible case

**A good bug report includes:**
- What you expected to happen
- What actually happened (include exact error output)
- Steps to reproduce (minimal)
- Environment (OS, versions, runtime, etc.)
- Any relevant logs/config/files (redact secrets)

### 3. Propose enhancements
For anything non-trivial, open a discussion first (or an issue) that includes:
- Problem statement
- Why it matters (who benefits / what pain it removes)
- Proposed solution(s)
- Tradeoffs and alternatives

### 4. Code contributions
- Pick an existing issue or open one first for alignment
- Keep PRs small and focused (one concern per PR)
- Add/update tests where applicable
- Update docs/examples if behavior changes

---

## AI-assisted contributions (allowed, but be responsible)
Using AI as a helper is fine. What’s not fine is **mass-producing issues/PRs without understanding the project**.

**If you use AI:**
- You are responsible for correctness and scope
- Read and understand the code/spec you’re changing
- Validate outputs (tests, linters, schema validation, examples)
- Don’t open “speculative” issues you haven’t reproduced
- Avoid drive-by refactors that rename/reformat large areas without purpose

**Maintainers may close without extended discussion:**
- Duplicate issues
- Issues missing reproducible steps/context
- PRs that are clearly low-effort, auto-generated or not aligned with project direction

---

## Getting started
1. Fork the repo and clone **your fork**
2. Create a branch in your fork:
   - `feat/<short-topic>` or `fix/<short-topic>`
3. Make your changes in small commits
4. Open a Draft PR early if you want feedback
5. Mark ready when:
   - Tests pass
   - Docs/examples updated
   - PR description is complete

---

## Pull Request checklist
Please include in your PR description:

- **What** does this PR change?
- **Why** is it needed? (link issue/discussion)
- **How** did you test it?
- Any breaking changes?

**Checklist**
- [ ] I linked the relevant issue/discussion (or explained why none exists)
- [ ] I kept scope focused and avoided unrelated changes
- [ ] I added/updated tests (if applicable)
- [ ] I updated docs/examples (if applicable)
- [ ] I ran formatting/linking/tools used by this repo (if applicable)

---

## Commit messages (recommended)
Use clear messages, optionally Conventional Commits:
- `feat: ...`
- `fix: ...`
- `docs: ...`
- `chore: ...`

---

## Licensing
By contributing, you agree that your contributions are licensed under the project’s license.

---

## GSoC (or similar programs)
If you’re contributing with an application in mind:
- Focus on **one meaningful area**
- Show that you can **communicate clearly**, **ship small improvements**, and **iterate on feedback**
- A short design doc or proposal discussion beats many low-impact PRs