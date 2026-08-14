---
id: "dsh-note-0614"
title: "Thinking rows use one disclosure target"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-23-thinking-row-disclosure-target.md"
implementation_evidence: "medium"
target_anchor: "append-only event and session store"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/session-state"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "ToolRow"
  - "expandOnRowClick"
  - "ThinkRow"
  - "Thinking rows use one disclosure target"
  - "bug fix"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "ownership"
  - "session state"
  - "testing"
  - "ui interaction"
  - "web retrieval"
  - "archived"
search_regex: "(?i)(ToolRow|expandOnRowClick|ThinkRow|Thinking[- ]rows[- ]use[- ]one[- ]disclosure[- ]target|bug[- ]fix|boundary|discovery[- ]routing|evidence)"
---

# 0614. Thinking rows use one disclosure target — implementation context

## Open this when

A collapsed reasoning entry presents Think and its one-line reasoning summary as one visual row, but an icon-only disclosure control leaves both visible labels inert. Applying title expansion to every tool row would instead break the generic tool-row contract, where the row opens details and only the leading control expands arguments.

## Source decision

ToolRow exposes the opt-in expandOnRowClick policy. ThinkRow enables it so the title and reasoning summary form one accessible disclosure target; pointer clicks, Enter, and Space toggle the same component-local expanded state. Tool rows that do not opt in retain row-to-details selection and leading-control argument expansion.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-23-thinking-row-disclosure-target.md](../02-notes/archived/bug-fix/2026-07-23-thinking-row-disclosure-target.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-23-thinking-row-disclosure-target.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-23-thinking-row-disclosure-target.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) | runtime implementation | Defines `ToolRow`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ToolRow` | `function` | [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx:128`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx#L128) | `export function ToolRow({` |

### Tests and executable evidence

- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `Think`.
- [`apps/web/tests/message-actions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/message-actions.e2e.ts) — A test under the owning area exercises or imports `Think`.
- [`apps/web/tests/replay-round-trip.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/replay-round-trip.e2e.ts) — A test under the owning area exercises or imports `Think`.
- [`apps/web/tests/declared-reasoning.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/declared-reasoning.e2e.ts) — A test under the owning area exercises or imports `Think`.
- [`apps/web/tests/snapshots/workflow-run/ui.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/workflow-run/ui.expected.md) — A test under the owning area exercises or imports `Think`.
- [`apps/web/tests/snapshots/steering/settled.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/steering/settled.expected.md) — A test under the owning area exercises or imports `Think`.
- [`apps/web/tests/snapshots/steer-all/settled.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/steer-all/settled.expected.md) — A test under the owning area exercises or imports `Think`.
- [`apps/web/tests/snapshots/skill-tool-row/ui.expected.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/snapshots/skill-tool-row/ui.expected.md) — A test under the owning area exercises or imports `Think`.
- Source verification intent: The component spec pins both Think click targets and the unchanged generic tool-row handoff. The keyless browser fixture loads the real sidebar and conversation bundles, opens an authored reasoning session, clicks the summary and title, and checks the disclosure state and expanded body.

## How to read the implementation

1. Start with [`packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/src/client/tool/components/ToolRow.tsx) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `domain/session-state`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `ToolRow`, `expandOnRowClick`, `ThinkRow`, `Thinking rows use one disclosure target`, `bug fix`, `boundary`, `discovery routing`, `evidence`, `ownership`, `session state`, `testing`, `ui interaction`, `web retrieval`, `archived`
- Regex: `(?i)(ToolRow|expandOnRowClick|ThinkRow|Thinking[- ]rows[- ]use[- ]one[- ]disclosure[- ]target|bug[- ]fix|boundary|discovery[- ]routing|evidence)`

```bash
rg -n --pcre2 "(?i)(ToolRow|expandOnRowClick|ThinkRow|Thinking[- ]rows[- ]use[- ]one[- ]disclosure[- ]target|bug[- ]fix|boundary|discovery[- ]routing|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0598. Address pending queue occurrences for edit and removal](0598-address-pending-queue-occurrences-for-edit-and-removal.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0332. Same-basename Workspace adoption](0332-same-basename-workspace-adoption.md): Shares source implementation: `apps/web/tests/message-actions.e2e.ts`.
- **`shares-code-with`** — [0207. Web terminal card --- the bash render intent reaches the browser](0207-web-terminal-card-the-bash-render-intent-reaches-the-browser.md): Shares source implementation: `packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`.
- **`shares-code-with`** — [0244. Web thinking tail scroll --- collapsed reasoning follows live output](0244-web-thinking-tail-scroll-collapsed-reasoning-follows-live-output.md): Shares source implementation: `packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`.
- **`shares-code-with`** — [0611. Keep the Code Mode result card complete](0611-keep-the-code-mode-result-card-complete.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.
- **`shares-code-with`** — [0237. opening a produced file from the web UI](0237-opening-a-produced-file-from-the-web-ui.md): Shares source implementation: `packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`.
- **`shares-code-with`** — [0606. Web context injection disclosure](0606-web-context-injection-disclosure.md): Shares source implementation: `packages/client/ui-tool/src/client/tool/components/ToolRow.tsx`.
- **`shares-code-with`** — [0296. Status-driven disclosure for workflow runs](0296-status-driven-disclosure-for-workflow-runs.md): Shares source implementation: `apps/web/tests/smoke-real.e2e.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0614-thinking-rows-use-one-disclosure-target.md`.
