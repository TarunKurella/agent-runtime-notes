---
id: "dsh-note-0114"
title: "An independent Events backstop closes the cordis-surface exhaustiveness gap"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-08-09-cordis-event-walk-backstop.md"
implementation_evidence: "high"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "connection"
  - "commands"
  - "eventNameList"
  - "SERVICE_PAGE"
  - "SERVICE_WALK_EXEMPTIONS"
  - "EVENT_SCOPE_PAGE"
  - "EVENT_WALK_EXEMPTIONS"
  - "LINK_MAP"
  - "FOUNDATION_TYPE_NAMES"
  - "TYPE_LINK_EXEMPTIONS"
  - "walkPartitionProblems"
  - "computeOutputs"
  - "gen-cordis-catalog"
  - "docs/subsystems/"
search_regex: "(?i)(connection|commands|eventNameList|SERVICE_PAGE|SERVICE_WALK_EXEMPTIONS|EVENT_SCOPE_PAGE|EVENT_WALK_EXEMPTIONS|LINK_MAP)"
---

# 0114. An independent Events backstop closes the cordis-surface exhaustiveness gap — implementation context

## Open this when

gen-cordis-catalog renders every service and event the Typert host-face projection discovers, and fail-closed page maps (SERVICE_PAGE, EVENT_SCOPE_PAGE) guarantee each discovered key or scope lands on exactly one docs/subsystems/ page (per-subsystem regions decision owns the page-region mechanism). Discovery itself was only backstopped for services: an independent AST scan read every declare module 'cordis' Context merge and demanded each declared key be rendered or carry a named SERVICE_WALK_EXEMPTIONS reason. Events had no such backstop.

## Source decision

