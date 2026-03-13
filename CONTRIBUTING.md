# Contributing to the Persona Interchange Format

Thank you for your interest in contributing to PIF. This is an open standard and community contributions are what make it better.

---

## Ways to contribute

### Reporting issues
Found an ambiguity or error in the spec? Open an issue with:
- The section of the spec affected
- What the current wording says
- What you think it should say and why

### Proposing new fields or extensions
Want to add a field or propose a new extension? Open an issue first before submitting a PR. This gives the community a chance to discuss the proposal before implementation work begins.

A good field proposal includes:
- The use case it solves
- The proposed field name, type, and description
- Example values
- Whether it's required or optional
- Privacy considerations — should it be public or private by default?

### Proposing new extensions
New extensions (beyond the current five) require a stronger case:
- What domain does it serve?
- What platforms would consume it?
- At least 2-3 concrete use cases
- A complete proposed schema

### Fixing documentation
Typos, unclear wording, broken links — PRs welcome, no issue required.

---

## Contribution rules

**All additions must be backwards compatible within a major version.**
New fields must be optional. Existing fields cannot be removed or renamed in a minor version. Implementations must be able to silently ignore unknown fields.

**Breaking changes require a major version.**
If your proposal requires a breaking change, it will be queued for the next major version with appropriate advance notice.

**One concern per issue or PR.**
Keep proposals focused. A PR that adds a new extension AND changes an existing field will be asked to split.

**Plain language.**
The spec is written to be readable by developers who are not identity or security experts. Keep proposals clear and jargon-free where possible.

---

## Pull request process

1. Fork the repository
2. Create a branch named `proposal/your-feature-name`
3. Make your changes
4. Update `CHANGELOG.md` under an `[Unreleased]` heading
5. Submit a PR with a clear description of what changed and why

---

## Versioning

This project follows [Semantic Versioning](https://semver.org/):

- **Patch** (1.0.x) — documentation fixes, clarifications, no schema changes
- **Minor** (1.x.0) — new optional fields or extensions, always backwards compatible
- **Major** (x.0.0) — breaking changes, minimum 24 month support window for previous major version

---

## Code of conduct

Be direct. Be constructive. Assume good intent. Proposals will be evaluated on technical merit and real-world utility — not on who proposes them.

---

## Questions

Open an issue or email [hello@personaspec.com](mailto:hello@personaspec.com).
