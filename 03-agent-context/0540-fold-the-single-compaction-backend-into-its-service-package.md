---
id: "dsh-note-0540"
title: "Fold the single compaction backend into its service package"
status: "rejected"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/rejected/simplification/2026-07-19-fold-compaction-package-split.md"
implementation_evidence: "medium"
target_anchor: "repository tests and release policy"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/rejected"
aliases:
  - "summarize"
  - "compaction"
  - "CompactionResult"
  - "compact"
  - "@deepseek-ai/dsh-compaction"
  - "@deepseek-ai/dsh-compaction-basic"
  - "ctx.compaction"
  - "compaction-basic"
  - "Fold the single compaction backend into its service package"
  - "simplification"
  - "boundary"
  - "cancellation timeout"
  - "ownership"
  - "recovery"
search_regex: "(?i)(summarize|compaction|CompactionResult|compact|@deepseek\\-ai/dsh\\-compaction|@deepseek\\-ai/dsh\\-compaction\\-basic|ctx\\.compaction|compaction\\-basic)"
---

# 0540. Fold the single compaction backend into its service package — implementation context

## Open this when

Compaction is split between @deepseek-ai/dsh-compaction, which owns an abstract two-method service and shared types, and @deepseek-ai/dsh-compaction-basic, which owns the only complete provider. Shipped configurations load only the basic package, and no production package independently consumes the Service Definition package except that provider. The split adds a package manifest, README, project boundary, dependency edge, abstract forwarding class, generated catalog entries, and composition wiring without demonstrating backend substitution.

## Source decision

Move the basic implementation into @deepseek-ai/dsh-compaction and remove @deepseek-ai/dsh-compaction-basic. Keep ctx.compaction, CompactionResult, the shared transcript and tool-pairing helpers, the existing configuration, and the concrete compaction algorithm in one package. Preserve summarize() as a protected customization hook. A deployment-specific summarizer can subclass or intercept the existing LLM call without requiring a second capability package. Reintroduce a separate Service Definition package only when a second complete backend and an independent Consumer need substitution.

## Decision status

Rejected design. Do not implement it as written. Reuse its failure analysis, constraints, and reversal condition.

