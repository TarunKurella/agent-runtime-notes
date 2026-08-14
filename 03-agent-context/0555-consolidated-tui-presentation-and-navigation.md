---
id: "dsh-note-0555"
title: "Consolidated TUI presentation and navigation"
status: "archived"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/architecture/2026-07-28-consolidated-tui-presentation.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "concern/trust"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "added"
  - "removed"
  - "SessionId"
  - "success"
  - "output"
  - "preview"
  - "/resume"
  - "paletteSpec"
  - "createPalette"
  - "/palette"
  - "Tool / <name>"
  - "renderUnknownXml"
  - "TerminalResultView.output"
  - "ContextCardComponent"
search_regex: "(?i)(added|removed|SessionId|success|output|preview|/resume|paletteSpec)"
---

# 0555. Consolidated TUI presentation and navigation — implementation context

## Open this when

The terminal UI accumulated independent presentation rules that interacted poorly: palette roles aliased one another or inverted emphasis on light terminals; tool-card framing, output, and exit markers repeated or competed; injected context was parsed as XML and could not fold reliably; and /resume excluded sessions outside the current workspace even when the launcher could reach them. Each symptom appeared local, but the durable decision is one terminal-reading model: a small inspectable palette, status-led cards with recessed bodies, content-independent transcript folding, and workspace-aware navigation.

## Source decision

paletteSpec(scheme) is the single table of SGR codes, close codes, and purposes. createPalette derives every wrapper from it and /palette prints the same table in the running terminal. Components do not emit their own SGR sequences except for the fixed startup brand gradient. Every close resets every SGR group its open sets. Duplicate roles are merged: muted into dim, added into success, removed into error, and the unused second accent is removed.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/architecture/2026-07-28-consolidated-tui-presentation.md](../02-notes/archived/architecture/2026-07-28-consolidated-tui-presentation.md)
- Pinned source: [.agents/notes/archived/architecture/2026-07-28-consolidated-tui-presentation.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/architecture/2026-07-28-consolidated-tui-presentation.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. Defines `output`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-bash`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/agent/src/inbox.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts) | runtime implementation | Defines `removed`, a construct named by the note. | `symbol-definition` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Defines `SessionId`, a construct named by the note. | `symbol-definition` |
| [`packages/spill/spill-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts) | package entry point | Defines `preview`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/DiffBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx) | runtime implementation | Defines `added`, a construct named by the note. | `symbol-definition` |
| [`packages/feedback/message-feedback/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/src/index.ts) | package entry point | Defines `success`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/tool-bash/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/README.md) | package contract and examples | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/shell/tool-bash/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/package.json) | composition and configuration | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-tool-bash` named by the note. | `exact-code-occurrence` |
| [`docs/glossary.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/glossary.md) | package contract and examples | Contains the exact code literal `dsh-tool-bash` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `added` | `let` | [`packages/client/ui-primitives/src/DiffBlock.tsx:79`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L79) | `let added = 0` |
| `removed` | `const` | [`packages/core/agent/src/inbox.ts:187`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/agent/src/inbox.ts#L187) | `const removed = inbox.splice(actualStart, actualDeleteCount, ...event.data.inserted)` |
| `SessionId` | `type` | [`packages/core/session/src/types.ts:22`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts#L22) | `export type SessionId = Branded<'SessionId'>` |
| `success` | `function` | [`packages/feedback/message-feedback/src/index.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/feedback/message-feedback/src/index.ts#L91) | `function success<T>(value: T): MessageFeedbackSuccess<T> {` |
| `output` | `const` | [`packages/shell/tool-bash/src/index.ts:160`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts#L160) | `const output = (stream: ShellRunResult['stdout']) => ({` |
| `preview` | `function` | [`packages/spill/spill-policy/src/index.ts:95`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/spill/spill-policy/src/index.ts#L95) | `function preview(text: string, budget: number): { text: string; omitted: Omitted } {` |

### Tests and executable evidence

- [`scripts/verify-cordis-config.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.spec.ts) — Contains the exact code literal `dsh-tool-bash` named by the note.
- Source verification intent: TUI unit and keyless terminal snapshots cover palette enumeration, light/dark roles, legal and illegal style composition, uniformly dim card bodies, semantic diff colors, marker-free terminal output with one exit pill, prose-preserving context frames, content-independent folding, the three-state Ctrl+O cycle, model filtering, and both resume scopes. CLI handoff tests cover passing the re-read cwd and rejecting directory-entry failure before teardown. Tool-bash tests pin result-marker emission, parse, and stripping as one round trip.

## How to read the implementation

1. Start with [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/concurrency`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `concern/trust`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `added`, `removed`, `SessionId`, `success`, `output`, `preview`, `/resume`, `paletteSpec`, `createPalette`, `/palette`, `Tool / <name>`, `renderUnknownXml`, `TerminalResultView.output`, `ContextCardComponent`
- Regex: `(?i)(added|removed|SessionId|success|output|preview|/resume|paletteSpec)`

```bash
rg -n --pcre2 "(?i)(added|removed|SessionId|success|output|preview|/resume|paletteSpec)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0599. TUI hidden mode folds a turn's assistant steps into one message](0599-tui-hidden-mode-folds-a-turn-s-assistant-steps-into-one-message.md): The source note links to this decision directly.
- **`shares-code-with`** — [0517. Pre-tool input rewrite --- a consistent design](0517-pre-tool-input-rewrite-a-consistent-design.md): Shares source implementation: `packages/shell/tool-bash`, `packages/shell/tool-bash/README.md`.
- **`shares-code-with`** — [0545. Every session event is enclosed in a turn](0545-every-session-event-is-enclosed-in-a-turn.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/shell/tool-bash`.
- **`shares-code-with`** — [0242. pwsh tool bash parity](0242-pwsh-tool-bash-parity.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.
- **`shares-code-with`** — [0265. Settlement delivery belongs to the continuation manager](0265-settlement-delivery-belongs-to-the-continuation-manager.md): Shares source implementation: `packages/core/agent/src/inbox.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0260. pwsh UI presentation matches bash](0260-pwsh-ui-presentation-matches-bash.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.
- **`shares-code-with`** — [0290. Background job completion wakes an idle owner](0290-background-job-completion-wakes-an-idle-owner.md): Shares source implementation: `packages/shell/tool-bash/src/index.ts`, `packages/shell/tool-bash/src/invariant.ts`.
- **`shares-code-with`** — [0014. Branded IDs everywhere they belong](0014-branded-ids-everywhere-they-belong.md): Shares source implementation: `packages/core/session/src/types.ts`, `packages/shell/tool-bash/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0555-consolidated-tui-presentation-and-navigation.md`.
