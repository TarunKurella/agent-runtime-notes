---
id: "dsh-note-0237"
title: "opening a produced file from the web UI"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-web-workspace-file-links.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/trust"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "producedForClosing"
  - "ToolRow"
  - "openFile"
  - "title"
  - "describe"
  - "list"
  - "openPath"
  - "locations"
  - "deepseek-homepage.html"
  - "ToolCallView.locations"
  - "host.openPath"
  - "@deepseek-ai/dsh-client-ui-deliverables"
  - "conversation.chat.turnTail"
  - "turnTail"
search_regex: "(?i)(producedForClosing|ToolRow|openFile|title|describe|list|openPath|locations)"
---

# 0237. opening a produced file from the web UI — implementation context

## Open this when

A web session that produced a file had no way to look at it. The agent wrote deepseek-homepage.html, said so, and the user's only recourse was to copy an absolute path like /private/tmp/dsh-client-hotplug.ygPvsm/workspaces/plugin-hotplug/deepseek-homepage.html into a terminal. Two distinct defects sat behind that. The transcript never said what a turn had produced: ToolCallView.locations --- the follow-along vocabulary the file tools already populate --- had no consumer in the client, so a reader's only account of the output was whatever the closing message happened to spell.

## Source decision

A finished turn ends with the files it produced. The row is its own plugin, @deepseek-ai/dsh-client-ui-deliverables, registered into the conversation.chat.turnTail hole the chat view renders between a closing message's body and its IconActions --- ui-conversation owns the hole and the owner currency (nodes, closing seq, openFile), the plugin owns every policy.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-web-workspace-file-links.md](../02-notes/implemented/feature/2026-07-31-web-workspace-file-links.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-web-workspace-file-links.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-web-workspace-file-links.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-deliverables/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-deliverables`. | `named-package-member` |
| [`packages/client/ui-deliverables/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-deliverables`. | `named-package-member` |
| [`packages/client/ui-deliverables/src/client/turn-deliverables.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/turn-deliverables.ts) | runtime implementation | Core file in the package named by the note: `packages/client/ui-deliverables`. Defines `producedForClosing`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-deliverables`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/client/web/src/app.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx) | runtime implementation | Defines `title`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/py-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts) | runtime implementation | Defines `describe`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/translate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/translate.ts) | runtime implementation | Defines `locations`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `openPath`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) | runtime implementation | Defines `ToolRow`, a construct named by the note. Defines `openFile`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-deliverables/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/README.md) | package contract and examples | Core file in the package named by the note: `packages/client/ui-deliverables`. | `named-package-member` |
| [`packages/client/ui-deliverables/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/package.json) | composition and configuration | Core file in the package named by the note: `packages/client/ui-deliverables`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `producedForClosing` | `function` | [`packages/client/ui-deliverables/src/client/turn-deliverables.ts:72`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/client/turn-deliverables.ts#L72) | `export function producedForClosing(` |
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |
| `openFile` | `const` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:177`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L177) | `const openFile = (event: MouseEvent<HTMLButtonElement>) => {` |
| `title` | `const` | [`packages/client/web/src/app.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/app.tsx#L32) | `const title = useSessions((state) => {` |
| `describe` | `function` | [`packages/core/tools/src/py-types.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/py-types.ts#L223) | `function describe(schema: object): string \| undefined {` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `openPath` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1899`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1899) | `function openPath(` |
| `locations` | `const` | [`packages/lsp/lsp-stdio/src/translate.ts:150`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/translate.ts#L150) | `const locations: LspLocation[] = []` |

### Tests and executable evidence

- [`packages/lsp/lsp-stdio/tests/built-lib.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/built-lib.e2e.ts) — A test under the owning area exercises or imports `locations`.
- [`packages/lsp/lsp-stdio/tests/instance.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/instance.spec.ts) — A test under the owning area exercises or imports `locations`.
- [`packages/lsp/lsp-stdio/tests/lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/lifecycle.spec.ts) — A test under the owning area exercises or imports `locations`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `openPath`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `openPath`.
- [`packages/lsp/lsp-stdio/tests/typescript-server.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/tests/typescript-server.e2e.ts) — A test under the owning area exercises or imports `locations`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`. A test under the owning area exercises or imports `openFile`.
- [`packages/client/ui-tool/tests/tool-row.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/tool-row.client.spec.tsx) — A test under the owning area exercises or imports `ToolRow`. A test under the owning area exercises or imports `openFile`.

## How to read the implementation

1. Start with [`packages/client/ui-deliverables/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-deliverables/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/trust`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `producedForClosing`, `ToolRow`, `openFile`, `title`, `describe`, `list`, `openPath`, `locations`, `deepseek-homepage.html`, `ToolCallView.locations`, `host.openPath`, `@deepseek-ai/dsh-client-ui-deliverables`, `conversation.chat.turnTail`, `turnTail`
- Regex: `(?i)(producedForClosing|ToolRow|openFile|title|describe|list|openPath|locations)`

```bash
rg -n --pcre2 "(?i)(producedForClosing|ToolRow|openFile|title|describe|list|openPath|locations)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0274. inline-code file mentions open the file they name](0274-inline-code-file-mentions-open-the-file-they-name.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/lsp/lsp-stdio/src/translate.ts`.
- **`shares-code-with`** — [0366. Preset cards clamp their description instead of sizing the roster](0366-preset-cards-clamp-their-description-instead-of-sizing-the-roster.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/py-types.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/hooks/hook-protocol/src/merge.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0622. Web conversation UI polish sweep](0622-web-conversation-ui-polish-sweep.md): Shares source implementation: `packages/core/tools/src/py-types.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0199. Workspace Registration Deletion](0199-workspace-registration-deletion.md): Shares source implementation: `packages/hooks/hook-protocol/src/merge.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/client/web/src/app.tsx`, `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0237-opening-a-produced-file-from-the-web-ui.md`.
