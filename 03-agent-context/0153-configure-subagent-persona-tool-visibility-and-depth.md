---
id: "dsh-note-0153"
title: "Configure subagent persona, tool visibility, and depth"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-12-subagent-persona-tool-filter-and-depth.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "ToolRuntime"
  - "allow"
  - "deny"
  - "persona"
  - "toolFilter"
  - "SubagentRuntime"
  - "SubagentCapabilities"
  - "SubagentStartRequest"
  - "maxDepth"
  - "dsh-tool-subagent"
  - "ToolRuntime.restrict"
  - "run_code"
  - "SessionHeader.delegationDepth"
  - "delegationDepth"
search_regex: "(?i)(ToolRuntime|allow|deny|persona|toolFilter|SubagentRuntime|SubagentCapabilities|SubagentStartRequest)"
---

# 0153. Configure subagent persona, tool visibility, and depth — implementation context

## Open this when

A reusable subagent provider answers how to run a child, but different delegation tools need different child behavior. One deployment may want a reviewer persona, a research-only tool set, or a hard recursion bound without creating a new provider for every combination. These controls affect the child's first model request and therefore cannot be installed after the child is visible. They also need honest provider support: an ACP backend cannot silently accept an in-process-only tool filter, and a filter must not be described as a security boundary when every plugin runs in the same trusted process.

## Source decision

