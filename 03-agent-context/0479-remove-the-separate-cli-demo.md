---
id: "dsh-note-0479"
title: "Remove the separate CLI demo"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-08-remove-cli-demo.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/simplification"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "dsh --profile headless"
  - "@deepseek-ai/dsh-cli-demo"
  - "pnpm dsh --profile headless"
  - "examples/headless-agent"
  - "@deepseek-ai/dsh-agent-spine-demo"
  - "@deepseek-ai/dsh-loader-smoke"
  - "dsh-cli-demo"
  - "--output-format"
  - "@deepseek-ai/dsh-cli-demo/src/cli.ts"
  - "Remove the separate CLI demo"
  - "simplification"
  - "boundary"
  - "cancellation timeout"
  - "discovery routing"
search_regex: "(?i)(dsh[- ]\\-\\-profile[- ]headless|@deepseek\\-ai/dsh\\-cli\\-demo|pnpm[- ]dsh[- ]\\-\\-profile[- ]headless|examples/headless\\-agent|@deepseek\\-ai/dsh\\-agent\\-spine\\-demo|@deepseek\\-ai/dsh\\-loader\\-smoke|dsh\\-cli\\-demo|\\-\\-output\\-format)"
---

# 0479. Remove the separate CLI demo — implementation context

## Open this when

After dsh --profile headless became the product one-shot command, @deepseek-ai/dsh-cli-demo remained a second application package for the same job. It carried another executable, argument grammar, app composition, cancellation lifecycle, text/JSON/stream-JSON output contract, built artifact, documentation surface, and test suite. The two entry points also assembled different trees, so a successful demo did not prove the shipped headless profile and users had to choose between overlapping commands. The replay suites still need canonical session events to pin assembled backend behavior.

## Source decision

Delete @deepseek-ai/dsh-cli-demo completely: its package, bin, parser, app plugin, output formats, tests, workspace references, generated-catalog entries, and active documentation. No alias or compatibility package remains. Source users invoke the product command through pnpm dsh --profile headless; it owns final-text stdout, failure diagnostics on stderr, persistence, exit status, and shutdown. examples/headless-agent becomes an explicit test composition.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-08-remove-cli-demo.md](../02-notes/implemented/simplification/2026-08-08-remove-cli-demo.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-08-remove-cli-demo.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-08-remove-cli-demo.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/bundle/headless/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/headless`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/test-support/loader-smoke/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/loader-smoke`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/test-support/loader-smoke/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/loader-smoke`. | `named-package-member` |
| [`examples/headless-agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `examples/headless-agent`. | `named-directory-member` |
| [`examples/headless-agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `examples/headless-agent`. | `named-directory-member` |
| [`examples/headless-agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`packages/bundle/headless`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/examples/agent-spine-demo`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/test-support/loader-smoke`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/loader-smoke) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: Focused Loader smokes cover the explicit composition in source and plain-Node built modes, snapshot tests diff its canonical JSONL and persisted logs, product acceptance covers dsh --profile headless, and documentation plus generated graph/catalog gates reject live references to the removed package. The frozen Agent Note archive remains historical evidence and is not rewritten.

## How to read the implementation

1. Start with [`packages/bundle/headless/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/headless/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/simplification`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `dsh --profile headless`, `@deepseek-ai/dsh-cli-demo`, `pnpm dsh --profile headless`, `examples/headless-agent`, `@deepseek-ai/dsh-agent-spine-demo`, `@deepseek-ai/dsh-loader-smoke`, `dsh-cli-demo`, `--output-format`, `@deepseek-ai/dsh-cli-demo/src/cli.ts`, `Remove the separate CLI demo`, `simplification`, `boundary`, `cancellation timeout`, `discovery routing`
- Regex: `(?i)(dsh[- ]\-\-profile[- ]headless|@deepseek\-ai/dsh\-cli\-demo|pnpm[- ]dsh[- ]\-\-profile[- ]headless|examples/headless\-agent|@deepseek\-ai/dsh\-agent\-spine\-demo|@deepseek\-ai/dsh\-loader\-smoke|dsh\-cli\-demo|\-\-output\-format)`

```bash
rg -n --pcre2 "(?i)(dsh[- ]\\-\\-profile[- ]headless|@deepseek\\-ai/dsh\\-cli\\-demo|pnpm[- ]dsh[- ]\\-\\-profile[- ]headless|examples/headless\\-agent|@deepseek\\-ai/dsh\\-agent\\-spine\\-demo|@deepseek\\-ai/dsh\\-loader\\-smoke|dsh\\-cli\\-demo|\\-\\-output\\-format)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0105. Apps own their command line through `ctx.cmdlineArgs`](0105-apps-own-their-command-line-through-ctx-cmdlineargs.md): The source note links to this decision directly.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `packages/bundle/headless/src/index.ts`, `packages/bundle/headless/src/invariant.ts`.
- **`shares-code-with`** — [0210. Persistent Bash and string-replacement editor tools](0210-persistent-bash-and-string-replacement-editor-tools.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0610. `dsh run` owns one-shot headless execution](0610-dsh-run-owns-one-shot-headless-execution.md): Shares source implementation: `packages/bundle/headless`, `packages/bundle/headless/src/index.ts`.
- **`shares-code-with`** — [0669. TUI titles come from the session-title service](0669-tui-titles-come-from-the-session-title-service.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0566. Optional time-context plugin](0566-optional-time-context-plugin.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0193. tmux-location context](0193-tmux-location-context.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0479-remove-the-separate-cli-demo.md`.
