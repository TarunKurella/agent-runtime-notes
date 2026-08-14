---
id: "dsh-note-0361"
title: "The slash catalog follows a blank session's preset switch"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-10-slash-catalog-follows-preset-switch.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
  - "mechanism/streaming"
aliases:
  - "CommandDirectory"
  - "sessions"
  - "select"
  - "model"
  - "plan"
  - "compact"
  - "SessionEventMap"
  - "menu contains. The Web composition disables host-plane"
  - "dsh-client-ui-commands"
  - "dsh-client-ui-skill"
  - "commands/change"
  - "connection/reset"
  - "agentPresets.recompose"
  - "agent-preset/selected"
search_regex: "(?i)(CommandDirectory|sessions|select|model|plan|compact|SessionEventMap|menu[- ]contains\\.[- ]The[- ]Web[- ]composition[- ]disables[- ]host\\-plane)"
---

# 0361. The slash catalog follows a blank session's preset switch — implementation context

## Open this when

Presets moved the rows that decide what a session's / menu contains. The Web composition disables host-plane skill-filesystem, tool-skill, plan-mode, and command-compact; a preset supplies them, so which commands and skills exist is a property of the session's composition rather than of the deployment. Both browser catalogs cache per session --- CommandDirectory in dsh-client-ui-commands, the single-flight fetch map in dsh-client-ui-skill --- and the composer warms both at scope birth, under whatever preset the session was created with.

## Source decision

