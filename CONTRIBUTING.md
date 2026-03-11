# Contributing to Persona Spec

Thanks for your interest in contributing. The goal is to keep the spec simple, portable, and useful — contributions that serve that goal are welcome.

## What we welcome

**Spec improvements**
- New optional fields that make personas richer
- Edge cases the spec doesn't handle
- Backwards-compatible additions
- Clarifications to existing field definitions

**Tooling**
- Validators in any language (Python, Go, Ruby, etc.)
- CLI tools for creating or validating `.persona` files
- IDE extensions and plugins

**Examples**
- Real `.persona` files (your own — don't submit files for other people)
- Minimal examples for specific use cases
- Industry-specific templates

**Importers and exporters**
- LinkedIn profile → `.persona` converter
- Resume PDF → `.persona` converter
- GitHub profile → `.persona` enrichment

**Documentation**
- Translations
- Integration guides
- Tutorials for building compatible sites

## What we don't accept

- Breaking changes to required fields without a major version proposal
- Fields that serve a single implementation rather than the general spec
- Contributions without a clear use case

## How to propose a spec change

1. Open an issue describing the problem you're solving
2. Wait for discussion before opening a PR
3. For major changes, write an RFC — a markdown document describing the change, rationale, and migration path
4. Final decisions rest with the maintainer

## Submitting a PR

1. Fork the repo
2. Create a branch: `feature/your-change` or `fix/your-fix`
3. Make your changes
4. Open a PR with a clear description of what and why

## Contributor License Agreement

By submitting a contribution you agree that your contribution may be used under the terms of the MIT license and that the maintainers retain the right to use contributions in commercial implementations built on this spec.

## Code of conduct

Be direct. Be constructive. No personal attacks.

---

Questions? Open an issue or email hello@personaspec.org
