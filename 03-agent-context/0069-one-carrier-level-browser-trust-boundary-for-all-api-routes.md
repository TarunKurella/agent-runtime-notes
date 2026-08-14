---
id: "dsh-note-0069"
title: "One carrier-level browser-trust boundary for all `/api` routes"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-28-api-browser-trust-boundary.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "trustedHosts"
  - "prompt"
  - "/api"
  - "127.0.0.1:3080"
  - "--host 0.0.0.0"
  - "session.prompt"
  - "text/plain"
  - "isTrustedNativeDialogRequest"
  - "host.pickDirectory"
  - "pickDirectory"
  - "application/json"
  - "src/api-request-trust.ts"
  - "sec-fetch-site: cross-site"
  - "host: 127.0.0.1 | 0.0.0.0"
search_regex: "(?i)(trustedHosts|prompt|/api|127\\.0\\.0\\.1:3080|\\-\\-host[- ]0\\.0\\.0\\.0|session\\.prompt|text/plain|isTrustedNativeDialogRequest)"
---

# 0069. One carrier-level browser-trust boundary for all `/api` routes — implementation context

## Open this when

The web GUI host serves /api over plain HTTP (default 127.0.0.1:3080, --host 0.0.0.0 supported), and the surface includes remote-code-execution-grade methods --- session.prompt drives an agent that runs bash. A browser turns the operator into a confused deputy against such a local API in two classic ways: a malicious page fires a "simple" cross-site POST (text/plain --- sent without a CORS preflight) whose side effects execute even though the response stays unreadable, and a DNS-rebound origin talks to the socket as if same-origin, making CORS inapplicable entirely, with only the Host header betraying.

## Source decision

Enforce browser trust once, at the carrier, for the entire /api prefix --- two halves: Media-type fence (dsh-host-apiproxy): every /api POST must declare application/json, else 415 before parsing. Cross-site "simple" requests thereby stop existing: any cross-site attempt is forced into a CORS preflight this server never answers. Authority fence (dsh-client-connection, src/api-request-trust.ts): every request must present a Host that is loopback or matches a trustedHosts entry (exact on host:port, any port on port-less entries, WHATWG-normalized; rebinding defense).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-28-api-browser-trust-boundary.md](../02-notes/implemented/architecture/2026-07-28-api-browser-trust-boundary.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-28-api-browser-trust-boundary.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-28-api-browser-trust-boundary.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/client/connection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/connection`. Defines `trustedHosts`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/host/apiproxy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/client/connection/src/rpc-host.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/rpc-host.ts) | runtime implementation | Core file in the package named by the note: `packages/client/connection`. Defines `trustedHosts`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/connection/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/connection`. | `named-package-member` |
| [`packages/host/apiproxy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/connection`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/schedule/schedule/src/domain.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts) | runtime implementation | Defines `prompt`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/README.md) | package contract and examples | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/apiproxy/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/package.json) | composition and configuration | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/client/connection/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/connection`. Contains the exact code literal `src/api-request-trust.ts` named by the note. | `exact-code-occurrence, named-package-member` |
| [`packages/client/connection/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/connection`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `trustedHosts` | `const` | [`packages/client/connection/src/index.ts:132`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L132) | `const trustedHosts = config?.trustedHosts ?? []` |
| `trustedHosts` | `const` | [`packages/client/connection/src/rpc-host.ts:97`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/rpc-host.ts#L97) | `const trustedHosts = options.authority === 'loopback' ? [] : this.trustedHosts` |
| `prompt` | `const` | [`packages/schedule/schedule/src/domain.ts:389`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/schedule/schedule/src/domain.ts#L389) | `const prompt = value['prompt']` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/client/connection/tests/fake-api.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/fake-api.client.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `pickDirectory`.
- [`packages/client/connection/tests/node-half.host.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/tests/node-half.host.spec.ts) — A test under the owning area exercises or imports `pickDirectory`. A test under the owning area exercises or imports `trustedHosts`.
- [`packages/host/apiproxy/tests/api-proxy-workspace.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-workspace.spec.ts) — A test under the owning area exercises or imports `pickDirectory`.

## How to read the implementation

1. Start with [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `trustedHosts`, `prompt`, `/api`, `127.0.0.1:3080`, `--host 0.0.0.0`, `session.prompt`, `text/plain`, `isTrustedNativeDialogRequest`, `host.pickDirectory`, `pickDirectory`, `application/json`, `src/api-request-trust.ts`, `sec-fetch-site: cross-site`, `host: 127.0.0.1 | 0.0.0.0`
- Regex: `(?i)(trustedHosts|prompt|/api|127\.0\.0\.1:3080|\-\-host[- ]0\.0\.0\.0|session\.prompt|text/plain|isTrustedNativeDialogRequest)`

```bash
rg -n --pcre2 "(?i)(trustedHosts|prompt|/api|127\\.0\\.0\\.1:3080|\\-\\-host[- ]0\\.0\\.0\\.0|session\\.prompt|text/plain|isTrustedNativeDialogRequest)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0099. WebSocket carrier for browser downlinks](0099-websocket-carrier-for-browser-downlinks.md): Shares source implementation: `packages/client/connection`, `packages/client/connection/src/index.ts`.
- **`shares-code-with`** — [0295. Web `/export` shares the streamed Session ZIP download](0295-web-export-shares-the-streamed-session-zip-download.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/index.ts`, `packages/host/apiproxy/src/invariant.ts`.
- **`shares-code-with`** — [0082. what the configuration plane exposes, and who may overwrite what](0082-what-the-configuration-plane-exposes-and-who-may-overwrite-what.md): Shares source implementation: `packages/client/connection/src/index.ts`, `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0114. An independent Events backstop closes the cordis-surface exhaustiveness gap](0114-an-independent-events-backstop-closes-the-cordis-surface-exhaustiveness.md): Shares source implementation: `packages/client/connection`, `packages/client/connection/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0069-one-carrier-level-browser-trust-boundary-for-all-api-routes.md`.