The switch's commit point is the logged agent-preset/selected event. The preset owner re-emits that commit as the client-safe cordis owner event agent-preset/selected(sessionId, agentPreset), the host stream forwards it verbatim, and each catalog subscribes directly through ctx.remote.$on: ui-commands soft-refreshes the key (the old snapshot keeps serving the open menu until the new one lands), while ui-skill invalidates it (aborting an in-flight prewarm, so a warm racing the switch cannot publish the stale catalog). The owner event is per session and carries no catalog, only the preset id.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-10-slash-catalog-follows-preset-switch.md](../02-notes/implemented/bug-fix/2026-08-10-slash-catalog-follows-preset-switch.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-10-slash-catalog-follows-preset-switch.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-10-slash-catalog-follows-preset-switch.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-skill`. | `named-package-member` |
| [`packages/client/ui-commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-commands`. | `named-package-member` |
| [`packages/client/ui-skill/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-skill`. | `named-package-member` |
| [`packages/preset/agent-presets/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/index.ts) | package entry point | Core file in the package named by the note: `packages/preset/agent-presets`. | `named-package-member` |
| [`packages/preset/agent-presets/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/preset/agent-presets`. | `named-package-member` |
| [`packages/client/ui-agent-preset/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-agent-preset`. | `named-package-member` |
| [`packages/client/ui-commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-commands`. | `named-package-member` |
| [`packages/client/ui-skill/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-skill`. Defines `sessions`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/preset/agent-presets/src/session.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/session.ts) | runtime implementation | Core file in the package named by the note: `packages/preset/agent-presets`. Defines `SessionEventMap`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/preset/agent-presets/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/preset/agent-presets`. | `named-package-member` |
| [`packages/client/ui-commands/src/client/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/index.ts) | package entry point | Core file in the package named by the note: `packages/client/ui-commands`. Defines `sessions`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/client/ui-agent-preset/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/ui-agent-preset`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `CommandDirectory` | `class` | [`packages/client/ui-commands/src/client/directory.ts:34`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/directory.ts#L34) | `export class CommandDirectory {` |
| `sessions` | `const` | [`packages/client/ui-commands/src/client/index.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/index.ts#L60) | `const sessions = scope.sessions` |
| `sessions` | `const` | [`packages/client/ui-commands/src/client/service.ts:203`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts#L203) | `const sessions = this.sessions()` |
| `sessions` | `const` | [`packages/client/ui-commands/src/client/service.ts:450`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/src/client/service.ts#L450) | `const sessions = this.ctx.get('sessions')` |
| `select` | `const` | [`packages/client/ui-permission-presets/src/client/index.ts:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-permission-presets/src/client/index.ts#L118) | `const select = (preset: string): Promise<void> => controller.select(preset)` |
| `model` | `const` | [`packages/client/ui-skill/src/client/SkillRow.tsx:118`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/SkillRow.tsx#L118) | `const model = skillRowModel(block)` |
| `sessions` | `const` | [`packages/client/ui-skill/src/client/index.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/client/index.ts#L71) | `const sessions = ctx.get('sessions') as ISessions` |
| `plan` | `const` | [`packages/core/session/src/surface.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/surface.ts#L357) | `const plan = planSurfaceEvent(state, event, expectedSeq, events, baseSeq)` |
| `compact` | `const` | [`packages/jobs/tool-jobs/src/index.ts:161`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts#L161) | `const compact = \`${prefix}${action}\`` |
| `SessionEventMap` | `interface` | [`packages/preset/agent-presets/src/session.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/src/session.ts#L19) | `interface SessionEventMap {` |

### Tests and executable evidence

- [`packages/preset/agent-presets/tests/mount.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/mount.spec.ts) — A test under the owning area exercises or imports `recompose`. A test under the owning area exercises or imports `dsh-agent-presets`.
- [`packages/preset/agent-presets/tests/settings.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/settings.spec.ts) — A test under the owning area exercises or imports `dsh-agent-presets`. A test under the owning area exercises or imports `minimal`.
- [`packages/preset/agent-presets/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/invariant.spec.ts) — A test under the owning area exercises or imports `recompose`. A test under the owning area exercises or imports `dsh-agent-presets`.
- [`packages/preset/agent-presets/tests/authoring.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/authoring.spec.ts) — A test under the owning area exercises or imports `dsh-agent-presets`. A test under the owning area exercises or imports `minimal`.
- [`packages/preset/agent-presets/tests/discovery.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/discovery.spec.ts) — A test under the owning area exercises or imports `dsh-agent-presets`.
- [`packages/preset/agent-presets/tests/user-root.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/agent-presets/tests/user-root.spec.ts) — A test under the owning area exercises or imports `dsh-agent-presets`.
- [`packages/client/ui-commands/tests/service.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/tests/service.client.spec.ts) — A test under the owning area exercises or imports `$on`. Contains the exact code literal `commands/change` named by the note.
- [`packages/client/ui-commands/tests/directory.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-commands/tests/directory.client.spec.ts) — A test under the owning area exercises or imports `CommandDirectory`.
- Source verification intent: api-proxy-agent-preset.spec.ts asserts the committed switch is forwarded once with the session and its new preset; the ui-agent-preset, ui-commands, and ui-skill specs assert that direct Remote subscriptions merge the row or repull only the recomposed session. The agent-preset-selection web e2e seeds a project skill and, after the hero chip applies minimal, asserts the / menu drops compact, plan, and the skill while keeping the host-plane rows --- the assembled-application evidence that the panel follows the composition.

## How to read the implementation

1. Start with [`packages/client/ui-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-skill/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`, `mechanism/streaming`
- Aliases: `CommandDirectory`, `sessions`, `select`, `model`, `plan`, `compact`, `SessionEventMap`, `menu contains. The Web composition disables host-plane`, `dsh-client-ui-commands`, `dsh-client-ui-skill`, `commands/change`, `connection/reset`, `agentPresets.recompose`, `agent-preset/selected`
- Regex: `(?i)(CommandDirectory|sessions|select|model|plan|compact|SessionEventMap|menu[- ]contains\.[- ]The[- ]Web[- ]composition[- ]disables[- ]host\-plane)`

```bash
rg -n --pcre2 "(?i)(CommandDirectory|sessions|select|model|plan|compact|SessionEventMap|menu[- ]contains\\.[- ]The[- ]Web[- ]composition[- ]disables[- ]host\\-plane)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0360. The session-row identity guard covers the preset](0360-the-session-row-identity-guard-covers-the-preset.md): The source note links to this decision directly.
- **`shares-code-with`** — [0295. Web `/export` shares the streamed Session ZIP download](0295-web-export-shares-the-streamed-session-zip-download.md): Shares source implementation: `packages/client/ui-commands/src/index.ts`, `packages/client/ui-commands/src/invariant.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0278. Web background-job display](0278-web-background-job-display.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0361-the-slash-catalog-follows-a-blank-session-s-preset-switch.md`.
