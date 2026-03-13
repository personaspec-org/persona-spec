# Persona Interchange Format (PIF)

**An open standard for portable AI identity.**

---

Every AI starts from zero. You explain yourself to every chatbot, every recommendation engine, every personalization system. Every single time.

PIF fixes that at the protocol level. A `.persona` file travels with you. Any AI-powered platform that supports PIF can understand who you are — your voice, your interests, your preferences, your rules — instantly, with your consent.

**Build your persona once. Every AI that meets you already knows you.**

→ [personaspec.com](https://personaspec.com) — build and host your persona  
→ [personaspec.org](https://personaspec.org) — full specification and documentation

---

## What it looks like

```json
{
  "version": "1.0",
  "identity": {
    "name": "Alex Chen",
    "tagline": "Product designer who codes",
    "location": "San Francisco, CA"
  },
  "voice": {
    "tone": "Casual",
    "style": "Concise",
    "examples": [
      {
        "prompt": "tell me about yourself",
        "response": "I design products and write just enough code to be dangerous."
      }
    ]
  },
  "rules": {
    "transparency": "if_asked",
    "behaviors": ["no_commitments", "deflect_personal"]
  },
  "extensions": {
    "personal": {
      "interests": ["Design systems", "Photography", "Coffee"]
    }
  }
}
```

That's a `.persona` file. A structured, portable, user-owned identity file that any AI system can read.

---

## For developers

### One API call. Ready-to-use system prompt.

```bash
GET https://personaspec.com/api/v1/alex-chen?format=prompt
```

Returns:

```
You are an AI persona representing Alex Chen, a product designer
who codes based in San Francisco, CA.
Tone: Casual. Style: Concise.
When asked "tell me about yourself" they responded: "I design
products and write just enough code to be dangerous."
Interests: Design systems, Photography, Coffee.
If sincerely asked whether you are an AI, acknowledge it honestly.
Only state facts explicitly present in this prompt.
Deflect anything not covered naturally in character.
You are not a generic assistant. You are Alex Chen.
```

Drop that string directly into your system prompt. Your AI now knows exactly who it's talking to.

### With context

```bash
GET https://personaspec.com/api/v1/alex-chen
  ?format=prompt
  &mode=professional
  &context=job_application
  &platform=YourJobBoard
  &audience=senior design recruiters
```

The same persona, tailored to the context your platform provides.

### Standard context values

`general` `professional` `job_application` `networking` `social` `dating` `customer_support` `education` `creative` `commerce`

### Response codes

| Code | Meaning |
|------|---------|
| `200` | Success |
| `404` | Not found or deleted — purge any cached data |
| `403` | Public API disabled by persona owner |
| `429` | Rate limited — 100 requests/hour unauthenticated |

---

## Why implement PIF?

**For your users** — they arrive already personalized. No onboarding friction. No re-explaining themselves. The AI knows them from message one.

**For your product** — personalized experiences convert and retain better. A user who brought their persona is a more invested user.

**For the ecosystem** — PIF is an open standard. Implementing it signals that your platform respects user data ownership and portability. That matters to the users who care most about privacy.

---

## Security requirements

Implementations must:

- **Request only necessary scopes** — don't request professional data for a recipe site
- **Cache for max 24 hours** — personas change, respect updates
- **Never train on persona data** — it's for runtime personalization only
- **Respect deletion** — `404` means the user deleted their persona, purge within 48 hours
- **Disclose usage** — tell your users their PersonaSpec persona is being used
- **Honor revocation immediately** — OAuth tokens invalidated on revocation

---

## File format

| Property | Value |
|----------|-------|
| Extension | `.persona` |
| MIME type | `application/persona+json` |
| Encoding | UTF-8 |
| Format | JSON |

---

## Versioning

Minor versions (1.0 → 1.1) are always backwards compatible. New fields are always optional. **Implementations must silently ignore unknown fields.**

Major versions (1.0 → 2.0) may introduce breaking changes with a minimum 24 month notice period.

---

## Examples

- [`examples/minimal.persona`](examples/minimal.persona) — identity and voice only
- [`examples/full.persona`](examples/full.persona) — all extensions populated

---

## Full specification

→ [SPEC.md](SPEC.md)

---

## Contributing

PIF is an open standard. Issues, proposals, and pull requests are welcome.

- Open an issue to propose a new extension or field
- Submit a PR with schema changes — all additions must be backwards compatible
- Let us know you've implemented PIF so we can add you to the ecosystem list

---

## License

MIT — free to implement in any product, commercial or otherwise.

© 2026 PersonaSpec — [personaspec.org](https://personaspec.org)
