---
id: "dsh-note-0592"
title: "Assistant timing line renders after the message body"
status: "archived"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/feature/2026-07-27-assistant-timing-header-trailing.md"
implementation_evidence: "medium"
target_anchor: "Rust provider-adapter boundary"
tags:
  - "class/feature"
  - "concern/evidence"
  - "concern/lifecycle"
  - "domain/llm"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/projection"
aliases:
  - "header"
  - "Assistant · Model wait 0.0s · Completed …"
  - "AssistantMessageComponent"
  - "StreamingAssistantComponent.rebuild"
  - "· Completed …"
  - "tui.spec.ts"
  - "Assistant · Model wait …"
  - "Assistant timing line renders after the message body"
  - "feature"
  - "evidence"
  - "lifecycle"
  - "llm"
  - "storage"
  - "testing"
search_regex: "(?i)(header|Assistant[- ]·[- ]Model[- ]wait[- ]0\\.0s[- ]·[- ]Completed[- ]…|AssistantMessageComponent|StreamingAssistantComponent\\.rebuild|·[- ]Completed[- ]…|tui\\.spec\\.ts|Assistant[- ]·[- ]Model[- ]wait[- ]…|Assistant[- ]timing[- ]line[- ]renders[- ]after[- ]the[- ]message[- ]body)"
---

# 0592. Assistant timing line renders after the message body — implementation context

## Open this when

The TUI assistant message opened with a single header line joining the Assistant label and the step-timing string (Assistant · Model wait 0.0s · Completed …). Placing the timing before the body pushed the durations away from the answer they describe and, once completed, buried the reply's first line under a metadata line the reader scans past.

## Source decision

