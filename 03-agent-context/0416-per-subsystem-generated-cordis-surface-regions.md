---
id: "dsh-note-0416"
title: "Per-subsystem generated cordis-surface regions"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-28-per-subsystem-cordis-surface-regions.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "ctx"
  - "renderPageRegion"
  - "SERVICE_PAGE"
  - "SERVICE_WALK_EXEMPTIONS"
  - "EVENT_SCOPE_PAGE"
  - "EVENT_WALK_EXEMPTIONS"
  - "partitionGeneratedRegions"
  - "ctx.<key>"
  - "docs/cordis-catalog/services.md"
  - "docs/cordis-catalog/events.md"
  - "ts cordis-catalog"
  - "gen-cordis-catalog.ts"
  - "<!-- BEGIN GENERATED cordis-surface … -->"
  - "<!-- END GENERATED cordis-surface -->"
search_regex: "(?i)(renderPageRegion|SERVICE_PAGE|SERVICE_WALK_EXEMPTIONS|EVENT_SCOPE_PAGE|EVENT_WALK_EXEMPTIONS|partitionGeneratedRegions|ctx\\.<key>|docs/cordis\\-catalog/services\\.md)"
---

# 0416. Per-subsystem generated cordis-surface regions — implementation context

## Open this when

One subsystem's documentation was split across three homes: its hand-written subsystems page (introduction, data structures, verbs), its ctx. slice of the flat generated docs/cordis-catalog/services.md, and its event scope's slice of the flat docs/cordis-catalog/events.md. A reader of shell.md had to open two more documents to see the service interface and events the page was describing, and nothing tied the three views together beyond hand-maintained links.

## Source decision

