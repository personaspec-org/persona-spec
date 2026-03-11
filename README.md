# Persona Spec

> The open standard for portable AI personas.

## What is a `.persona` file?

A `.persona` file is a structured, portable representation of a person — their identity, voice, values, and rules for how they want to be represented across the internet.

Think of it like a Gmail account, but for your identity. You create it once, and it works everywhere.

## The Problem

The web knows you as a cookie. A device fingerprint. A demographic guess. Every platform makes you rebuild your profile from scratch. Every AI tool starts with zero context about who you are.

There is no standard for a person to declare who they are, how they communicate, and how they want to be represented — across the entire web.

## The Solution

A `.persona` file is that standard.

- **You own it** — it lives on your device, not on a platform
- **You control it** — sites request access, you approve or deny per extension
- **It travels with you** — import it anywhere that supports the spec
- **It works with AI** — any AI system can load your persona and represent you accurately
- **It works for any context** — job searching, social, commerce, dating, education, and beyond

## Structure

The spec has two layers:

**Core** — universal fields every `.persona` file contains:
- `identity` — who you are
- `voice` — how you communicate
- `rules` — what you share and what you don't

**Extensions** — optional domain-specific blocks sites can request:
- `professional` — work history, skills, availability
- `personal` — interests, values, personality
- `social` — content preferences, engagement style
- `commerce` — purchasing preferences
- `learning` — knowledge level, learning goals
- Custom extensions for any domain

## What it looks like

```json
{
  "version": "1.0",
  "meta": {
    "id": "alex-rivera",
    "created": "2026-01-01",
    "visibility": "public"
  },
  "identity": {
    "name": "Alex Rivera",
    "location": "Austin, TX",
    "tagline": "Building things that matter",
    "language": "en"
  },
  "voice": {
    "tone": "direct, warm, curious",
    "style": "conversational, no corporate speak"
  },
  "rules": {
    "privacy": {
      "hide_phone": true,
      "hide_system_prompt": true
    },
    "sharing": {
      "default": "deny",
      "require_approval": true
    }
  },
  "extensions": {
    "professional": {
      "availability": { "status": "actively-looking" },
      "skills": ["Go", "Python", "Kubernetes"]
    }
  }
}
```

See the [full spec](SPEC.md), a [minimal example](examples/minimal.persona), and a [complete example](examples/full.persona).

## How it gets used

**Personal websites** — import your `.persona` and get an AI chatbot that answers as you

**Browser extension** — your persona travels with you as you browse, auto-filling forms and declaring your identity to AI-powered sites on your terms

**Job applications** — auto-fill applications and professional profiles from your `professional` extension

**AI tools** — load your persona so any AI tool instantly knows your context without re-explaining yourself every time

**Any platform** — sites request the extensions they need, you approve what gets shared

## Implementations

| Project | Type | URL |
|---------|------|-----|
| personaspec.com | Hosted persona builder + chatbot platform | Coming soon |

Built something with persona-spec? Open a PR to add it here.

## Spec version

Current: `1.0`

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). The spec is open — tooling, validators, importers, and example personas are all welcome contributions.

## License

MIT — the spec is free to use, implement, and build on.

---

*[personaspec.org](https://personaspec.org) · [hello@personaspec.com](mailto:hello@personaspec.com)*
