---
id: "dsh-note-0017"
title: "Mandatory `User-Agent` attribution for provider requests"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-06-21-mandatory-app-attribution-headers.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "createRequire"
  - "user"
  - "metadata"
  - "headers"
  - "AppIdentity"
  - "APP_IDENTITY"
  - "userAgent"
  - "attributionHeaders"
  - "LlmAdapter"
  - "From"
  - "User-Agent"
  - "packages/llm/llm-deepseek/src/adapter.ts"
  - "packages/llm/llm-pi-ai/src/adapter.ts"
  - "HTTP-Referer"
search_regex: "(?i)(createRequire|user|metadata|headers|AppIdentity|APP_IDENTITY|userAgent|attributionHeaders)"
---

# 0017. Mandatory `User-Agent` attribution for provider requests — implementation context

## Open this when

LLM provider requests should identify the product making them. That is useful for provider-side support, abuse investigation, compatibility debugging, and traffic analytics. Before this Agent Note the harness only partially did this: the hand-rolled DeepSeek adapter sent a hand-copied User-Agent constant (packages/llm/llm-deepseek/src/adapter.ts), while the pi-ai-backed twin sent no harness-owned headers at all (packages/llm/llm-pi-ai/src/adapter.ts).

## Source decision

Provider-neutral app attribution is mandatory at the LLM adapter boundary, using the standard User-Agent header only. The rule: every product LLM adapter sends a static, non-secret application identity on every provider HTTP request, and every adapter has tests proving that User-Agent reaches the wire (a mock server asserting received headers; for a library-backed adapter, the library's header hook feeding the same mock-server assertion). This rule governs app attribution, not provider-specific request identity: the DeepSeek request-identity decision separately owns its user and session headers.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-06-21-mandatory-app-attribution-headers.md](../02-notes/implemented/architecture/2026-06-21-mandatory-app-attribution-headers.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-06-21-mandatory-app-attribution-headers.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-06-21-mandatory-app-attribution-headers.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/subsystems/llm-streaming.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/llm-streaming.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `packages/llm/llm/src/attribution.ts` named by the note. | `exact-code-occurrence, named-file` |
| [`packages/llm/llm/src/attribution.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts) | runtime implementation | The source note names this file directly. Core file in the package named by the note: `packages/llm/llm`. | `named-file, named-package-member, symbol-definition` |
| [`packages/llm/llm-pi-ai/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/adapter.ts) | provider/backend adapter | The source note names this file directly. | `named-file` |
| [`packages/llm/llm-deepseek/src/adapter.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts) | provider/backend adapter | The source note names this file directly. Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-file, named-package-member, symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. Defines `LlmAdapter`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-pi-ai/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-pi-ai`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `createRequire` | `const` | [`apps/web/src/node-module-stub.ts:7`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/src/node-module-stub.ts#L7) | `export const createRequire = (): never => {` |
| `user` | `const` | [`packages/boot/app-boot/src/index.ts:185`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L185) | `const user = home === resolve(cwd) ? undefined : readEnvLayer(binName, home, warn)` |
| `metadata` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:555`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L555) | `const metadata = sessionListMetadata(session.events)` |
| `headers` | `const` | [`packages/llm/llm-deepseek/src/adapter.ts:283`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/adapter.ts#L283) | `const headers = {` |
| `AppIdentity` | `interface` | [`packages/llm/llm/src/attribution.ts:25`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts#L25) | `export interface AppIdentity {` |
| `APP_IDENTITY` | `const` | [`packages/llm/llm/src/attribution.ts:40`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts#L40) | `export const APP_IDENTITY: AppIdentity = {` |
| `userAgent` | `function` | [`packages/llm/llm/src/attribution.ts:53`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts#L53) | `export function userAgent(identity: AppIdentity = APP_IDENTITY): string {` |
| `attributionHeaders` | `function` | [`packages/llm/llm/src/attribution.ts:64`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/attribution.ts#L64) | `export function attributionHeaders(` |
| `LlmAdapter` | `class` | [`packages/llm/llm/src/index.ts:180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L180) | `export abstract class LlmAdapter {` |
| `From` | `type` | [`vendor/schemastery/src/index.ts:10`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/schemastery/src/index.ts#L10) | `export type From<X> =` |

### Tests and executable evidence

- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `createRequire`.
- [`packages/llm/llm/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/service.spec.ts) — A test under the owning area exercises or imports `LlmAdapter`.
- [`packages/llm/llm/tests/topology.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/topology.spec.ts) — A test under the owning area exercises or imports `LlmAdapter`.
- [`packages/llm/llm/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/invariant.spec.ts) — A test under the owning area exercises or imports `LlmAdapter`.
- [`packages/llm/llm/tests/attribution.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/tests/attribution.spec.ts) — A test under the owning area exercises or imports `User-Agent`. A test under the owning area exercises or imports `AppIdentity`.
- [`packages/llm/llm-pi-ai/tests/adapter.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.e2e.ts) — A test under the owning area exercises or imports `dsh-llm-deepseek`. A test under the owning area exercises or imports `dsh-llm-pi-ai`.
- [`packages/llm/llm-pi-ai/tests/adapter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/adapter.spec.ts) — A test under the owning area exercises or imports `User-Agent`. A test under the owning area exercises or imports `userAgent`.
- [`packages/llm/llm-pi-ai/tests/catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-pi-ai/tests/catalog.spec.ts) — A test under the owning area exercises or imports `dsh-llm-pi-ai`.
- Source verification intent: The landed contract: dsh-llm documents the mandatory User-Agent attribution contract for LlmAdapter authors (LlmAdapter JSDoc, package README, and the adapter-contract section of docs/subsystems/llm-streaming.md). A shared helper (attributionHeaders / userAgent) constructs the app identity and the standard User-Agent value from package metadata, so adapters do not hand-copy version constants. dsh-llm-deepseek sends the shared User-Agent on every request and its mock-server suite asserts the exact value.

## How to read the implementation

1. Start with [`docs/subsystems/llm-streaming.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/llm-streaming.md) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `createRequire`, `user`, `metadata`, `headers`, `AppIdentity`, `APP_IDENTITY`, `userAgent`, `attributionHeaders`, `LlmAdapter`, `From`, `User-Agent`, `packages/llm/llm-deepseek/src/adapter.ts`, `packages/llm/llm-pi-ai/src/adapter.ts`, `HTTP-Referer`
- Regex: `(?i)(createRequire|user|metadata|headers|AppIdentity|APP_IDENTITY|userAgent|attributionHeaders)`

```bash
rg -n --pcre2 "(?i)(createRequire|user|metadata|headers|AppIdentity|APP_IDENTITY|userAgent|attributionHeaders)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0008. Two LLM adapters as a design-verification twin](0008-two-llm-adapters-as-a-design-verification-twin.md): The source note links to this decision directly.
- **`source-link`** — [0291. DeepSeek request user and session identity headers](0291-deepseek-request-user-and-session-identity-headers.md): The source note links to this decision directly.
- **`shares-code-with`** — [0077. request-level LLM configuration and the credential seam](0077-request-level-llm-configuration-and-the-credential-seam.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-pi-ai/src/adapter.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-deepseek/src/adapter.ts`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0097. Declaring a provider from the Models page](0097-declaring-a-provider-from-the-models-page.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm-pi-ai/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0371. First-run readiness reads every provider, and the setup card closes](0371-first-run-readiness-reads-every-provider-and-the-setup-card-closes.md): Shares source implementation: `packages/llm/llm-deepseek/src/adapter.ts`, `packages/llm/llm-deepseek/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0017-mandatory-user-agent-attribution-for-provider-requests.md`.
