---
id: "dsh-note-0206"
title: "Tool-call file open in OS"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-28-tool-call-file-open-in-os.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
aliases:
  - "openPath"
  - "path"
  - "file_path"
  - "host.openPath"
  - "WorkspaceRuntime.openPath"
  - "host.pickDirectory"
  - "pickDirectory"
  - "Invoke-Item"
  - "xdg-open"
  - "wslpath -w"
  - "web_fetch"
  - "powershell.exe"
  - "Tool-call file open in OS"
  - "feature"
search_regex: "(?i)(openPath|path|file_path|host\\.openPath|WorkspaceRuntime\\.openPath|host\\.pickDirectory|pickDirectory|Invoke\\-Item)"
---

# 0206. Tool-call file open in OS — implementation context

## Open this when

Chat tool rows treated the whole summary line as a click target that opened the right-hand details panel, with a hover background on the row. For filesystem tools the useful action is opening the mentioned file in the operating system's default application, not inspecting the raw tool payload in a sidebar.

## Source decision

File-tool path summaries (read / write / edit args carrying path or file_path) render as links underlined at rest with a pointer cursor. Clicking the path calls host.openPath through WorkspaceRuntime.openPath, resolving relative paths against the session cwd. File-link rows disable args expand (leading icon is inert); whole-row click, row hover fill, and the click-to-open-details gesture are removed from tool rows (including bash and todo registrations). The details panel and its inject surface remain for programmatic selection; rows no longer drive them.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-28-tool-call-file-open-in-os.md](../02-notes/implemented/feature/2026-07-28-tool-call-file-open-in-os.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-28-tool-call-file-open-in-os.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-28-tool-call-file-open-in-os.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `path`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `openPath`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `openPath` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1899`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1899) | `function openPath(` |
| `path` | `const` | [`vendor/hmr/src/index.ts:514`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L514) | `const path = relative(this.baseDir, fileURLToPath(filename))` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `openPath`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `openPath`.
- [`packages/host/apiproxy/tests/api-proxy-workspace.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-workspace.spec.ts) — A test under the owning area exercises or imports `openPath`.
- [`packages/host/apiproxy/tests/api-proxy-agent-preset.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-agent-preset.spec.ts) — A test under the owning area exercises or imports `openPath`.

## How to read the implementation

1. Start with [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`
- Aliases: `openPath`, `path`, `file_path`, `host.openPath`, `WorkspaceRuntime.openPath`, `host.pickDirectory`, `pickDirectory`, `Invoke-Item`, `xdg-open`, `wslpath -w`, `web_fetch`, `powershell.exe`, `Tool-call file open in OS`, `feature`
- Regex: `(?i)(openPath|path|file_path|host\.openPath|WorkspaceRuntime\.openPath|host\.pickDirectory|pickDirectory|Invoke\-Item)`

```bash
rg -n --pcre2 "(?i)(openPath|path|file_path|host\\.openPath|WorkspaceRuntime\\.openPath|host\\.pickDirectory|pickDirectory|Invoke\\-Item)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`.
- **`shares-code-with`** — [0191. Native workspace directory picker](0191-native-workspace-directory-picker.md): Shares source implementation: `packages/host/apiproxy/tests/api-proxy-workspace.spec.ts`, `packages/host/apiproxy/tests/client-handler.spec.ts`.
- **`shares-code-with`** — [0620. TUI diff card dropped the duplicated file path](0620-tui-diff-card-dropped-the-duplicated-file-path.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`.
- **`shares-code-with`** — [0478. Copy-only preset authoring, and the way into a preset's files](0478-copy-only-preset-authoring-and-the-way-into-a-preset-s-files.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- **`shares-code-with`** — [0212. Current sandbox policy context](0212-current-sandbox-policy-context.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.
- **`shares-code-with`** — [0216. Search render intent --- grep and glob emit a structured search card](0216-search-render-intent-grep-and-glob-emit-a-structured-search-card.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.
- **`shares-code-with`** — [0284. A minimal read_image tool over existing seams](0284-a-minimal-read-image-tool-over-existing-seams.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `vendor/hmr/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0206-tool-call-file-open-in-os.md`.