gen-cordis-catalog.ts injects each subsystem's service and event reference INTO its own page, between / markers, and the flat services/events catalogs are deleted. One page per subsystem now carries introduction, data structures, and the generated wiring surface. Curated fail-loud partition. SERVICE_PAGE maps every discovered ctx. to exactly one page; EVENT_SCOPE_PAGE maps every event scope. The generator hard-errors in both directions --- an unmapped discovered service/scope, and a mapped key/scope the walk no longer discovers --- so the partition cannot drift from the source surface.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-28-per-subsystem-cordis-surface-regions.md](../02-notes/implemented/process/2026-07-28-per-subsystem-cordis-surface-regions.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-28-per-subsystem-cordis-surface-regions.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-28-per-subsystem-cordis-surface-regions.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/cordis-api/inherited.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/inherited.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/subsystems/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `docs/subsystems`. | `named-directory-member` |
| [`docs/cordis-api`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`docs/subsystems`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/docs/subsystems) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`apps/cli/src/profile-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts) | repository automation | Defines `SERVICE_PAGE`, a construct named by the note. Defines `EVENT_SCOPE_PAGE`, a construct named by the note. | `exact-code-occurrence, symbol-definition` |
| [`scripts/translation-pairing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.ts) | repository automation | Defines `partitionGeneratedRegions`, a construct named by the note. Contains the exact code literal `docs/cordis-api/inherited.md` named by the note. | `exact-code-occurrence, symbol-definition` |
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/client/web/src/boot.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx) | runtime implementation | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Defines `ctx`, a construct named by the note. | `symbol-definition` |
| [`packages/typert/generator/src/cordis-catalog.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts) | runtime implementation | Defines `renderPageRegion`, a construct named by the note. | `symbol-definition` |
| [`website/docs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts) | runtime implementation | Contains the exact code literal `docs/cordis-api/` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ctx` | `const` | [`apps/cli/src/profile-boot.ts:248`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/profile-boot.ts#L248) | `const ctx = await boot(NAME, rootConfig, structuredClone(allPatches(composed)), (hostCtx) => {` |
| `ctx` | `const` | [`packages/boot/app-boot/src/index.ts:764`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L764) | `const ctx = new Context()` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L162) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/client/web/src/boot.tsx:217`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/web/src/boot.tsx#L217) | `const ctx = this.ctx` |
| `ctx` | `const` | [`packages/core/tools/src/index.ts:947`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L947) | `const ctx = this.ctx` |
| `renderPageRegion` | `function` | [`packages/typert/generator/src/cordis-catalog.ts:1005`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/src/cordis-catalog.ts#L1005) | `export function renderPageRegion(page: string, services: ServiceEntry[], events: EventEntry[], policy: CordisCatalogPolicy): string {` |
| `SERVICE_PAGE` | `const` | [`scripts/gen-cordis-catalog.ts:52`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L52) | `export const SERVICE_PAGE: Record<string, string> = {` |
| `SERVICE_WALK_EXEMPTIONS` | `const` | [`scripts/gen-cordis-catalog.ts:129`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L129) | `export const SERVICE_WALK_EXEMPTIONS: Record<string, string> = {` |
| `EVENT_SCOPE_PAGE` | `const` | [`scripts/gen-cordis-catalog.ts:165`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L165) | `export const EVENT_SCOPE_PAGE: Record<string, string> = {` |
| `EVENT_WALK_EXEMPTIONS` | `const` | [`scripts/gen-cordis-catalog.ts:197`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog.ts#L197) | `export const EVENT_WALK_EXEMPTIONS: Record<string, string> = {` |
| `partitionGeneratedRegions` | `function` | [`scripts/translation-pairing.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.ts#L35) | `export function partitionGeneratedRegions(content: string): { regions: string[]; stripped: string } {` |

### Tests and executable evidence

- [`scripts/translation-pairing.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `partitionGeneratedRegions`.
- [`scripts/gen-cordis-catalog-record.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog-record.spec.ts) — The source note names this file directly.
- [`packages/typert/generator/tests/cordis-catalog-contract.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/cordis-catalog-contract.spec.ts) — The source note names this file directly. A test under the owning area exercises or imports `renderPageRegion`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `TODO`.
- [`scripts/translation-brief.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-brief.spec.ts) — A test under the owning area exercises or imports `verify-translation-pairing`.
- [`scripts/translation-pairing-merge.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing-merge.spec.ts) — A test under the owning area exercises or imports `verify-translation-pairing`.
- [`scripts/gen-cordis-catalog-partition.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-cordis-catalog-partition.spec.ts) — A test under the owning area exercises or imports `SERVICE_PAGE`. A test under the owning area exercises or imports `EVENT_SCOPE_PAGE`.
- [`packages/typert/generator/tests/cordis-catalog.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/cordis-catalog.spec.ts) — A test under the owning area exercises or imports `SERVICE_PAGE`. A test under the owning area exercises or imports `EVENT_SCOPE_PAGE`.

## How to read the implementation

1. Start with [`docs/cordis-api/inherited.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/cordis-api/inherited.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `ctx`, `renderPageRegion`, `SERVICE_PAGE`, `SERVICE_WALK_EXEMPTIONS`, `EVENT_SCOPE_PAGE`, `EVENT_WALK_EXEMPTIONS`, `partitionGeneratedRegions`, `ctx.<key>`, `docs/cordis-catalog/services.md`, `docs/cordis-catalog/events.md`, `ts cordis-catalog`, `gen-cordis-catalog.ts`, `<!-- BEGIN GENERATED cordis-surface … -->`, `<!-- END GENERATED cordis-surface -->`
- Regex: `(?i)(renderPageRegion|SERVICE_PAGE|SERVICE_WALK_EXEMPTIONS|EVENT_SCOPE_PAGE|EVENT_WALK_EXEMPTIONS|partitionGeneratedRegions|ctx\.<key>|docs/cordis\-catalog/services\.md)`

```bash
rg -n --pcre2 "(?i)(renderPageRegion|SERVICE_PAGE|SERVICE_WALK_EXEMPTIONS|EVENT_SCOPE_PAGE|EVENT_WALK_EXEMPTIONS|partitionGeneratedRegions|ctx\\.<key>|docs/cordis\\-catalog/services\\.md)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0632. Generated cordis events + services catalog](0632-generated-cordis-events-services-catalog.md): The source note links to this decision directly.
- **`source-link`** — [0114. An independent Events backstop closes the cordis-surface exhaustiveness gap](0114-an-independent-events-backstop-closes-the-cordis-surface-exhaustiveness.md): The source note links to this decision directly.
- **`shares-code-with`** — [0644. Personal staging maintenance skills](0644-personal-staging-maintenance-skills.md): Shares source implementation: `scripts/translation-pairing.spec.ts`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `scripts/gen-cordis-catalog.ts`.
- **`shares-code-with`** — [0639. Generate the Cordis core API reference](0639-generate-the-cordis-core-api-reference.md): Shares source implementation: `apps/cli/src/profile-boot.ts`, `scripts/gen-cordis-catalog.ts`.
- **`shares-code-with`** — [0383. Subsystems catalog and the `ts type-equiv` drift gate](0383-subsystems-catalog-and-the-ts-type-equiv-drift-gate.md): Shares source implementation: `docs/subsystems/README.md`.
- **`shares-code-with`** — [0554. dsh-tui chat channel module split](0554-dsh-tui-chat-channel-module-split.md): Shares source implementation: `apps/cli/src/profile-boot.ts`.
- **`shares-code-with`** — [0042. Explicit turn cancellation capability](0042-explicit-turn-cancellation-capability.md): Shares source implementation: `apps/cli/src/profile-boot.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0416-per-subsystem-generated-cordis-surface-regions.md`.
