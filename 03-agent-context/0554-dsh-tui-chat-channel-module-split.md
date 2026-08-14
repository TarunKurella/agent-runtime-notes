---
id: "dsh-note-0554"
title: "dsh-tui chat channel module split"
status: "archived"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/architecture/2026-07-27-tui-chat-channel-module-split.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/projection"
aliases:
  - "ctx"
  - "resolved"
  - "deps"
  - "packages/ui/tui/src/index.ts"
  - "createTuiChat"
  - "src/"
  - "components/"
  - "session/"
  - "extension/"
  - "autocomplete.ts"
  - "file-autocomplete.ts"
  - "skill-invocation.ts"
  - "xml-tool-output.ts"
  - "src/chat/"
search_regex: "(?i)(resolved|deps|packages/ui/tui/src/index\\.ts|createTuiChat|src/|components/|session/|extension/)"
---

# 0554. dsh-tui chat channel module split — implementation context

## Open this when

packages/ui/tui/src/index.ts had grown past 2000 lines. Most of it was one createTuiChat factory: a ~1600-line closure holding roughly forty mutable variables and as many nested closures. Model selection, the ask-user-question queue, and session resume were tangled into that single scope, so a reader could not follow any one concern without holding the whole file in their head, and unrelated edits collided.

## Source decision

The chat channel's cohesive sub-machines are extracted from createTuiChat into src/chat/, each a factory that takes an explicit dependency bundle instead of closing over the entry scope: chat/model-command.ts --- createModelController: the queued /model command, the model+reasoning-effort selector overlay, and the selected model's context-window resolution. Owns the context-window cache that the prompt and status views read. chat/questions.ts --- createQuestionQueue: the user-interaction provider and the one-at-a-time FIFO ask-user-question overlays.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/architecture/2026-07-27-tui-chat-channel-module-split.md](../02-notes/archived/architecture/2026-07-27-tui-chat-channel-module-split.md)
- Pinned source: [.agents/notes/archived/architecture/2026-07-27-tui-chat-channel-module-split.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/architecture/2026-07-27-tui-chat-channel-module-split.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) | repository automation | Defines `deps`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-module-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-module-graph.ts) | repository automation | Defines `deps`, a construct named by the note. | `symbol-definition` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts) | package entry point | Defines `resolved`, a construct named by the note. | `symbol-definition` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Defines `resolved`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/boot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/e2b/fs-e2b/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts) | package entry point | Defines `resolved`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts) | package entry point | Defines `resolved`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `deps`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `ctx` | `const` | [`packages/boot/app-boot/src/index.ts:764`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L764) | `const ctx = new Context()` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L162) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:217`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L217) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/core/tools/src/index.ts:947`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L947) | `const ctx = this.ctx` |
| `resolved` | `const` | [`packages/e2b/e2b/src/index.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L93) | `const resolved = config as SchemaResolvedConfig` |
| `resolved` | `const` | [`packages/e2b/fs-e2b/src/index.ts:359`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/fs-e2b/src/index.ts#L359) | `const resolved = entry.symlinkTarget === undefined` |
| `resolved` | `const` | [`packages/fs/tool-fs/src/index.ts:56`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/index.ts#L56) | `const resolved = config as ResolvedConfig` |
| `deps` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3644`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3644) | `const deps = sessionLogExportDeps(ctx)` |
| `resolved` | `const` | [`packages/llm/llm/src/index.ts:633`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L633) | `const resolved = await registration.adapter.resolveModel(provider, model, signal)` |
| `resolved` | `const` | [`packages/llm/llm/src/index.ts:781`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts#L781) | `const resolved = await this.resolveCallFor(registration, config, signal)` |
| `deps` | `const` | [`scripts/gen-module-graph.ts:72`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-module-graph.ts#L72) | `const deps = p.deps.length ? p.deps.map((d) => {` |
| `deps` | `const` | [`scripts/package-graph.ts:44`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts#L44) | `const deps = Object.keys(json.peerDependencies ?? {})` |

### Tests and executable evidence

- [`apps/web/tests/lifecycle-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/lifecycle-chrome.e2e.ts) — A test under the owning area exercises or imports `palette`.
- [`apps/web/tests/sidebar-scrollbar.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/sidebar-scrollbar.e2e.ts) — A test under the owning area exercises or imports `palette`.
- [`packages/client/ui-theme/tests/theme.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/theme.client.spec.ts) — A test under the owning area exercises or imports `palette`.
- [`packages/client/ui-primitives/tests/ansi.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/ansi.client.spec.ts) — A test under the owning area exercises or imports `palette`.
- [`packages/client/ui-primitives/tests/icons.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/icons.client.spec.tsx) — A test under the owning area exercises or imports `palette`.
- [`packages/client/ui-layout/tests/theme-presenter.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/theme-presenter.client.spec.ts) — A test under the owning area exercises or imports `palette`.
- [`packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-theme/tests/scrollbar-styles.client.spec.ts) — A test under the owning area exercises or imports `palette`.
- [`apps/web/tests/snapshots/sidebar-scrollbar/geometry.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/sidebar-scrollbar/geometry.expected.md) — A test under the owning area exercises or imports `palette`.
- Source verification intent: Behavior is unchanged: all existing package tests and TUI snapshots pass without re-recording, which is the contract for this refactor.

## How to read the implementation

1. Start with [`scripts/package-graph.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/package-graph.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** read/write/patch filesystem capability.

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
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/concurrency`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/projection`
- Aliases: `ctx`, `resolved`, `deps`, `packages/ui/tui/src/index.ts`, `createTuiChat`, `src/`, `components/`, `session/`, `extension/`, `autocomplete.ts`, `file-autocomplete.ts`, `skill-invocation.ts`, `xml-tool-output.ts`, `src/chat/`
- Regex: `(?i)(resolved|deps|packages/ui/tui/src/index\.ts|createTuiChat|src/|components/|session/|extension/)`

```bash
rg -n --pcre2 "(?i)(resolved|deps|packages/ui/tui/src/index\\.ts|createTuiChat|src/|components/|session/|extension/)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0018. Web capability seam - stable tools over multiple providers](0018-web-capability-seam-stable-tools-over-multiple-providers.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0639. Generate the Cordis core API reference](0639-generate-the-cordis-core-api-reference.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0659. Remove the `agent/steering` mirror emit](0659-remove-the-agent-steering-mirror-emit.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `packages/client/web/src/boot.tsx`.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/fs/tool-fs/src/index.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/llm/llm/src/index.ts`.
- **`shares-code-with`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): Shares source implementation: `packages/e2b/e2b/src/index.ts`, `packages/llm/llm/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0554-dsh-tui-chat-channel-module-split.md`.
