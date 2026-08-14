---
id: "dsh-note-0534"
title: "Drop bash full-output spill files"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-06-20-drop-bash-output-spill-files.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/trust"
  - "domain/context"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/rejected"
aliases:
  - "renderResult"
  - "OutputCollector"
  - "CollectedOutput"
  - "dsh-bash-local"
  - "bash_output"
  - "Drop bash full-output spill files"
  - "simplification"
  - "boundary"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "performance"
  - "recovery"
  - "trust"
search_regex: "(?i)(renderResult|OutputCollector|CollectedOutput|dsh\\-bash\\-local|bash_output|Drop[- ]bash[- ]full\\-output[- ]spill[- ]files|simplification|boundary)"
---

# 0534. Drop bash full-output spill files — implementation context

## Open this when

dsh-bash-local keeps bounded in-memory output and spills large stdout/stderr streams into private temp files. That requires a private directory, random owner-only file creation, close-failure handling, byte-offset incremental reads, lossy read reporting, path rendering in model-facing text, and cleanup discipline. The tool then tells the model to read a local spill path when output was truncated. This solves a real problem, but in a narrow and leaky way. A spill path is a process-local filesystem artifact exposed to model output, not a durable harness artifact with scoped access, retention, or UI affordances.

## Source decision

Keep tail truncation, drop full-output spill files. A bash result contains the bounded tail plus a clear truncation marker; no path is emitted. If users need full-output recovery, add a generic artifact/blob service with explicit ownership, cleanup, and UI rendering, then let bash attach large outputs to that service. This proposal can land independently of a generic long-running tool runtime. If background jobs stay, bash_output should still report that output was dropped, but without advertising a spill path.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-06-20-drop-bash-output-spill-files.md](../02-notes/rejected/simplification/2026-06-20-drop-bash-output-spill-files.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-06-20-drop-bash-output-spill-files.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-06-20-drop-bash-output-spill-files.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/shell/bash-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/shell/bash-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/shell/bash-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/shell/tool-bash/src/render.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts) | runtime implementation | Defines `renderResult`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts) | public types and contract | Defines `CollectedOutput`, a construct named by the note. | `symbol-definition` |
| [`packages/subprocess/subprocess-local/src/spawn.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts) | runtime implementation | Defines `OutputCollector`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/bash-local/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/README.md) | package contract and examples | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`packages/shell/bash-local/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/package.json) | composition and configuration | Core file in the package named by the note: `packages/shell/bash-local`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-bash-local` named by the note. | `exact-code-occurrence` |
| [`docs/testing.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.md) | package contract and examples | Contains the exact code literal `dsh-bash-local` named by the note. | `exact-code-occurrence` |
| [`docs/glossary.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.md) | package contract and examples | Contains the exact code literal `dsh-bash-local` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `renderResult` | `function` | [`packages/shell/tool-bash/src/render.ts:28`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L28) | `export function renderResult(` |
| `OutputCollector` | `class` | [`packages/subprocess/subprocess-local/src/spawn.ts:104`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/src/spawn.ts#L104) | `export class OutputCollector {` |
| `CollectedOutput` | `interface` | [`packages/subprocess/subprocess/src/types.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess/src/types.ts#L22) | `export interface CollectedOutput {` |

### Tests and executable evidence

- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `renderResult`.
- [`packages/shell/bash-local/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`.
- [`packages/shell/bash-local/tests/executor.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-local/tests/executor.spec.ts) — A test under the owning area exercises or imports `dsh-bash-local`.
- [`packages/subprocess/subprocess-local/tests/spawn.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subprocess/subprocess-local/tests/spawn.spec.ts) — A test under the owning area exercises or imports `OutputCollector`.
- Source verification intent: CollectedOutput no longer carries spill paths. OutputCollector keeps bounded buffers only and deletes the temp-file machinery. renderResult() reports truncation without a filesystem path. Tests cover tail truncation and no longer assert full-output file contents. Security guidance in docs/defensive-patterns.md stops treating private spill files as a model-visible interface.

## How to read the implementation

1. Start with [`docs/defensive-patterns.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/defensive-patterns.md) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/context`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/rejected`
- Aliases: `renderResult`, `OutputCollector`, `CollectedOutput`, `dsh-bash-local`, `bash_output`, `Drop bash full-output spill files`, `simplification`, `boundary`, `evidence`, `lifecycle`, `ownership`, `performance`, `recovery`, `trust`
- Regex: `(?i)(renderResult|OutputCollector|CollectedOutput|dsh\-bash\-local|bash_output|Drop[- ]bash[- ]full\-output[- ]spill[- ]files|simplification|boundary)`

```bash
rg -n --pcre2 "(?i)(renderResult|OutputCollector|CollectedOutput|dsh\\-bash\\-local|bash_output|Drop[- ]bash[- ]full\\-output[- ]spill[- ]files|simplification|boundary)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0015. The background job runtime (`ctx.jobs`) and generic task control tools](0015-the-background-job-runtime-ctx-jobs-and-generic-task-control-tools.md): The source note links to this decision directly.
- **`shares-code-with`** — [0020. stdin + extra env on the bash seam](0020-stdin-extra-env-on-the-bash-seam.md): Shares source implementation: `docs/defensive-patterns.md`, `packages/shell/bash-local/src/index.ts`.
- **`shares-code-with`** — [0242. pwsh tool bash parity](0242-pwsh-tool-bash-parity.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/bash-local/src/invariant.ts`.
- **`shares-code-with`** — [0367. Synchronous cleanup of managed subprocesses on host exit](0367-synchronous-cleanup-of-managed-subprocesses-on-host-exit.md): Shares source implementation: `packages/subprocess/subprocess-local/src/spawn.ts`, `packages/subprocess/subprocess/src/types.ts`.
- **`shares-code-with`** — [0066. The subprocess service is its own seam under the bash executors (`dsh-subprocess` / `dsh-subprocess-local`)](0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/shell/bash-local/src/invariant.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `docs/defensive-patterns.md`.
- **`shares-code-with`** — [0028. A shared timeout/deadline primitive, with hard-kill left to each capability](0028-a-shared-timeout-deadline-primitive-with-hard-kill-left-to-each-capabili.md): Shares source implementation: `packages/shell/bash-local/src/index.ts`, `packages/subprocess/subprocess-local/src/spawn.ts`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `docs/defensive-patterns.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0534-drop-bash-full-output-spill-files.md`.
