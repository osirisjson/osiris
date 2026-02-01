# CONTRIBUTING.md
OSIRIS is developed in public and warmly welcomes contributions of any size: questions, clarifications, examples, reviews and tooling improvements. As an open source project, we believe in giving back to our contributors and are happy to help with guidance on PRs, technical writing and turning any feature idea into a reality.

## Quick principles
- **Quality > volume.** A few well-scoped contributions beat many low-impact issues/PRs.
- **Context matters.** Explain *why* a change is needed, not only *what* you changed.
- **Be kind and patient.** As volunteers reviews, responding and closing issues are conducted alonside regular job mostly over night or weekend.

---

## What you can contribute
### Documentation (recommended starting point)
Documentation improvements are always valuable and usually the fastest to review:
- fixing typos and broken links
- clarifying wording where the spec is ambiguous
- improving explanations and navigation between pages
- expanding “how to validate” or “how to read an OSIRIS document” guidance


### Examples
You can contribute by:
- proposing new example scenarios
- improving readability and consistency of existing examples
- adding comments or accompanying notes that help readers understand why certain fields are present

### Specification and schema
For spec/schema-related contributions:
- open an issue describing the change and motivation
- include a minimal example that demonstrates the problem
- propose the smallest change that preserves compatibility

## How to keep contributions easy to review
- Prefer small, focused PRs.
- Avoid mixing unrelated changes (e.g., refactor + new feature in the same PR).
- Keep language precise (normative statements should use MUST/SHOULD/MAY only where appropriate).
- If a change could affect compatibility, call it out explicitly in the PR description.

## Contributors
This project is currently maintained by a single lead maintainer. Contributors will be listed here and in the maintainer page over time.

## Code of Conduct
Participation in the OSIRIS community is governed by the Code of Conduct.  
[Read the Code of Conduct](https://github.com/osirisjson/osiris/tree/main/CODE_OF_CONDUCT.md)

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