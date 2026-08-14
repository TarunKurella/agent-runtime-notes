---
id: "dsh-note-0319"
title: "Web details follow the current Session lifecycle"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-29-web-details-session-lifecycle.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "blank"
  - "AppFrame"
  - "localStorage"
  - "Web details follow the current Session lifecycle"
  - "bug fix"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "build release"
  - "filesystem"
  - "session state"
  - "storage"
search_regex: "(?i)(blank|AppFrame|localStorage|Web[- ]details[- ]follow[- ]the[- ]current[- ]Session[- ]lifecycle|bug[- ]fix|boundary|discovery[- ]routing|evidence)"
---

# 0319. Web details follow the current Session lifecycle — implementation context

## Open this when

The details entry is Session-scoped, but its preferred grid width is root-scoped. Selecting a different Session replaced the details content without closing that root preference, so the new owner inherited stale viewing geometry. Hero and other unselected states render no Session-scoped details; they need a derived zero track without becoming false owners in the comparison.

## Source decision

AppFrame reads the current Session id and its blank summary flag from the authoritative Session projection. It records the last non-blank selected id only when that Session can own details, so hero and other unselected states neither trigger closure nor replace the last Session owner; their rendered details track derives as zero without changing the stored preference.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-29-web-details-session-lifecycle.md](../02-notes/implemented/bug-fix/2026-07-29-web-details-session-lifecycle.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-29-web-details-session-lifecycle.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-29-web-details-session-lifecycle.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `blank`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-layout/src/client/AppFrame.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx) | runtime implementation | Defines `AppFrame`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx) | runtime implementation | Defines `blank`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `blank` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L71) | `const blank = useSession(s => s.blank)` |
| `blank` | `const` | [`packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx:147`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-conversation/src/client/skeleton/ConversationSession.tsx#L147) | `const blank = useSession(s => s.blank)` |
| `AppFrame` | `function` | [`packages/client/ui-layout/src/client/AppFrame.tsx:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/src/client/AppFrame.tsx#L87) | `export function AppFrame({` |
| `blank` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:513`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L513) | `const blank = state.blank && event.type !== 'turn/start'` |

### Tests and executable evidence

- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `localStorage`.
- [`apps/web/tests/schedule-after.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/schedule-after.e2e.ts) — A test under the owning area exercises or imports `localStorage`.
- [`apps/web/tests/settings-chrome.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/settings-chrome.e2e.ts) — A test under the owning area exercises or imports `localStorage`.
- [`scripts/vitest-environment.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/vitest-environment.compat.spec.ts) — A test under the owning area exercises or imports `localStorage`.
- [`apps/web/tests/workspace-management.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/workspace-management.e2e.ts) — A test under the owning area exercises or imports `localStorage`.
- [`packages/client/runtime/tests/store.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/store.client.spec.ts) — A test under the owning area exercises or imports `localStorage`.
- [`packages/client/ui-layout/tests/apply.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-layout/tests/apply.client.spec.ts) — A test under the owning area exercises or imports `AppFrame`.
- [`packages/client/ui-tool/tests/web-card.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-tool/tests/web-card.client.spec.tsx) — A test under the owning area exercises or imports `localStorage`.

## How to read the implementation

1. Start with [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) because it has the strongest evidence link to the note.
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
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/build-release`, `domain/filesystem`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `blank`, `AppFrame`, `localStorage`, `Web details follow the current Session lifecycle`, `bug fix`, `boundary`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `build release`, `filesystem`, `session state`, `storage`
- Regex: `(?i)(blank|AppFrame|localStorage|Web[- ]details[- ]follow[- ]the[- ]current[- ]Session[- ]lifecycle|bug[- ]fix|boundary|discovery[- ]routing|evidence)`

```bash
rg -n --pcre2 "(?i)(blank|AppFrame|localStorage|Web[- ]details[- ]follow[- ]the[- ]current[- ]Session[- ]lifecycle|bug[- ]fix|boundary|discovery[- ]routing|evidence)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0624. Web details default closed](0624-web-details-default-closed.md): The source note links to this decision directly.
- **`source-link`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): The source note links to this decision directly.
- **`source-link`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): The source note links to this decision directly.
- **`shares-code-with`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): Shares source implementation: `apps/web/tests/schedule-after.e2e.ts`, `apps/web/tests/workspace-management.e2e.ts`.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0267. Resolved theme color metadata](0267-resolved-theme-color-metadata.md): Shares source implementation: `apps/web/tests/settings-chrome.e2e.ts`, `packages/client/ui-layout/tests/apply.client.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0319-web-details-follow-the-current-session-lifecycle.md`.
