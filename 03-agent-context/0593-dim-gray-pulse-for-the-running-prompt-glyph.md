---
id: "dsh-note-0593"
title: "Dim-gray pulse for the running prompt glyph"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-27-tui-running-glyph-smooth-fade.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "domain/llm"
  - "domain/shell-terminal"
  - "domain/ui-interaction"
  - "lifecycle/archived"
aliases:
  - "visible"
  - "STATUS_FADE_MS = 300"
  - "(now - startedAt)/FADE"
  - "1 - (now - endedAt)/FADE"
  - "pulseLevel"
  - "STATUS_PULSE_FLOOR"
  - "STATUS_PULSE_PERIOD_MS = 1400"
  - "fadeGlyph"
  - "STATUS_FADE_MIN_OPACITY"
  - "STATUS_FADE_GRAY.trough"
  - ".settled"
  - "STATUS_ANIMATION_INTERVAL_MS = 50"
  - "beginFadeOut"
  - "FadingStatus"
search_regex: "(?i)(visible|STATUS_FADE_MS[- ]=[- ]300|\\(now[- ]\\-[- ]startedAt\\)/FADE|1[- ]\\-[- ]\\(now[- ]\\-[- ]endedAt\\)/FADE|pulseLevel|STATUS_PULSE_FLOOR|STATUS_PULSE_PERIOD_MS[- ]=[- ]1400|fadeGlyph)"
---

# 0593. Dim-gray pulse for the running prompt glyph — implementation context

## Open this when

While a turn runs, the TUI replaces the > prompt caret with a phase glyph ([circle]/[asterisk]/●/[gear]). Earlier iterations animated its brightness in the accent blue (a discrete SGR wave, then a truecolor throb) --- a colored, always-pulsing indicator. The desired effect keeps the continuous pulse to signal ongoing work, but as a quiet dim gray rather than a color, and with smooth fade-in and fade-out at its edges.

## Source decision

The running glyph is a dim gray that fades in on turn start, throbs continuously while the turn runs, and fades out after it ends before the plain > caret returns. It is never the accent color. Brightness is a fade envelope times a running throb. The envelope gates appear/disappear, linear in the render clock over STATUS_FADE_MS = 300: (now - startedAt)/FADE clamped for fade-in, 1 - (now - endedAt)/FADE for fade-out. pulseLevel is a cosine between STATUS_PULSE_FLOOR (0) and 1 over STATUS_PULSE_PERIOD_MS = 1400, so each breath swells from fully invisible to full and back.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-27-tui-running-glyph-smooth-fade.md](../02-notes/archived/feature/2026-07-27-tui-running-glyph-smooth-fade.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-27-tui-running-glyph-smooth-fade.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-27-tui-running-glyph-smooth-fade.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Defines `visible`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/tool-skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts) | package entry point | Defines `visible`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `visible`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/JsonTree.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx) | runtime implementation | Defines `visible`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `visible` | `const` | [`packages/client/ui-primitives/src/JsonTree.tsx:139`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/JsonTree.tsx#L139) | `const visible = entries.slice(0, limit)` |
| `visible` | `const` | [`packages/core/tools/src/index.ts:1166`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1166) | `const visible = new Map<string, ToolDefinition>()` |
| `visible` | `const` | [`packages/core/tools/src/index.ts:1380`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts#L1380) | `const visible = this.get(name, agent)` |
| `visible` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2052`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2052) | `const visible = await listVisibleSessionSummaries(signal)` |
| `visible` | `const` | [`packages/skill/tool-skill/src/index.ts:362`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/tool-skill/src/index.ts#L362) | `const visible = new Set(agent.session.surface.nodes)` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** Rust provider-adapter boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `domain/llm`, `domain/shell-terminal`, `domain/ui-interaction`, `lifecycle/archived`
- Aliases: `visible`, `STATUS_FADE_MS = 300`, `(now - startedAt)/FADE`, `1 - (now - endedAt)/FADE`, `pulseLevel`, `STATUS_PULSE_FLOOR`, `STATUS_PULSE_PERIOD_MS = 1400`, `fadeGlyph`, `STATUS_FADE_MIN_OPACITY`, `STATUS_FADE_GRAY.trough`, `.settled`, `STATUS_ANIMATION_INTERVAL_MS = 50`, `beginFadeOut`, `FadingStatus`
- Regex: `(?i)(visible|STATUS_FADE_MS[- ]=[- ]300|\(now[- ]\-[- ]startedAt\)/FADE|1[- ]\-[- ]\(now[- ]\-[- ]endedAt\)/FADE|pulseLevel|STATUS_PULSE_FLOOR|STATUS_PULSE_PERIOD_MS[- ]=[- ]1400|fadeGlyph)`

```bash
rg -n --pcre2 "(?i)(visible|STATUS_FADE_MS[- ]=[- ]300|\\(now[- ]\\-[- ]startedAt\\)/FADE|1[- ]\\-[- ]\\(now[- ]\\-[- ]endedAt\\)/FADE|pulseLevel|STATUS_PULSE_FLOOR|STATUS_PULSE_PERIOD_MS[- ]=[- ]1400|fadeGlyph)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0341. The conversation column scrolls on one axis](0341-the-conversation-column-scrolls-on-one-axis.md): Shares source implementation: `packages/client/ui-primitives/src/JsonTree.tsx`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0603. /details command for transcript detail state](0603-details-command-for-transcript-detail-state.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0127. Trajectory assembly from registered Conversation Contexts](0127-trajectory-assembly-from-registered-conversation-contexts.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0085. the end-seed log boundary](0085-the-end-seed-log-boundary.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0591. Code Mode sub-calls in the trajectory and waterfall views](0591-code-mode-sub-calls-in-the-trajectory-and-waterfall-views.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0625. Hero stays visible while a blank session opens](0625-hero-stays-visible-while-a-blank-session-opens.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/skill/tool-skill/src/index.ts`.
- **`shares-code-with`** — [0594. Fixed `Tool / <name>` header for tool-call cards](0594-fixed-tool-name-header-for-tool-call-cards.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0593-dim-gray-pulse-for-the-running-prompt-glyph.md`.
