---
id: "dsh-note-0350"
title: "Recoverable provider credential lifecycle"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-06-provider-credential-lifecycle.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/generation"
aliases:
  - "set"
  - "yaml"
  - "apiKeyEnv"
  - ".env"
  - "credentials.set"
  - "settings-conflict"
  - "<ROUTE>_API_KEY"
  - "Display Name"
  - "settings.yaml"
  - ".credentials.yaml"
  - "Recoverable provider credential lifecycle"
  - "bug fix"
  - "boundary"
  - "concurrency"
search_regex: "(?i)(yaml|apiKeyEnv|\\.env|credentials\\.set|settings\\-conflict|<ROUTE>_API_KEY|Display[- ]Name|settings\\.yaml)"
---

# 0350. Recoverable provider credential lifecycle — implementation context

## Open this when

The Models editor spans independent settings and credential RPC domains. It previously committed provider settings before storing the API key but kept the revision and original subtree from when the card opened. If the credential write failed, retry replayed the already-committed settings mutation with a stale revision and produced a conflict, leaving the user unable to complete the second stage from the same card. A blank pi-ai key also wrote the derived apiKeyEnv without a credential, which prevented pi-ai from using provider-native discovery.

## Source decision

Provider save remains a two-stage settings-then-credentials operation over the existing wire domains, but the card treats the successful settings response as a commit checkpoint. It replaces its comparison subtree and expected revision with the returned redacted descriptor before attempting credentials.set; if that second stage fails, the draft key and card stay visible, and retry produces no settings ops and repeats only the credential write. Genuine concurrent changes before the first settings commit still fail with settings-conflict.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-06-provider-credential-lifecycle.md](../02-notes/implemented/bug-fix/2026-08-06-provider-credential-lifecycle.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-06-provider-credential-lifecycle.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-06-provider-credential-lifecycle.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Defines `yaml`, a construct named by the note. | `symbol-definition` |
| [`packages/web/web-search-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/index.ts) | package entry point | Defines `apiKeyEnv`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/client-runtime/src/sessions.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts) | runtime implementation | Defines `set`, a construct named by the note. | `symbol-definition` |
| [`packages/session/session-telemetry/src/coordinator.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/coordinator.ts) | runtime implementation | Defines `set`, a construct named by the note. | `symbol-definition` |
| [`packages/test-support/client-runtime/src/settings-scope.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/settings-scope.ts) | runtime implementation | Defines `set`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `set` | `let` | [`packages/session/session-telemetry/src/coordinator.ts:251`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-telemetry/src/coordinator.ts#L251) | `let set = this.chunkSeen.get(session)` |
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `set` | `const` | [`packages/test-support/client-runtime/src/sessions.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/sessions.ts#L52) | `const set = listeners.get(key) ?? new Set()` |
| `set` | `const` | [`packages/test-support/client-runtime/src/settings-scope.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/client-runtime/src/settings-scope.ts#L34) | `const set = vi.fn(() => Promise.resolve())` |
| `apiKeyEnv` | `const` | [`packages/web/web-search-deepseek/src/index.ts:96`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/web-search-deepseek/src/index.ts#L96) | `const apiKeyEnv = credentialRef(config.apiKeyEnv ?? DEFAULT_API_KEY_ENV)` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/api-proxy-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-config.spec.ts) — A test under the owning area exercises or imports `settings-conflict`.
- [`packages/client/ui-settings-models/tests/components.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/components.client.spec.tsx) — A test under the owning area exercises or imports `settings-conflict`.
- [`packages/client/ui-settings-models/tests/provider-form.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-settings-models/tests/provider-form.client.spec.tsx) — A test under the owning area exercises or imports `settings-conflict`.
- [`packages/client/ui-permission-presets/tests/settings-store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/tests/settings-store.client.spec.ts) — A test under the owning area exercises or imports `settings-conflict`.
- [`packages/client/ui-permission-presets/tests/permission-presets-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/tests/permission-presets-row.client.spec.tsx) — A test under the owning area exercises or imports `settings-conflict`.

## How to read the implementation

1. Start with [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/configuration`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/generation`
- Aliases: `set`, `yaml`, `apiKeyEnv`, `.env`, `credentials.set`, `settings-conflict`, `<ROUTE>_API_KEY`, `Display Name`, `settings.yaml`, `.credentials.yaml`, `Recoverable provider credential lifecycle`, `bug fix`, `boundary`, `concurrency`
- Regex: `(?i)(yaml|apiKeyEnv|\.env|credentials\.set|settings\-conflict|<ROUTE>_API_KEY|Display[- ]Name|settings\.yaml)`

```bash
rg -n --pcre2 "(?i)(yaml|apiKeyEnv|\\.env|credentials\\.set|settings\\-conflict|<ROUTE>_API_KEY|Display[- ]Name|settings\\.yaml)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0087. the web configuration plane](0087-the-web-configuration-plane.md): The source note links to this decision directly.
- **`shares-code-with`** — [0299. Shared-modal product onboarding](0299-shared-modal-product-onboarding.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/test-support/client-runtime/src/sessions.ts`.
- **`shares-code-with`** — [0097. Declaring a provider from the Models page](0097-declaring-a-provider-from-the-models-page.md): Shares source implementation: `packages/client/ui-settings-models/tests/provider-form.client.spec.tsx`.
- **`shares-code-with`** — [0287. Plugin configuration in the web settings page](0287-plugin-configuration-in-the-web-settings-page.md): Shares source implementation: `packages/web/web-search-deepseek/src/index.ts`.
- **`shares-code-with`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares source implementation: `packages/test-support/client-runtime/src/sessions.ts`.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`.
- **`shares-code-with`** — [0098. Interrogating a draft provider endpoint](0098-interrogating-a-draft-provider-endpoint.md): Shares source implementation: `packages/host/apiproxy/tests/api-proxy-config.spec.ts`.
- **`shares-code-with`** — [0235. Default Web search in shipped compositions](0235-default-web-search-in-shipped-compositions.md): Shares source implementation: `packages/web/web-search-deepseek/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0350-recoverable-provider-credential-lifecycle.md`.
