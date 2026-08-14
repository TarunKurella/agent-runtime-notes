---
id: "dsh-note-0295"
title: "Web `/export` shares the streamed Session ZIP download"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-web-export-command-and-dialog.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/trust"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
aliases:
  - "json"
  - "actions"
  - "/export"
  - "@deepseek-ai/dsh-session-log-export"
  - "ctx.sessionLogDownload"
  - "sessionLogDownload"
  - "command/run"
  - "command/done"
  - "command.execute"
  - "dsh-client-ui-commands"
  - "GET /api/session.export"
  - "HEAD"
  - "conversation.session.header.utilities"
  - "conversation.session.header.actions"
search_regex: "(?i)(json|actions|/export|@deepseek\\-ai/dsh\\-session\\-log\\-export|ctx\\.sessionLogDownload|sessionLogDownload|command/run|command/done)"
---

# 0295. Web `/export` shares the streamed Session ZIP download — implementation context

## Open this when

Session export needs a stable Session-level visible action and an equivalent slash-command path. A second backend reader or Host-path writer would duplicate the download implementation and introduce platform-specific file-permission and path-reveal problems.

## Source decision

@deepseek-ai/dsh-session-log-export registers a Web-only /export human command and provides the browser ctx.sessionLogDownload controller. The command records an ordinary command/run and command/done; after command.execute returns a successful result, dsh-client-ui-commands emits a local acknowledgment that asks this browser's controller to download ApiProxy's existing GET /api/session.export ZIP. Other clients render the broadcast command nodes without repeating the browser side effect. The 111×32 Session log capsule in the Session Header calls that controller directly.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-web-export-command-and-dialog.md](../02-notes/implemented/feature/2026-08-11-web-export-command-and-dialog.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-web-export-command-and-dialog.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-web-export-command-and-dialog.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/apiproxy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/client/ui-commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-commands`. | `named-package-member` |
| [`packages/client/ui-commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-commands`. | `named-package-member` |
| [`packages/session-query/session-log-export/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-log-export/src/index.ts) | package entry point | Core file in the package named by the note: `packages/session-query/session-log-export`. | `named-package-member` |
| [`packages/session-query/session-log-export/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-log-export/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/session-query/session-log-export`. | `named-package-member` |
| [`packages/host/apiproxy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/ui-commands`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/session-query/session-log-export`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-log-export) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `json`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/contract/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts) | runtime implementation | Defines `actions`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/README.md) | package contract and examples | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `actions` | `const` | [`packages/client/runtime/src/client/contract/store.ts:218`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/contract/store.ts#L218) | `const actions = {} as Record<string, (...params: unknown[]) => void>` |

### Tests and executable evidence

- [`packages/host/apiproxy/tests/session-export.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/session-export.spec.ts) — A test under the owning area exercises or imports `HEAD`. A test under the owning area exercises or imports `readRaw`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/api-proxy-blank.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-blank.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/api-proxy-rename.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-rename.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/api-proxy-question.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-question.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/session-query/session-log-export/tests/controller.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-log-export/tests/controller.client.spec.ts) — A test under the owning area exercises or imports `HEAD`.
- [`packages/session-query/session-log-export/tests/client-apply.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session-query/session-log-export/tests/client-apply.client.spec.tsx) — A test under the owning area exercises or imports `sessionLogDownload`. A test under the owning area exercises or imports `utilities`.

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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/trust`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/web-retrieval`, `lifecycle/implemented`
- Aliases: `json`, `actions`, `/export`, `@deepseek-ai/dsh-session-log-export`, `ctx.sessionLogDownload`, `sessionLogDownload`, `command/run`, `command/done`, `command.execute`, `dsh-client-ui-commands`, `GET /api/session.export`, `HEAD`, `conversation.session.header.utilities`, `conversation.session.header.actions`
- Regex: `(?i)(json|actions|/export|@deepseek\-ai/dsh\-session\-log\-export|ctx\.sessionLogDownload|sessionLogDownload|command/run|command/done)`

```bash
rg -n --pcre2 "(?i)(json|actions|/export|@deepseek\\-ai/dsh\\-session\\-log\\-export|ctx\\.sessionLogDownload|sessionLogDownload|command/run|command/done)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0060. dsh web config-tree boot and the web transport layering](0060-dsh-web-config-tree-boot-and-the-web-transport-layering.md): Shares source implementation: `packages/client/modules/src/index.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0069. One carrier-level browser-trust boundary for all `/api` routes](0069-one-carrier-level-browser-trust-boundary-for-all-api-routes.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/index.ts`, `packages/host/apiproxy/src/invariant.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/index.ts`, `packages/host/apiproxy/src/invariant.ts`.
- **`shares-code-with`** — [0205. Todo plan strip clears on the next turn](0205-todo-plan-strip-clears-on-the-next-turn.md): Shares source implementation: `packages/host/apiproxy`, `packages/host/apiproxy/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0295-web-export-shares-the-streamed-session-zip-download.md`.
