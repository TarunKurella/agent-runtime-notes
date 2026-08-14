---
id: "dsh-note-0681"
title: "Pin request-header content in one snapshot scenario"
status: "archived"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/testing/2026-07-06-pin-request-header-content-in-one-scenario.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "scrubSystemPrompts"
  - "scrubToolSchemas"
  - "scrubRequestHeaders"
  - "system"
  - "jsonl"
  - "request/header"
  - "session.jsonl"
  - "pinsHeader"
  - "system-prompt.expected.md"
  - "tool-schemas.expected.json"
  - "header.system"
  - "header.tools"
  - "dsh-acp-snapshot"
  - "session*.jsonl"
search_regex: "(?i)(scrubSystemPrompts|scrubToolSchemas|scrubRequestHeaders|system|jsonl|request/header|session\\.jsonl|pinsHeader)"
---

# 0681. Pin request-header content in one snapshot scenario — implementation context

## Open this when

An ACP snapshot suite needs to prove the exact composed system prompt and tool-schema list sent in each request/header, but duplicating that content inside every session.jsonl makes a prompt or schema edit rewrite dozens of giant one-line JSON records. Keeping one raw header avoids the duplication but still makes prompt review poor: prose is JSON-escaped onto one line and mixed with thousands of characters of tool schemas.

## Source decision

Exactly one scenario per header-composition class is flagged pinsHeader. Its directory splits the pin by review format: system-prompt.expected.md contains the normalized full prompt sequence as ordinary Markdown, tool-schemas.expected.json contains the corresponding complete schema sequence as structured JSON, and session.jsonl retains config, reason, and any model-visible prefix while storing header.system and header.tools as "{{system}}" / "{{tools}}". Every other JSONL uses the same prompt and tool tokens and also tokenizes session-prefix content.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/testing/2026-07-06-pin-request-header-content-in-one-scenario.md](../02-notes/archived/testing/2026-07-06-pin-request-header-content-in-one-scenario.md)
- Pinned source: [.agents/notes/archived/testing/2026-07-06-pin-request-header-content-in-one-scenario.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/testing/2026-07-06-pin-request-header-content-in-one-scenario.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/suite.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. Defines `system`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/test-support/acp-snapshot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/acp-snapshot`. | `named-package-member` |
| [`packages/test-support/acp-snapshot/src/normalize.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts) | runtime implementation | Core file in the package named by the note: `packages/test-support/acp-snapshot`. Defines `scrubSystemPrompts`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/tools`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/acp-snapshot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot) | package or module directory | The note names this package or capability. | `named-package` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `jsonl`, a construct named by the note. | `symbol-definition` |
| [`packages/core/tools/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |
| [`packages/core/tools/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/tools`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `scrubSystemPrompts` | `function` | [`packages/test-support/acp-snapshot/src/normalize.ts:343`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts#L343) | `export function scrubSystemPrompts(rawLog: string): string {` |
| `scrubToolSchemas` | `function` | [`packages/test-support/acp-snapshot/src/normalize.ts:357`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts#L357) | `export function scrubToolSchemas(rawLog: string): string {` |
| `scrubRequestHeaders` | `function` | [`packages/test-support/acp-snapshot/src/normalize.ts:372`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/normalize.ts#L372) | `export function scrubRequestHeaders(rawLog: string): string {` |
| `system` | `const` | [`packages/test-support/acp-snapshot/src/suite.ts:409`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/src/suite.ts#L409) | `const system = (header as { system?: unknown }).system` |
| `jsonl` | `const` | [`scripts/gen-doc-graphs.ts:731`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L731) | `const jsonl = nodeId('bundle', 'jsonl')` |

### Tests and executable evidence

- [`packages/test-support/acp-snapshot/tests/suite.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/tests/suite.spec.ts) — A test under the owning area exercises or imports `pinsHeader`.
- [`packages/test-support/acp-snapshot/tests/normalize.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/tests/normalize.spec.ts) — A test under the owning area exercises or imports `scrubSystemPrompts`. A test under the owning area exercises or imports `scrubToolSchemas`.
- [`packages/test-support/acp-snapshot/tests/fixtures/fake-acp-agent.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/acp-snapshot/tests/fixtures/fake-acp-agent.ts) — A test under the owning area exercises or imports `dsh-acp-snapshot`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-acp-snapshot` named by the note.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — Contains the exact code literal `request/header` named by the note.
- Source verification intent: The suite replays every scenario against the split pins. Unit coverage exercises the independent and full scrubbers, both full-header sidecar formats, record/refresh regeneration, normalized prompt/schema extraction, fixed-point enforcement, required-file symmetry, reconstructed-header uniformity, and changed-header count rejection.

## How to read the implementation

1. Start with [`packages/core/tools/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/tools/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/evidence`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/llm`, `domain/multi-agent`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `scrubSystemPrompts`, `scrubToolSchemas`, `scrubRequestHeaders`, `system`, `jsonl`, `request/header`, `session.jsonl`, `pinsHeader`, `system-prompt.expected.md`, `tool-schemas.expected.json`, `header.system`, `header.tools`, `dsh-acp-snapshot`, `session*.jsonl`
- Regex: `(?i)(scrubSystemPrompts|scrubToolSchemas|scrubRequestHeaders|system|jsonl|request/header|session\.jsonl|pinsHeader)`

```bash
rg -n --pcre2 "(?i)(scrubSystemPrompts|scrubToolSchemas|scrubRequestHeaders|system|jsonl|request/header|session\\.jsonl|pinsHeader)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0165. Typed tool returns in Code Mode](0165-typed-tool-returns-in-code-mode.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0137. The `todo_write` tool --- model task list as event-sourced session state](0137-the-todo-write-tool-model-task-list-as-event-sourced-session-state.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0682. Extract the ACP snapshot suite into a support package](0682-extract-the-acp-snapshot-suite-into-a-support-package.md): Shares source implementation: `packages/test-support/acp-snapshot/src/index.ts`, `packages/test-support/acp-snapshot/src/invariant.ts`.
- **`shares-code-with`** — [0525. Periodic human-review maintenance for dsh-code-review](0525-periodic-human-review-maintenance-for-dsh-code-review.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0187. Code Mode UI foundation --- run_code description and native-parity dispatch logging](0187-code-mode-ui-foundation-run-code-description-and-native-parity-dispatch.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0032. Tool output spill policy](0032-tool-output-spill-policy.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.
- **`shares-code-with`** — [0051. Unified JSON-value schema DSL](0051-unified-json-value-schema-dsl.md): Shares source implementation: `packages/core/tools`, `packages/core/tools/src/index.ts`.
- **`shares-code-with`** — [0228. Code Mode language dispatch and the Python SDK renderer](0228-code-mode-language-dispatch-and-the-python-sdk-renderer.md): Shares source implementation: `packages/core/tools/src/index.ts`, `packages/core/tools/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0681-pin-request-header-content-in-one-snapshot-scenario.md`.