- Raw note: [rejected/simplification/2026-07-19-fold-compaction-package-split.md](../02-notes/rejected/simplification/2026-07-19-fold-compaction-package-split.md)
- Pinned source: [.agents/notes/rejected/simplification/2026-07-19-fold-compaction-package-split.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/rejected/simplification/2026-07-19-fold-compaction-package-split.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/compaction/compaction/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction`. Defines `CompactionResult`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/compaction/compaction/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/index.ts) | package entry point | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction-basic/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/compaction/compaction-basic`. | `named-package-member` |
| [`packages/compaction/compaction`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/compaction/compaction-basic`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Defines `compact`, a construct named by the note. | `symbol-definition` |
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Defines `summarize`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-trajectory/src/client/layout.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts) | runtime implementation | Defines `compaction`, a construct named by the note. | `symbol-definition` |
| [`packages/compaction/compaction/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/README.md) | package contract and examples | Core file in the package named by the note: `packages/compaction/compaction`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `summarize` | `function` | [`packages/bundle/headless/src/index.ts:61`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts#L61) | `function summarize(events: readonly SessionEvent[], firstSeq: number): RunOutcome {` |
| `compaction` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:339`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L339) | `const compaction: TurnBucket = {` |
| `CompactionResult` | `interface` | [`packages/compaction/compaction/src/types.ts:93`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/types.ts#L93) | `export interface CompactionResult {` |
| `compact` | `const` | [`packages/jobs/tool-jobs/src/index.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L161) | `const compact = \`${prefix}${action}\`` |

### Tests and executable evidence

- [`packages/compaction/compaction/tests/compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/tests/compaction.spec.ts) — A test under the owning area exercises or imports `CompactionResult`.
- [`packages/compaction/compaction-basic/tests/compaction-basic.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-basic.spec.ts) — A test under the owning area exercises or imports `CompactionResult`. A test under the owning area exercises or imports `summarize`.
- [`packages/compaction/compaction-basic/tests/manual-compaction.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/manual-compaction.spec.ts) — A test under the owning area exercises or imports `CompactionResult`. A test under the owning area exercises or imports `summarize`.
- [`packages/compaction/compaction-basic/tests/loader-composition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/loader-composition.spec.ts) — A test under the owning area exercises or imports `compaction-basic`.
- [`packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/tests/compaction-loop-repro.spec.ts) — A test under the owning area exercises or imports `summarize`. A test under the owning area exercises or imports `compaction-basic`.
- Source verification intent: @deepseek-ai/dsh-compaction-basic and its workspace/package metadata are removed. @deepseek-ai/dsh-compaction owns the current configuration, plugin class, algorithm, types, events, and shared helpers. Existing deployments can load the surviving package with equivalent configuration and model-visible behavior. Automatic and manual compaction preserve cancellation, locking, token accounting, tool pairing, durable events, cited source-event seqs, retry convergence, and transcript rendering.

## How to read the implementation

1. Start with [`packages/compaction/compaction/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and release policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/storage`, `domain/testing`, `lifecycle/rejected`
- Aliases: `summarize`, `compaction`, `CompactionResult`, `compact`, `@deepseek-ai/dsh-compaction`, `@deepseek-ai/dsh-compaction-basic`, `ctx.compaction`, `compaction-basic`, `Fold the single compaction backend into its service package`, `simplification`, `boundary`, `cancellation timeout`, `ownership`, `recovery`
- Regex: `(?i)(summarize|compaction|CompactionResult|compact|@deepseek\-ai/dsh\-compaction|@deepseek\-ai/dsh\-compaction\-basic|ctx\.compaction|compaction\-basic)`

```bash
rg -n --pcre2 "(?i)(summarize|compaction|CompactionResult|compact|@deepseek\\-ai/dsh\\-compaction|@deepseek\\-ai/dsh\\-compaction\\-basic|ctx\\.compaction|compaction\\-basic)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0007. Capability seams --- Service Definition / Service Provider / Consumer roles](0007-capability-seams-service-definition-service-provider-consumer-roles.md): The source note links to this decision directly.
- **`source-link`** — [0133. Compaction as a capability seam (abstract contract + basic backend)](0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md): The source note links to this decision directly.
- **`source-link`** — [0518. Recallable compaction --- index checkpoints, a state checkpoint, and in-session history recall](0518-recallable-compaction-index-checkpoints-a-state-checkpoint-and-in-sessio.md): The source note links to this decision directly.
- **`shares-code-with`** — [0033. After-call compaction pressure and context-overflow recovery](0033-after-call-compaction-pressure-and-context-overflow-recovery.md): Shares source implementation: `packages/bundle/headless/src/index.ts`, `packages/compaction/compaction-basic`.
- **`shares-code-with`** — [0304. The summarization call replays the conversation prefix for KV-cache reuse](0304-the-summarization-call-replays-the-conversation-prefix-for-kv-cache-reus.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.
- **`shares-code-with`** — [0666. Retire the readline front door and the repl-agent example](0666-retire-the-readline-front-door-and-the-repl-agent-example.md): Shares source implementation: `packages/compaction/compaction`, `packages/compaction/compaction/src/index.ts`.
- **`shares-code-with`** — [0343. the context meter could not see a compaction](0343-the-context-meter-could-not-see-a-compaction.md): Shares source implementation: `packages/compaction/compaction-basic`, `packages/compaction/compaction-basic/src/index.ts`.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/compaction/compaction-basic/src/index.ts`, `packages/compaction/compaction-basic/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0540-fold-the-single-compaction-backend-into-its-service-package.md`.
