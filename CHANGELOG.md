# Changelog

All notable changes to the Persona Interchange Format (PIF) will be documented here.

This project adheres to [Semantic Versioning](https://semver.org/).

---

## [1.0.0] — 2026

### Initial Release

- Core schema: `meta`, `identity`, `voice`, `rules`, `privacy`, `sharing`, `extensions`, `modes`
- Five standard modes: `default`, `professional`, `social`, `minimal`, `full`
- Five extensions: `professional`, `personal`, `social`, `commerce`, `learning`
- Public API specification: `GET /api/v1/{username}`
- Dynamic context system: `?context=`, `?mode=`, `?format=prompt`, `?audience=`
- Transparency field: `always | if_asked | never`
- Security requirements: data minimization, no training, deletion propagation, consent transparency
- Versioning policy: minor versions additive and backwards compatible, unknown fields silently ignored