Split the label from the timing; render the timing as the message's trailing line. AssistantMessageComponent (packages/ui/tui/src/index.ts) now emits the bold Assistant label as the first line and appends the dim timing string (already assembled by StreamingAssistantComponent.rebuild() as header, including the · Completed … suffix when settled) as the last child, after reasoning and text. The timing content, bucket-hiding, and completion-time behavior are unchanged --- only its position moved from the top to the bottom of the message.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/feature/2026-07-27-assistant-timing-header-trailing.md](../02-notes/archived/feature/2026-07-27-assistant-timing-header-trailing.md)
- Pinned source: [.agents/notes/archived/feature/2026-07-27-assistant-timing-header-trailing.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/feature/2026-07-27-assistant-timing-header-trailing.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Defines `header`, a construct named by the note. | `symbol-definition` |
| [`packages/web/tool-web/src/fetch.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts) | runtime implementation | Defines `header`, a construct named by the note. | `symbol-definition` |
| [`packages/lsp/lsp-stdio/src/framing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/framing.ts) | runtime implementation | Defines `header`, a construct named by the note. | `symbol-definition` |
| [`packages/core/agent-loop/src/agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts) | runtime implementation | Defines `header`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `header` | `const` | [`packages/core/agent-loop/src/agent.ts:458`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/src/agent.ts#L458) | `const header = canonicalHeader({` |
| `header` | `const` | [`packages/core/session/src/index.ts:259`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L259) | `const header = record?.['header']` |
| `header` | `const` | [`packages/core/session/src/index.ts:877`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts#L877) | `const header: SessionHeader = {` |
| `header` | `const` | [`packages/lsp/lsp-stdio/src/framing.ts:21`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/lsp/lsp-stdio/src/framing.ts#L21) | `const header = Buffer.from(\`Content-Length: ${body.length}\r\n\r\n\`, 'ascii')` |
| `header` | `const` | [`packages/web/tool-web/src/fetch.ts:311`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/web/tool-web/src/fetch.ts#L311) | `const header = \`Fetched ${result.url} (HTTP ${result.statusCode})\n\n\`` |

### Tests and executable evidence

- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `You`.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `You`.
- [`apps/web/tests/minimal-preset.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/minimal-preset.snapshot.ts) — A test under the owning area exercises or imports `You`.
- [`packages/core/agent-loop/tests/loop.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/loop.spec.ts) — A test under the owning area exercises or imports `You`.
- [`packages/preset/persona/tests/persona.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/preset/persona/tests/persona.spec.ts) — A test under the owning area exercises or imports `You`.
- [`packages/core/agent-loop/tests/request-cache.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/request-cache.e2e.ts) — A test under the owning area exercises or imports `You`.
- [`packages/core/agent-loop/tests/scope-lifecycle.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent-loop/tests/scope-lifecycle.spec.ts) — A test under the owning area exercises or imports `You`.
- [`packages/client/runtime/tests/conversation.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/tests/conversation.client.spec.ts) — A test under the owning area exercises or imports `Assistant`.

## How to read the implementation

1. Start with [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/evidence`, `concern/lifecycle`, `domain/llm`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/projection`
- Aliases: `header`, `Assistant · Model wait 0.0s · Completed …`, `AssistantMessageComponent`, `StreamingAssistantComponent.rebuild`, `· Completed …`, `tui.spec.ts`, `Assistant · Model wait …`, `Assistant timing line renders after the message body`, `feature`, `evidence`, `lifecycle`, `llm`, `storage`, `testing`
- Regex: `(?i)(header|Assistant[- ]·[- ]Model[- ]wait[- ]0\.0s[- ]·[- ]Completed[- ]…|AssistantMessageComponent|StreamingAssistantComponent\.rebuild|·[- ]Completed[- ]…|tui\.spec\.ts|Assistant[- ]·[- ]Model[- ]wait[- ]…|Assistant[- ]timing[- ]line[- ]renders[- ]after[- ]the[- ]message[- ]body)`

```bash
rg -n --pcre2 "(?i)(header|Assistant[- ]\u00b7[- ]Model[- ]wait[- ]0\\.0s[- ]\u00b7[- ]Completed[- ]\u2026|AssistantMessageComponent|StreamingAssistantComponent\\.rebuild|\u00b7[- ]Completed[- ]\u2026|tui\\.spec\\.ts|Assistant[- ]\u00b7[- ]Model[- ]wait[- ]\u2026|Assistant[- ]timing[- ]line[- ]renders[- ]after[- ]the[- ]message[- ]body)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0673. Copyable TUI transcript without gutter bars](0673-copyable-tui-transcript-without-gutter-bars.md): Shares source implementation: `apps/cli/tests/web-agent-presets.e2e.ts`, `apps/web/tests/minimal-preset.snapshot.ts`.
- **`shares-code-with`** — [0535. Drop durable step boundary events](0535-drop-durable-step-boundary-events.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/lsp/lsp-stdio/src/framing.ts`.
- **`shares-code-with`** — [0025. Every LLM request is reconstructable from the session log](0025-every-llm-request-is-reconstructable-from-the-session-log.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0562. The session prefix --- request-only messages in front of the derived history](0562-the-session-prefix-request-only-messages-in-front-of-the-derived-history.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/agent-loop/tests/request-cache.e2e.ts`.
- **`shares-code-with`** — [0607. experimental subcommands gate behind `--experimental` or `DSH_EXPERIMENTAL=1`](0607-experimental-subcommands-gate-behind-experimental-or-dsh-experimental-1.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/web/tool-web/src/fetch.ts`.
- **`shares-code-with`** — [0522. Architectural conformance --- dependency rules and the adapter kit](0522-architectural-conformance-dependency-rules-and-the-adapter-kit.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/session/src/index.ts`.
- **`shares-code-with`** — [0336. Message fork actions require a completed turn tail](0336-message-fork-actions-require-a-completed-turn-tail.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/core/agent-loop/tests/loop.spec.ts`.
- **`shares-code-with`** — [0653. Stop mirroring the token stream as an agent event](0653-stop-mirroring-the-token-stream-as-an-agent-event.md): Shares source implementation: `packages/core/agent-loop/src/agent.ts`, `packages/lsp/lsp-stdio/src/framing.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0592-assistant-timing-line-renders-after-the-message-body.md`.