Subagent starts have three independent composition controls: persona, toolFilter, and maxDepth. A provider advertises support for each control, the service rejects unsupported requests before starting a run, and an in-process provider installs the requested composition while the child is still unpublished. The controls answer different questions: dsh-tool-subagent exposes the controls as plugin configuration and copies them into each request it creates. Direct SubagentRuntime callers may choose them per request.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-12-subagent-persona-tool-filter-and-depth.md](../02-notes/implemented/feature/2026-07-12-subagent-persona-tool-filter-and-depth.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-12-subagent-persona-tool-filter-and-depth.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-12-subagent-persona-tool-filter-and-depth.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/jsonrpc-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/preset/persona/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/src/index.ts) | package entry point | Core file in the package named by the note: `packages/preset/persona`. | `named-package-member` |
| [`packages/preset/persona/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/preset/persona`. | `named-package-member` |
| [`packages/subagent/tool-subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts) | package entry point | Core file in the package named by the note: `packages/subagent/tool-subagent`. Defines `maxDepth`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/subagent/tool-subagent/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/subagent/tool-subagent`. | `named-package-member` |
| [`packages/preset/persona`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/subagent/tool-subagent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ToolRuntime`, a construct named by the note. Defines `allow`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts) | package entry point | Defines `SubagentRuntime`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts) | public types and contract | Defines `SubagentStartRequest`, a construct named by the note. Defines `SubagentCapabilities`, a construct named by the note. | `symbol-definition` |
| [`packages/subagent/subagent/src/descriptor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts) | runtime implementation | Defines `persona`, a construct named by the note. Defines `toolFilter`, a construct named by the note. | `symbol-definition` |
| [`packages/preset/persona/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/README.md) | package contract and examples | Core file in the package named by the note: `packages/preset/persona`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ToolRuntime` | `class` | [`packages/core/tools/src/index.ts:787`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L787) | `export class ToolRuntime extends Service {` |
| `allow` | `const` | [`packages/core/tools/src/index.ts:1076`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1076) | `const allow = filter.allow` |
| `deny` | `const` | [`packages/core/tools/src/index.ts:1077`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1077) | `const deny = filter.deny` |
| `persona` | `const` | [`packages/subagent/subagent/src/descriptor.ts:234`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts#L234) | `const persona = optionalString(value, 'persona')` |
| `toolFilter` | `const` | [`packages/subagent/subagent/src/descriptor.ts:235`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/descriptor.ts#L235) | `const toolFilter = Object.hasOwn(value, 'toolFilter')` |
| `SubagentRuntime` | `class` | [`packages/subagent/subagent/src/index.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/index.ts#L171) | `export class SubagentRuntime extends Service {` |
| `SubagentCapabilities` | `interface` | [`packages/subagent/subagent/src/types.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts#L86) | `export interface SubagentCapabilities {` |
| `SubagentStartRequest` | `interface` | [`packages/subagent/subagent/src/types.ts:100`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/src/types.ts#L100) | `export interface SubagentStartRequest {` |
| `maxDepth` | `const` | [`packages/subagent/tool-subagent/src/index.ts:376`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/src/index.ts#L376) | `const maxDepth = typeof config.maxDepth === 'number' ? config.maxDepth : undefined` |

### Tests and executable evidence

- [`packages/core/tools/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/tools.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`. A test under the owning area exercises or imports `deny`.
- [`packages/core/tools/tests/scoped.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/scoped.spec.ts) — A test under the owning area exercises or imports `allow`. A test under the owning area exercises or imports `deny`.
- [`packages/core/tools/tests/code-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/code-mode.spec.ts) — A test under the owning area exercises or imports `allow`. A test under the owning area exercises or imports `deny`.
- [`packages/core/tools/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/invariant.spec.ts) — A test under the owning area exercises or imports `allow`.
- [`packages/preset/persona/tests/persona.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/tests/persona.spec.ts) — A test under the owning area exercises or imports `persona`.
- [`packages/core/tools/tests/execution-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/tests/execution-mode.spec.ts) — A test under the owning area exercises or imports `ToolRuntime`.
- [`packages/subagent/subagent/tests/service.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/subagent/tests/service.spec.ts) — A test under the owning area exercises or imports `SubagentStartRequest`. A test under the owning area exercises or imports `SubagentCapabilities`.
- [`packages/subagent/tool-subagent/tests/scripted-provider.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/subagent/tool-subagent/tests/scripted-provider.ts) — A test under the owning area exercises or imports `toolFilter`. A test under the owning area exercises or imports `SubagentStartRequest`.

## How to read the implementation

1. Start with [`examples/jsonrpc-agent/cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/cordis.yml) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/storage`, `domain/tools`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/registry`
- Aliases: `ToolRuntime`, `allow`, `deny`, `persona`, `toolFilter`, `SubagentRuntime`, `SubagentCapabilities`, `SubagentStartRequest`, `maxDepth`, `dsh-tool-subagent`, `ToolRuntime.restrict`, `run_code`, `SessionHeader.delegationDepth`, `delegationDepth`
- Regex: `(?i)(ToolRuntime|allow|deny|persona|toolFilter|SubagentRuntime|SubagentCapabilities|SubagentStartRequest)`

```bash
rg -n --pcre2 "(?i)(ToolRuntime|allow|deny|persona|toolFilter|SubagentRuntime|SubagentCapabilities|SubagentStartRequest)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0148. Background subagent tasks](0148-background-subagent-tasks.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/types.ts`.
- **`shares-code-with`** — [0462. Merge subagent control into the subagent service](0462-merge-subagent-control-into-the-subagent-service.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/types.ts`.
- **`shares-code-with`** — [0280. Parallel subagent delegations](0280-parallel-subagent-delegations.md): Shares source implementation: `packages/subagent/subagent/src/index.ts`, `packages/subagent/subagent/src/types.ts`.
- **`shares-code-with`** — [0117. Forked children stay one-shot](0117-forked-children-stay-one-shot.md): Shares source implementation: `packages/preset/persona/src/index.ts`, `packages/preset/persona/src/invariant.ts`.
- **`shares-code-with`** — [0664. Retire the standalone subagent mock package](0664-retire-the-standalone-subagent-mock-package.md): Shares source implementation: `packages/subagent/subagent/src/types.ts`, `packages/subagent/tool-subagent`.
- **`shares-code-with`** — [0031. The agent is a registration scope](0031-the-agent-is-a-registration-scope.md): Shares source implementation: `packages/preset/persona`, `packages/preset/persona/src/index.ts`.
- **`shares-code-with`** — [0285. Delegated subagents run with approvals pinned to `'never'`](0285-delegated-subagents-run-with-approvals-pinned-to-never.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/subagent/subagent/src/index.ts`.
- **`shares-code-with`** — [0203. SDK max output tokens](0203-sdk-max-output-tokens.md): Shares source implementation: `packages/subagent/tool-subagent`, `packages/subagent/tool-subagent/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0153-configure-subagent-persona-tool-visibility-and-depth.md`.