Events get the exact mirror of the services backstop, and both scans read the full package source tree. scripts/cordis-walk.ts gains eventNameList (every member name of an interface Events merge, read from method and property members alike so a shape the projector would reject still enters the scan); the scan yields every declare module 'cordis' block in a file (the Typert analyzer reads them all, so stopping at the first would hide a second block's face), and its quote-agnostic prefilter matches the declare module heads instead of the literal text interface Context, so an Events-only or double-quoted merge.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-08-09-cordis-event-walk-backstop.md](../02-notes/implemented/architecture/2026-08-09-cordis-event-walk-backstop.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-08-09-cordis-event-walk-backstop.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-08-09-cordis-event-walk-backstop.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/cordis-walk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-walk.ts) | repository automation | The source note names this file directly. Defines `eventNameList`, a construct named by the note. | `named-file, symbol-definition` |
| [`packages/client/connection/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts) | package entry point | Core file in the package named by the note: `packages/client/connection`. Defines `connection`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/interaction/commands/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/interaction/commands/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`packages/client/connection/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/client/connection`. | `named-package-member` |
| [`packages/interaction/commands/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/commands`. | `named-package-member` |
| [`docs/subsystems/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `docs/subsystems`. | `named-directory-member` |
| [`docs/subsystems`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/client/connection`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/interaction/commands`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/commands) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | Defines `SERVICE_PAGE`, a construct named by the note. Defines `EVENT_SCOPE_PAGE`, a construct named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/hooks/hooks-codex/src/config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts) | runtime implementation | Defines `commands`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `connection` | `const` | [`packages/client/connection/src/index.ts:138`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/connection/src/index.ts#L138) | `const connection = new HostConnectionService(ctx, trustedHosts)` |
| `commands` | `const` | [`packages/hooks/hooks-codex/src/config.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts#L60) | `const commands: MatcherGroup['hooks'] = []` |
| `eventNameList` | `function` | [`scripts/cordis-walk.ts:90`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-walk.ts#L90) | `export function eventNameList(body: ts.ModuleBlock, sf: ts.SourceFile): string[] {` |
| `SERVICE_PAGE` | `const` | [`scripts/gen-cordis-catalog.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L52) | `export const SERVICE_PAGE: Record<string, string> = {` |
| `SERVICE_WALK_EXEMPTIONS` | `const` | [`scripts/gen-cordis-catalog.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L129) | `export const SERVICE_WALK_EXEMPTIONS: Record<string, string> = {` |
| `EVENT_SCOPE_PAGE` | `const` | [`scripts/gen-cordis-catalog.ts:165`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L165) | `export const EVENT_SCOPE_PAGE: Record<string, string> = {` |
| `EVENT_WALK_EXEMPTIONS` | `const` | [`scripts/gen-cordis-catalog.ts:197`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L197) | `export const EVENT_WALK_EXEMPTIONS: Record<string, string> = {` |
| `LINK_MAP` | `const` | [`scripts/gen-cordis-catalog.ts:215`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L215) | `export const LINK_MAP: Readonly<Record<string, string>> = {` |
| `FOUNDATION_TYPE_NAMES` | `const` | [`scripts/gen-cordis-catalog.ts:492`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L492) | `export const FOUNDATION_TYPE_NAMES: ReadonlySet<string> = new Set([` |
| `TYPE_LINK_EXEMPTIONS` | `const` | [`scripts/gen-cordis-catalog.ts:507`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L507) | `export const TYPE_LINK_EXEMPTIONS: Readonly<Record<string, string>> = {` |
| `walkPartitionProblems` | `function` | [`scripts/gen-cordis-catalog.ts:714`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L714) | `export function walkPartitionProblems(input: WalkPartitionInput, maps: WalkPartitionMaps): string[] {` |
| `computeOutputs` | `function` | [`scripts/gen-cordis-catalog.ts:781`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L781) | `export function computeOutputs(): [string, string][] {` |

### Tests and executable evidence

- [`scripts/gen-cordis-catalog-partition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog-partition.spec.ts) — The source note names this file directly. Contains the exact code literal `theme/change` named by the note.
- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — Contains the exact code literal `docs/subsystems/` named by the note.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — Contains the exact code literal `theme/change` named by the note.
- [`packages/client/locale/tests/locale.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/locale/tests/locale.client.spec.ts) — Contains the exact code literal `locale/change` named by the note.
- [`packages/client/ui-layout/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/apply.client.spec.ts) — Contains the exact code literal `theme/change` named by the note.
- [`packages/typert/generator/tests/cordis-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/cordis-catalog.spec.ts) — Contains the exact code literal `docs/subsystems/` named by the note.
- Source verification intent: scripts/gen-cordis-catalog-partition.spec.ts proves each acceptance path: the green partition, an invisible unexempted event (named with its declaring file), a stale rendered-event exemption, a stale never-declared exemption, the service mirror of each, unmapped rendered surface in both page maps, rendered surface the scan cannot see (the third direction), and the scan reaching nested Events-only merges, every block of a multi-block file, double-quoted heads, and .tsx sources.

## How to read the implementation

1. Start with [`scripts/cordis-walk.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-walk.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `connection`, `commands`, `eventNameList`, `SERVICE_PAGE`, `SERVICE_WALK_EXEMPTIONS`, `EVENT_SCOPE_PAGE`, `EVENT_WALK_EXEMPTIONS`, `LINK_MAP`, `FOUNDATION_TYPE_NAMES`, `TYPE_LINK_EXEMPTIONS`, `walkPartitionProblems`, `computeOutputs`, `gen-cordis-catalog`, `docs/subsystems/`
- Regex: `(?i)(connection|commands|eventNameList|SERVICE_PAGE|SERVICE_WALK_EXEMPTIONS|EVENT_SCOPE_PAGE|EVENT_WALK_EXEMPTIONS|LINK_MAP)`

```bash
rg -n --pcre2 "(?i)(connection|commands|eventNameList|SERVICE_PAGE|SERVICE_WALK_EXEMPTIONS|EVENT_SCOPE_PAGE|EVENT_WALK_EXEMPTIONS|LINK_MAP)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0416. Per-subsystem generated cordis-surface regions](0416-per-subsystem-generated-cordis-surface-regions.md): The source note links to this decision directly.
- **`shares-code-with`** — [0215. Queued manual compaction with one durable lock](0215-queued-manual-compaction-with-one-durable-lock.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0202. `/feedback` command](0202-feedback-command.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0160. Human `/goal` command](0160-human-goal-command.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0163. Plugin-owned human command registration](0163-plugin-owned-human-command-registration.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.
- **`shares-code-with`** — [0099. WebSocket carrier for browser downlinks](0099-websocket-carrier-for-browser-downlinks.md): Shares source implementation: `packages/client/connection`, `packages/client/connection/src/index.ts`.
- **`shares-code-with`** — [0069. One carrier-level browser-trust boundary for all `/api` routes](0069-one-carrier-level-browser-trust-boundary-for-all-api-routes.md): Shares source implementation: `packages/client/connection`, `packages/client/connection/src/index.ts`.
- **`shares-code-with`** — [0062. Web command business surfaces and assembly (ui-commands / ui-skill / ui-subagent)](0062-web-command-business-surfaces-and-assembly-ui-commands-ui-skill-ui-subag.md): Shares source implementation: `packages/interaction/commands/src/index.ts`, `packages/interaction/commands/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0114-an-independent-events-backstop-closes-the-cordis-surface-exhaustiveness.md`.
