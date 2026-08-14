---
id: "dsh-note-0664"
title: "Retire the standalone subagent mock package"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-07-19-retire-subagent-mock-package.md"
implementation_evidence: "high"
target_anchor: "repository tests and release policy"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/jobs-tasks"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "SubagentProvider"
  - "@deepseek-ai/dsh-subagent-mock"
  - "tool-subagent"
  - "packages/subagent/tool-subagent/tests/scripted-provider.ts"
  - "SubagentService"
  - "ToolSubagent"
  - "Retire the standalone subagent mock package"
  - "simplification"
  - "boundary"
  - "cancellation timeout"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "build release"
search_regex: "(?i)(SubagentProvider|@deepseek\\-ai/dsh\\-subagent\\-mock|tool\\-subagent|packages/subagent/tool\\-subagent/tests/scripted\\-provider\\.ts|SubagentService|ToolSubagent|Retire[- ]the[- ]standalone[- ]subagent[- ]mock[- ]package|simplification)"
---

# 0664. Retire the standalone subagent mock package — implementation context

## Open this when

@deepseek-ai/dsh-subagent-mock was a configurable test double packaged as a workspace plugin. Its only external consumers were the tool-subagent unit suite and the tool-catalog generator; no runtime package, example, snapshot configuration, or real provider loaded it. That narrow fixture carried a manifest, exports, peer and development dependencies, project references, package README obligations, Loader composition tests, module-graph membership, and documentation exceptions. The tool-catalog generator mounted it only to make production consumers register their schemas and never executed a child.

## Source decision

The standalone package is deleted. Its scripted child behavior now lives in packages/subagent/tool-subagent/tests/scripted-provider.ts, where tests mount the real SubagentService, provider registry, tool implementation, and task runtime while replacing only the nondeterministic child boundary. The local fixture retains deterministic replies, structured results, stop reasons, cancellation before and after publication, conversation-inheritance descriptors, and effect-scoped disposal. Package-specific Schemastery and Loader-export tests disappear because the fixture is no longer a deployable plugin.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-07-19-retire-subagent-mock-package.md](../02-notes/archived/simplification/2026-07-19-retire-subagent-mock-package.md)
- Pinned source: [.agents/notes/archived/simplification/2026-07-19-retire-subagent-mock-package.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-07-19-retire-subagent-mock-package.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Defines `SubagentProvider`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/tool-subagent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/README.md) | package contract and examples | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/subagent/tool-subagent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/package.json) | composition and configuration | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `SubagentProvider` | `interface` | [`packages/subagent/subagent/src/types.ts:285`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts#L285) | `export interface SubagentProvider {` |

### Tests and executable evidence

- [`packages/subagent/tool-subagent/tests/scripted-provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/scripted-provider.ts) — The source note names this file directly. A test under the owning area exercises or imports `tool-subagent`.
- [`packages/subagent/subagent/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/service.spec.ts) — A test under the owning area exercises or imports `SubagentProvider`.
- [`packages/subagent/subagent/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/invariant.spec.ts) — A test under the owning area exercises or imports `SubagentProvider`.
- [`packages/subagent/tool-subagent/tests/tool-subagent.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/tool-subagent.spec.ts) — A test under the owning area exercises or imports `tool-subagent`.

## How to read the implementation

1. Start with [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/jobs-tasks`, `domain/llm`, `domain/multi-agent`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`, `mechanism/registry`
- Aliases: `SubagentProvider`, `@deepseek-ai/dsh-subagent-mock`, `tool-subagent`, `packages/subagent/tool-subagent/tests/scripted-provider.ts`, `SubagentService`, `ToolSubagent`, `Retire the standalone subagent mock package`, `simplification`, `boundary`, `cancellation timeout`, `discovery routing`, `evidence`, `lifecycle`, `build release`
- Regex: `(?i)(SubagentProvider|@deepseek\-ai/dsh\-subagent\-mock|tool\-subagent|packages/subagent/tool\-subagent/tests/scripted\-provider\.ts|SubagentService|ToolSubagent|Retire[- ]the[- ]standalone[- ]subagent[- ]mock[- ]package|simplification)`

```bash
rg -n --pcre2 "(?i)(SubagentProvider|@deepseek\\-ai/dsh\\-subagent\\-mock|tool\\-subagent|packages/subagent/tool\\-subagent/tests/scripted\\-provider\\.ts|SubagentService|ToolSubagent|Retire[- ]the[- ]standalone[- ]subagent[- ]mock[- ]package|simplification)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0289. Continuable delegation is background-first](0289-continuable-delegation-is-background-first.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0662. Drop unconsumed skill provider events](0662-drop-unconsumed-skill-provider-events.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0120. Product subagent providers live in the shared profile host](0120-product-subagent-providers-live-in-the-shared-profile-host.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/README.md`.
- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent/src/types.ts`, `packages/subagent/tool-subagent`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/subagent/src/types.ts`, `packages/subagent/tool-subagent`.
- **`shares-code-with`** — [0153. Configure subagent persona, tool visibility, and depth](0153-configure-subagent-persona-tool-visibility-and-depth.md): Shares source implementation: `packages/subagent/subagent/src/types.ts`, `packages/subagent/tool-subagent`.
- **`shares-code-with`** — [0280. Parallel subagent delegations](0280-parallel-subagent-delegations.md): Shares source implementation: `packages/subagent/subagent/src/types.ts`, `packages/subagent/tool-subagent/src/index.ts`.
- **`shares-code-with`** — [0203. SDK max output tokens](0203-sdk-max-output-tokens.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0664-retire-the-standalone-subagent-mock-package.md`.
