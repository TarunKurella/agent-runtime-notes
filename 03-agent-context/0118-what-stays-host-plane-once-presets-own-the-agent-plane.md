---
id: "dsh-note-0118"
title: "What stays host-plane once presets own the agent plane"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-10-host-plane-ownership-after-presets.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "compaction"
  - "standard"
  - "snapshot"
  - "tasks"
  - "checkpoint"
  - "presenterScopeFor"
  - "goals"
  - "active"
  - "AgentPresets"
  - "get"
  - "skill"
  - "toolFilter"
  - "dsh-token-meter"
  - "tokenUsage"
search_regex: "(?i)(compaction|standard|snapshot|tasks|checkpoint|presenterScopeFor|goals|active)"
---

# 0118. What stays host-plane once presets own the agent plane — implementation context

## Open this when

Per-session agent presets moved every model-facing row onto the agent plane, and each later fix has been one reader that assumed the world before the move. tasks came back to the host because a preset row outside its realm resolved it; goals never left for the same reason; a child agent's toolFilter was repaired once every model-facing tool became an ancestor contribution rather than a global one (child agents join their parent's preset). Two more readers were still on the wrong side of that line. dsh-token-meter was disabled on the host and mounted inside each preset's compaction realm.

## Source decision

The meter is host-plane. dsh-token-meter returns to the host composition and leaves the presets' isolate map, so compaction-basic and tool-result-pruner resolve the one host instance from inside their realm. The presets keep the realm and the backend --- what a preset chooses is whether its agent compacts, not whether its tokens are counted.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-10-host-plane-ownership-after-presets.md](../02-notes/implemented/architecture/2026-08-10-host-plane-ownership-after-presets.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-10-host-plane-ownership-after-presets.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-10-host-plane-ownership-after-presets.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/preset/agent-presets/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/preset/agent-presets`. | `named-file, named-package-member` |
| [`packages/extensions/tool-cordis/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/tool-cordis/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/extensions/tool-cordis`. | `named-file, named-package-member` |
| [`packages/session/session-projection/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/session/session-projection/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/session/session-projection`. | `named-file, named-package-member` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/session/src/json.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts) | runtime implementation | Core file in the package named by the note: `packages/core/session`. Defines `tasks`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools/src/schema.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `tasks`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. Defines `snapshot`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/code-mode.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts) | runtime implementation | Core file in the package named by the note: `packages/core/tools`. Defines `tasks`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `compaction` | `const` | [`packages/client/ui-trajectory/src/client/layout.ts:339`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-trajectory/src/client/layout.ts#L339) | `const compaction: TurnBucket = {` |
| `standard` | `let` | [`packages/client/web-react/src/scoped-slots.tsx:364`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web-react/src/scoped-slots.tsx#L364) | `let standard = byInfo.get(info)` |
| `snapshot` | `const` | [`packages/core/session/src/index.ts:154`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L154) | `const snapshot = snapshotJsonValue(input)` |
| `snapshot` | `const` | [`packages/core/session/src/index.ts:519`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L519) | `const snapshot = mode === 'restore' ? source : snapshotJsonValue(source)` |
| `tasks` | `const` | [`packages/core/session/src/json.ts:89`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/json.ts#L89) | `const tasks: JsonWalkTask[] = [{` |
| `tasks` | `const` | [`packages/core/tools/src/code-mode.ts:187`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/code-mode.ts#L187) | `const tasks: JsonRenderTask[] = [{ kind: 'value', value, depth: 0, compact: false }]` |
| `tasks` | `const` | [`packages/core/tools/src/json-schema.ts:228`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/json-schema.ts#L228) | `const tasks: SchemaWalkTask[] = [{ kind: 'enter', node: root, path: rootPath }]` |
| `tasks` | `const` | [`packages/core/tools/src/schema.ts:277`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/schema.ts#L277) | `const tasks: CompileTask[] = [initial]` |
| `tasks` | `const` | [`packages/core/tools/src/ts-types.ts:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/ts-types.ts#L79) | `const tasks: (string \| TypeDocument)[] = [document]` |
| `checkpoint` | `const` | [`packages/examples/acp-demo/src/index.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/acp-demo/src/index.ts#L131) | `const checkpoint = ctx.plugin(sessionCheckpointPolicy)` |
| `presenterScopeFor` | `function` | [`packages/host/apiproxy/src/api-proxy.ts:1596`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1596) | `async function presenterScopeFor(` |
| `goals` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:1798`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L1798) | `const goals = presets?.serviceFor(agent, 'goals') ?? ctx.get('goals')` |
| `active` | `let` | [`packages/plan/plan-mode/src/index.ts:130`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts#L130) | `let active = false` |
| `active` | `const` | [`packages/plan/plan-mode/src/index.ts:404`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts#L404) | `const active = foldPlanMode(agent.session.events)` |
| `active` | `const` | [`packages/plan/plan-mode/src/invariant.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/invariant.ts#L22) | `const active = (event.data as { active?: unknown }).active` |
| `AgentPresets` | `class` | [`packages/preset/agent-presets/src/index.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/index.ts#L82) | `export class AgentPresets extends Service {` |

### Tests and executable evidence

- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — The source note names this file directly. Contains the exact code literal `dsh-agent-presets` named by the note.
- [`packages/preset/agent-presets/tests/mount.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/mount.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `AgentPresets`.
- [`packages/skill/skill/tests/skill.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/tests/skill.spec.ts) — A test under the owning area exercises or imports `recompose`.
- [`packages/todo/tool-todo/tests/tool-todo.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/tool-todo.spec.ts) — A test under the owning area exercises or imports `tool-todo`.
- [`packages/plan/plan-mode/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/projection.spec.ts) — A test under the owning area exercises or imports `sessionProjections`.
- [`packages/todo/tool-todo/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/todo/tool-todo/tests/projection.spec.ts) — A test under the owning area exercises or imports `dsh-session-projection`. A test under the owning area exercises or imports `tool-todo`.
- [`packages/llm/token-meter/tests/token-meter.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/tests/token-meter.spec.ts) — A test under the owning area exercises or imports `dsh-token-meter`.
- [`packages/preset/agent-presets/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/settings.spec.ts) — A test under the owning area exercises or imports `minimal`. A test under the owning area exercises or imports `AgentPresets`.
- Source verification intent: apps/cli/tests/web-agent-presets.e2e.ts reads ctx.get('tokenMeter') on the booted Web composition before any preset in the file mounts --- a preset-side meter sits behind an isolate realm and is invisible to ctx.get, so the read is an ownership assertion rather than a mount-order coincidence --- then asserts a minimal session's snapshot carries all three units. packages/preset/agent-presets/tests/mount.spec.ts asserts the warning fires exactly once for a bare agent and not at all for a joined one.

## How to read the implementation

1. Start with [`packages/preset/agent-presets/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/README.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/projection`, `mechanism/registry`
- Aliases: `compaction`, `standard`, `snapshot`, `tasks`, `checkpoint`, `presenterScopeFor`, `goals`, `active`, `AgentPresets`, `get`, `skill`, `toolFilter`, `dsh-token-meter`, `tokenUsage`
- Regex: `(?i)(compaction|standard|snapshot|tasks|checkpoint|presenterScopeFor|goals|active)`

```bash
rg -n --pcre2 "(?i)(compaction|standard|snapshot|tasks|checkpoint|presenterScopeFor|goals|active)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): The source note links to this decision directly.
- **`source-link`** — [0357. Child agents join their parent's preset composition](0357-child-agents-join-their-parent-s-preset-composition.md): The source note links to this decision directly.
- **`shares-code-with`** — [0149. The self-referential cordis toolset](0149-the-self-referential-cordis-toolset.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0051. Unified JSON-value schema DSL](0051-unified-json-value-schema-dsl.md): Shares source implementation: `packages/core/session/src/json.ts`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0147. MCP client plugin --- connect to external MCP servers and bridge their tools](0147-mcp-client-plugin-connect-to-external-mcp-servers-and-bridge-their-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0142. Dynamic workflows --- a script-driven multi-agent orchestration seam](0142-dynamic-workflows-a-script-driven-multi-agent-orchestration-seam.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/schema.ts`.
- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/session/src/json.ts`, `packages/core/tools/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0118-what-stays-host-plane-once-presets-own-the-agent-plane.md`.
