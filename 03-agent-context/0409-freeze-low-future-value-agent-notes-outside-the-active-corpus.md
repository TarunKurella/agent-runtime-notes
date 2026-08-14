---
id: "dsh-note-0409"
title: "Freeze low-future-value Agent Notes outside the active corpus"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-26-frozen-agent-note-archive.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/security"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
aliases:
  - ".agents/notes/archived/{kind}/yyyy-mm-dd-topic.md"
  - "Archived: YYYY-MM-DD"
  - ".rgignore"
  - "verify-archived-agent-notes"
  - "--write"
  - "dsh-archive-agent-notes"
  - "Freeze low-future-value Agent Notes outside the active corpus"
  - "process"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "recovery"
search_regex: "(?i)(\\.agents/notes/archived/\\{kind\\}/yyyy\\-mm\\-dd\\-topic\\.md|Archived:[- ]YYYY\\-MM\\-DD|\\.rgignore|verify\\-archived\\-agent\\-notes|\\-\\-write|dsh\\-archive\\-agent\\-notes|boundary|discovery[- ]routing)"
---

# 0409. Freeze low-future-value Agent Notes outside the active corpus — implementation context

## Open this when

Implemented Agent Notes are maintained as current decision records, so every path, symbol, default, translation, code fence, package reference, and outbound link in the active corpus remains an obligation. That cost is justified when the rationale can guide future work, but not for closed UI details, minor fixes, superseded implementation mechanics, or process history whose current authority lives elsewhere. Deleting every low-value implemented record would erase useful historical evidence, while retaining every rejected proposal preserves ideas that are neither plausible nor instructive.

## Source decision

Only implemented Agent Notes can be archived. An implemented note moves when its shipped decision is complete and its rationale, alternatives, consequences, negative guarantees, and reintroduction conditions are unlikely to guide future work. Foundational boundaries, durable and wire semantics, security rules, recurring design temptations, and unresolved reintroduction conditions remain active regardless of age or word count. Proposed notes never enter the archive; an obsolete proposal becomes rejected.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-26-frozen-agent-note-archive.md](../02-notes/implemented/process/2026-07-26-frozen-agent-note-archive.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-26-frozen-agent-note-archive.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-26-frozen-agent-note-archive.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/verify-archived-agent-notes.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-archived-agent-notes.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-archive-agent-notes/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-archive-agent-notes/SKILL.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `dsh-archive-agent-notes` named by the note. | `exact-code-occurrence, named-file` |
| [`.agents/skills/dsh-find-simplifications/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-find-simplifications/SKILL.md) | package contract and examples | Contains the exact code literal `dsh-archive-agent-notes` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`scripts/archived-agent-notes.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/archived-agent-notes.spec.ts) — A test under the owning area exercises or imports `implemented`.
- [`scripts/verify-public-repository-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-public-repository-links.spec.ts) — A test under the owning area exercises or imports `implemented`.

## How to read the implementation

1. Start with [`scripts/verify-archived-agent-notes.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-archived-agent-notes.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/security`, `domain/session-state`, `domain/storage`, `lifecycle/implemented`, `mechanism/event-sourcing`
- Aliases: `.agents/notes/archived/{kind}/yyyy-mm-dd-topic.md`, `Archived: YYYY-MM-DD`, `.rgignore`, `verify-archived-agent-notes`, `--write`, `dsh-archive-agent-notes`, `Freeze low-future-value Agent Notes outside the active corpus`, `process`, `boundary`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `recovery`
- Regex: `(?i)(\.agents/notes/archived/\{kind\}/yyyy\-mm\-dd\-topic\.md|Archived:[- ]YYYY\-MM\-DD|\.rgignore|verify\-archived\-agent\-notes|\-\-write|dsh\-archive\-agent\-notes|boundary|discovery[- ]routing)`

```bash
rg -n --pcre2 "(?i)(\\.agents/notes/archived/\\{kind\\}/yyyy\\-mm\\-dd\\-topic\\.md|Archived:[- ]YYYY\\-MM\\-DD|\\.rgignore|verify\\-archived\\-agent\\-notes|\\-\\-write|dsh\\-archive\\-agent\\-notes|boundary|discovery[- ]routing)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0687. Agent Notes](0687-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `scripts/verify-archived-agent-notes.ts`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/verify-public-repository-links.spec.ts`.
- **`shares-code-with`** — [0685. AGENTS.md --- Archived Agent Notes](0685-agents-md-archived-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`shares-code-with`** — [0506. AGENTS.md --- Implemented Agent Notes](0506-agents-md-implemented-agent-notes.md): Shares source implementation: `.agents/skills/dsh-archive-agent-notes/SKILL.md`, `.agents/skills/dsh-find-simplifications/SKILL.md`.
- **`same-design-pressure`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`same-design-pressure`** — [0528. Prune dead public API and result fields](0528-prune-dead-public-api-and-result-fields.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0409-freeze-low-future-value-agent-notes-outside-the-active-corpus.md`.
