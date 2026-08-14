---
id: "dsh-note-0174"
title: "Explicit web bind address"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-22-web-bind-address.md"
implementation_evidence: "medium"
target_anchor: "exec, terminal, and process lifecycle"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/shell-terminal"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "host"
  - "startWebServer"
  - "127.0.0.1"
  - "--host 0.0.0.0"
  - "WebServerOptions.host"
  - "0.0.0.0"
  - "http://127.0.0.1:3080"
  - "dsh web --host 0.0.0.0"
  - "Explicit web bind address"
  - "feature"
  - "boundary"
  - "evidence"
  - "human control"
  - "ownership"
search_regex: "(?i)(host|startWebServer|127\\.0\\.0\\.1|\\-\\-host[- ]0\\.0\\.0\\.0|WebServerOptions\\.host|0\\.0\\.0\\.0|http://127\\.0\\.0\\.1:3080|dsh[- ]web[- ]\\-\\-host[- ]0\\.0\\.0\\.0)"
---

# 0174. Explicit web bind address — implementation context

## Open this when

dsh web binds every network interface even when its browser runs on the same machine. Local use therefore exposes an unauthenticated development server without an explicit operator choice, while remote-container and LAN-browser use still needs a supported way to accept non-loopback connections. The HTTP carrier also hides the bind address inside startWebServer(), so alternate shells cannot state their own network policy at the package boundary.

## Source decision

dsh web binds 127.0.0.1 by default. The CLI accepts --host 0.0.0.0 as the explicit all-interface mode and rejects other values so its network modes remain a small, deliberate contract. All-interface mode keeps printing the loopback URL and, when available, the first external IPv4 URL. WebServerOptions.host is required. The HTTP carrier passes that value to node:http without supplying a fallback, leaving each shell responsible for its bind policy. Programmatic carrier consumers may select another hostname or address directly.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-22-web-bind-address.md](../02-notes/implemented/feature/2026-07-22-web-bind-address.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-22-web-bind-address.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-22-web-bind-address.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/modules/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/invariant.ts) | runtime contract checks | Defines `host`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `host` | `const` | [`packages/client/modules/src/invariant.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/invariant.ts#L28) | `const host = ctx.get('clientModules')` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/host/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/host/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/host`.

## How to read the implementation

1. Start with [`packages/client/modules/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/invariant.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** exec, terminal, and process lifecycle.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/recovery`, `domain/build-release`, `domain/shell-terminal`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `host`, `startWebServer`, `127.0.0.1`, `--host 0.0.0.0`, `WebServerOptions.host`, `0.0.0.0`, `http://127.0.0.1:3080`, `dsh web --host 0.0.0.0`, `Explicit web bind address`, `feature`, `boundary`, `evidence`, `human control`, `ownership`
- Regex: `(?i)(host|startWebServer|127\.0\.0\.1|\-\-host[- ]0\.0\.0\.0|WebServerOptions\.host|0\.0\.0\.0|http://127\.0\.0\.1:3080|dsh[- ]web[- ]\-\-host[- ]0\.0\.0\.0)`

```bash
rg -n --pcre2 "(?i)(host|startWebServer|127\\.0\\.0\\.1|\\-\\-host[- ]0\\.0\\.0\\.0|WebServerOptions\\.host|0\\.0\\.0\\.0|http://127\\.0\\.0\\.1:3080|dsh[- ]web[- ]\\-\\-host[- ]0\\.0\\.0\\.0)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0082. what the configuration plane exposes, and who may overwrite what](0082-what-the-configuration-plane-exposes-and-who-may-overwrite-what.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/host`, `packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`.
- **`shares-code-with`** — [0069. One carrier-level browser-trust boundary for all `/api` routes](0069-one-carrier-level-browser-trust-boundary-for-all-api-routes.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/host`, `packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`.
- **`shares-code-with`** — [0209. Adaptive default for the directory-picker interaction](0209-adaptive-default-for-the-directory-picker-interaction.md): Shares source implementation: `packages/client/modules/src/invariant.ts`.
- **`shares-code-with`** — [0099. WebSocket carrier for browser downlinks](0099-websocket-carrier-for-browser-downlinks.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/host/src/index.ts`.
- **`same-design-pressure`** — [0175. Web multimodal image input and durable attachments](0175-web-multimodal-image-input-and-durable-attachments.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/human-control`.
- **`same-design-pressure`** — [0278. Web background-job display](0278-web-background-job-display.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/human-control`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/human-control`.
- **`same-design-pressure`** — [0177. Safe assistant Markdown in the Web conversation](0177-safe-assistant-markdown-in-the-web-conversation.md): Shares design concerns: `concern/boundary`, `concern/evidence`, `concern/human-control`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0174-explicit-web-bind-address.md`.
