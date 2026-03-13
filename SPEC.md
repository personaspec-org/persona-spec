# Persona Interchange Format (PIF)
### Version 1.0

**Status:** Draft  
**Published:** 2026  
**Author:** PersonaSpec — [personaspec.org](https://personaspec.org)  
**License:** MIT  
**Feedback:** hello@personaspec.com  

---

## Abstract

The Persona Interchange Format (PIF) is an open standard for a portable, structured identity file that communicates who a person is to any AI system — their personality, voice, interests, and preferences — without requiring the user to re-explain themselves in every new context.

PIF files are user-owned, consent-driven, and designed to be consumed at runtime by AI-powered applications. The user controls what is shared, with whom, and in what context.

---

## 1. Why Implement PIF?

Every AI-powered product starts from zero. Users re-introduce themselves to every chatbot, every personalization engine, every recommendation system. The result is generic experiences that feel indifferent to who the user actually is.

PIF solves this at the protocol level. When a user brings their persona to your platform:

- **Zero onboarding friction** — your AI already knows who it's talking to
- **Instant personalization** — tone, style, interests, and context available immediately
- **User trust** — the user chose to share their context with you explicitly
- **Better retention** — personalized experiences convert and retain better than generic ones

A user with a PIF persona is a more engaged user. They arrived with intent.

---

## 2. File Format

| Property | Value |
|----------|-------|
| File extension | `.persona` |
| MIME type | `application/persona+json` |
| Encoding | UTF-8 |
| Format | JSON |
| Current version | `1.0` |

---

## 3. Quickstart

The simplest valid PIF file:

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
    "style": "Concise"
  },
  "rules": {
    "transparency": "if_asked"
  }
}
```

Fetch a ready-to-use AI system prompt in one call:

```bash
GET https://personaspec.com/api/v1/alex-chen?format=prompt
```

Response:

```
You are representing Alex Chen, a product designer who codes based in San Francisco, CA.
Tone: Casual. Style: Concise.
If sincerely asked whether you are an AI, acknowledge it honestly.
Only state facts explicitly present in this prompt. Deflect anything not covered.
```

Drop that string directly into your system prompt. Done.

---

## 4. Full Schema

### 4.1 Root

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `version` | string | ✅ | PIF version. Currently `"1.0"` |
| `meta` | object | | File metadata |
| `identity` | object | ✅ | Core identity fields |
| `voice` | object | | Communication style |
| `rules` | object | | Behavioral rules |
| `privacy` | object | | Field-level privacy controls |
| `sharing` | object | | API access and consent controls |
| `extensions` | object | | Domain-specific context |
| `modes` | object | | Named configuration presets |

---

### 4.2 Meta

```json
"meta": {
  "spec": "https://personaspec.org/spec/v1",
  "min_compatible": "1.0",
  "created": "2026-01-01",
  "updated": "2026-01-01",
  "username": "alex-chen",
  "url": "https://personaspec.com/alex-chen",
  "visibility": "public",
  "api_enabled": true
}
```

| Field | Type | Description |
|-------|------|-------------|
| `spec` | string | URL of the spec version this file conforms to |
| `min_compatible` | string | Minimum spec version required to read this file |
| `created` | string | ISO 8601 date of creation |
| `updated` | string | ISO 8601 date of last update |
| `username` | string | Unique username on the hosting platform |
| `url` | string | Canonical URL of the hosted persona |
| `visibility` | string | `"public"` or `"private"` |
| `api_enabled` | boolean | Whether the public API is enabled for this persona |

---

### 4.3 Identity

```json
"identity": {
  "name": "Alex Chen",
  "tagline": "Product designer who codes",
  "location": "San Francisco, CA"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | ✅ | Display name |
| `tagline` | string | | One line self-description |
| `location` | string | | City, region, or country |

---

### 4.4 Voice

```json
"voice": {
  "tone": "Casual",
  "style": "Concise",
  "personality_narrative": "...",
  "examples": [
    {
      "prompt": "tell me about yourself",
      "response": "I design products and write just enough code to be dangerous."
    }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `tone` | string | One of: `Casual`, `Professional`, `Formal`, `Playful`, `Direct`, `Warm` |
| `style` | string | One of: `Concise`, `Detailed`, `Storyteller`, `Analytical`, `Conversational` |
| `personality_narrative` | string | 3-4 sentence first-person description. AI-synthesized. Private by default. |
| `examples` | array | Voice examples. Each has `prompt` (string) and `response` (string). |

---

### 4.5 Rules

```json
"rules": {
  "transparency": "if_asked",
  "behaviors": ["no_opinions", "no_commitments"],
  "custom": ["Don't discuss current employer details"]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `transparency` | string | `"always"` — identifies as AI at start of every conversation. `"if_asked"` — acknowledges honestly when sincerely asked (default). `"never"` — stays in character at all times. |
| `behaviors` | array | Standard behavior flags (see below) |
| `custom` | array | Freeform rules as plain English strings |

**Standard behavior flags:**

| Flag | Meaning |
|------|---------|
| `no_opinions` | Don't share strong opinions on the person's behalf |
| `no_commitments` | Don't make commitments or promises |
| `deflect_personal` | Deflect overly personal questions gracefully |
| `stay_on_topic` | Stay focused on professional and stated context |

---

### 4.6 Privacy

Controls which fields are exposed at each access level.

```json
"privacy": {
  "personality_narrative": "private",
  "professional": "private",
  "personal": "public",
  "social": "private",
  "commerce": "private",
  "learning": "private",
  "location": "public",
  "employer": "private"
}
```

**Access levels:**

| Level | Description |
|-------|-------------|
| `"public"` | Returned by the public API with no authentication |
| `"authenticated"` | Returned only after OAuth consent |
| `"private"` | Never returned by the API |

**Default privacy by field type:**

| Field type | Default |
|------------|---------|
| User-authored fields (name, tagline, voice examples) | `public` |
| AI-synthesized fields (personality_narrative) | `private` |
| PII-adjacent fields (employer, salary, handles) | `private` |

---

### 4.7 Sharing

```json
"sharing": {
  "public_api": true,
  "allowed_scopes": ["identity", "voice", "personal"],
  "allowed_contexts": ["general", "social", "professional"],
  "blocked_contexts": ["dating"],
  "blocked_domains": [],
  "require_attribution": true
}
```

| Field | Type | Description |
|-------|------|-------------|
| `public_api` | boolean | Whether the public API is enabled. Default `true`. |
| `allowed_scopes` | array | Which extension scopes can be requested |
| `allowed_contexts` | array | Which contexts this persona can be used in |
| `blocked_contexts` | array | Contexts explicitly blocked |
| `blocked_domains` | array | Domains blocked from accessing this persona |
| `require_attribution` | boolean | Whether consuming platforms must display PersonaSpec attribution |

---

### 4.8 Extensions

Extensions provide domain-specific context. All extension fields are optional.

#### Professional

```json
"professional": {
  "role": "Lead Developer",
  "company": "",
  "industry": "SaaS",
  "bio": "5 years building financial compliance software.",
  "skills": ["C#", ".NET", "Angular"],
  "availability": "open",
  "preferred_contact": "LinkedIn"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `role` | string | Current job title |
| `company` | string | Current employer |
| `industry` | string | Industry or domain |
| `bio` | string | Professional bio |
| `skills` | array | Skills and technologies |
| `availability` | string | `"open"`, `"not_looking"`, `"freelance"`, `"closed"` |
| `preferred_contact` | string | Preferred method of professional contact |

#### Personal

```json
"personal": {
  "interests": ["Music", "Gaming", "Photography"],
  "values": ["Simplicity", "Craft"],
  "causes": [],
  "personality_traits": ["Direct", "Dry humor"],
  "communication_preferences": {
    "preferred_medium": "async",
    "response_style": "concise"
  }
}
```

#### Social

```json
"social": {
  "handles": {
    "twitter": "",
    "linkedin": "",
    "github": "",
    "instagram": ""
  },
  "availability": {
    "open_to_meeting": true,
    "open_to_collaboration": true,
    "open_to_mentoring": false,
    "open_to_being_mentored": true
  },
  "communication_style": {
    "preferred_medium": "async",
    "response_time": "within a day",
    "preferred_format": "text"
  },
  "conversation_style": {
    "humor": "dry",
    "depth": "deep diver",
    "energy": "measured"
  },
  "topics": {
    "loves": ["Software architecture", "Music"],
    "avoids": ["Politics"],
    "expert_in": ["Angular", ".NET"]
  },
  "persona_to_persona": {
    "opener": "Usually asks what someone is working on",
    "ask_me_about": ["Software architecture", "Music"],
    "looking_for": ["Peers", "Collaborators"]
  }
}
```

The `persona_to_persona` section is used when two PIF personas interact directly. It provides context that allows each persona to engage meaningfully with the other.

#### Commerce (v1.0 stub)

```json
"commerce": {
  "buying_preferences": [],
  "brands": [],
  "budget_range": "",
  "shopping_style": ""
}
```

#### Learning (v1.0 stub)

```json
"learning": {
  "current_topics": [],
  "learning_style": "",
  "experience_level": {},
  "credentials": []
}
```

---

### 4.9 Modes

Modes are named configurations that activate a subset of extensions and optionally override voice settings. Five standard modes ship with PIF v1.0. Custom modes can be added on top.

```json
"modes": {
  "default": {
    "active_extensions": ["identity", "voice", "personal"],
    "override_tone": null,
    "override_style": null,
    "privacy_level": "public"
  },
  "professional": {
    "active_extensions": ["identity", "voice", "professional"],
    "override_tone": null,
    "override_style": null,
    "privacy_level": "public"
  },
  "social": {
    "active_extensions": ["identity", "voice", "personal", "social"],
    "override_tone": null,
    "override_style": null,
    "privacy_level": "public"
  },
  "minimal": {
    "active_extensions": ["identity"],
    "override_tone": null,
    "override_style": null,
    "privacy_level": "public"
  },
  "full": {
    "active_extensions": ["identity", "voice", "personal", "social", "professional", "learning"],
    "override_tone": null,
    "override_style": null,
    "privacy_level": "authenticated"
  }
}
```

**Standard mode inheritance chain:**

```
minimal → default → social    → full
                 → professional → full
```

Each mode inherits from the mode to its left and adds on top.

**Platforms activate modes via query parameter:**

```
GET /api/v1/{username}?mode=professional
```

If a requested mode is unavailable or blocked, the API falls back through the chain: `requested → default → minimal → identity only`.

---

## 5. Public API

### 5.1 Get Persona

```
GET https://personaspec.com/api/v1/{username}
```

Returns the public persona for the given username.

**Query parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `mode` | string | Activate a named mode. Default: `default` |
| `format` | string | `json` (default) or `prompt` (returns ready-to-use AI system prompt) |
| `scope` | string | `minimal`, `full`, `professional`, `personal` |
| `context` | string | Standard context value (see below) |
| `audience` | string | Free text description of the intended audience |
| `platform` | string | Name of the calling platform |
| `include` | string | Comma-separated fields to include |
| `exclude` | string | Comma-separated fields to exclude |

**Standard context values:**

| Value | Description |
|-------|-------------|
| `general` | Default, no specific context |
| `professional` | Work, career, business |
| `job_application` | Actively applying for roles |
| `networking` | Meeting peers and collaborators |
| `social` | Casual personal interaction |
| `dating` | Romantic context |
| `customer_support` | Helping with a product or service |
| `education` | Teaching or learning context |
| `creative` | Creative collaboration |
| `commerce` | Buying or selling context |

**Example — get a ready-to-use system prompt for a job platform:**

```
GET /api/v1/alex-chen
  ?format=prompt
  &mode=professional
  &context=job_application
  &platform=JobBoard
  &audience=senior engineering recruiters
```

**Response codes:**

| Code | Meaning |
|------|---------|
| `200` | Success |
| `404` | Username not found or persona deleted |
| `403` | Persona exists but public API is disabled |
| `429` | Rate limited — 100 requests/hour per IP unauthenticated |

### 5.2 Versioning

Minor versions (1.0 → 1.1) are additive only. New fields are always optional. Implementations must silently ignore unknown fields. Minor versions are always backwards compatible.

Major versions (1.0 → 2.0) may introduce breaking changes. A minimum 24 month support window will be provided for any major version after a successor is released.

The `min_compatible` field in `meta` indicates the minimum spec version required to fully read a file.

---

## 6. The `?format=prompt` Template

When `format=prompt` is requested the API returns a plain text string ready to use as an AI system prompt. The template degrades gracefully — a minimal persona still produces a useful prompt.

**Template structure:**

```
You are an AI persona representing {name}.

WHO THEY ARE:
{tagline}. Based in {location}.
{personality_narrative if present and public}

HOW THEY COMMUNICATE:
Tone: {tone}. Style: {style}.
{style guidance based on style value}

THEIR VOICE — respond the way they would:
When asked "{prompt}" they responded: "{response}"
{repeated for each voice example}

WHAT THEY CARE ABOUT:
{interests, values as comma separated list}

PROFESSIONAL CONTEXT {if public and present}:
{role} in {industry}.
{bio}

CONTEXT FOR THIS INTERACTION {if context param present}:
Platform: {platform}
Purpose: {context}
Audience: {audience}

RULES — always follow these:
{transparency rule based on transparency field}
- Only state facts explicitly present in this prompt
- If asked about anything not covered, deflect naturally in character
- Never infer or assume personal details not stated
{behavior rules translated to plain English}
{custom rules}

You are not a generic assistant. You are {name}.
Every response should sound like them, not like an AI.
```

---

## 7. Security Requirements

Implementations consuming PIF data must follow these requirements:

**Data minimization** — Request only the scopes necessary for your use case. Do not request professional data for a recipe platform.

**Cache TTL** — Do not cache persona data for more than 24 hours. Users may update or delete their persona at any time.

**No training** — Persona data must not be used to train, fine-tune, or otherwise inform machine learning models. Persona data is provided for runtime personalization only.

**Deletion propagation** — A `404` response from the API is a deletion signal. Implementations must purge any stored persona data within 48 hours of receiving a `404`.

**Consent transparency** — Any product implementing PIF must disclose to the user that their PersonaSpec persona is being used to personalize their experience.

**Revocation** — OAuth access tokens must be invalidated immediately upon user revocation. Consuming applications must stop receiving data at the point of revocation.

**Liability disclaimer** — PIF data is user-provided and unverified. Implementations must not use PIF data for consequential decisions — hiring, lending, insurance, legal — without independent verification.

---

## 8. GDPR Compliance Notes

PIF is designed to support GDPR compliance for European implementations:

- **Lawful basis** — User-initiated sharing constitutes explicit consent under Article 6(1)(a)
- **Data minimization** — The scope system directly supports Article 5(1)(c)
- **Right to erasure** — Deletion propagation supports Article 17
- **Data portability** — The `.persona` file format directly supports Article 20
- **Purpose limitation** — The context system supports Article 5(1)(b)

Implementations serving EU users should maintain records of persona data access for compliance purposes.

---

## 9. Contributing

PIF is an open standard. Contributions are welcome.

- **Spec feedback** — Open an issue at [github.com/personaspec-org/persona-spec](https://github.com/personaspec-org/persona-spec)
- **New extension proposals** — Submit a pull request with a proposed extension schema and use cases
- **Implementation reports** — Let us know you've implemented PIF so we can list you in the ecosystem

**Versioning policy for contributions:**

All additions to the spec must be backwards compatible within a major version. New fields must be optional. Breaking changes require a major version increment with advance notice.

---

## 10. License

The Persona Interchange Format specification is published under the MIT License.

You are free to implement, extend, and build on this standard in any product, commercial or otherwise, without restriction.

© 2026 PersonaSpec — [personaspec.org](https://personaspec.org)
