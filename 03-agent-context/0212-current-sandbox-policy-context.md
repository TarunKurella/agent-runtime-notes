---
id: "dsh-note-0212"
title: "Current sandbox policy context"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-30-current-sandbox-policy-context.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/trust"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "resolve"
  - "read-only"
  - "/permission danger-full-access"
  - "dsh-sandbox-policy"
  - "ctx.sandboxPolicy.resolve"
  - "workspace-write"
  - "danger-full-access"
  - "/dev/null"
  - "dsh-system-prompt"
  - "user/message"
  - "step/start"
  - "request/header"
  - "/permission"
  - "WorldState"
search_regex: "(?i)(resolve|read\\-only|/permission[- ]danger\\-full\\-access|dsh\\-sandbox\\-policy|ctx\\.sandboxPolicy\\.resolve|workspace\\-write|danger\\-full\\-access|/dev/null)"
---

# 0212. Current sandbox policy context — implementation context

## Open this when

The sandbox policy already enforced and logged each session's file-effect mode, but a fresh model request did not contain that state. In a Web session under read-only, write and edit schemas remained visible, so the model claimed it could write and learned otherwise only after a denied call. After /permission danger-full-access, the next request carried the approval-policy change but still omitted the sandbox mode. Denial results were therefore the first model-visible policy source even when the user asked about capability before any operation.

## Source decision

dsh-sandbox-policy, the owner of mode and workspace-root resolution, registers one sandbox:policy cache-safe context contribution. Every agent request resolves the active session directly through ctx.sandboxPolicy.resolve({ session }); there is no denial-history scan or process-local "last told" state. The policy contribution is capability-neutral and available to every agent session by default. A composition may suppress the complete runtime-context channel when its model interface intentionally excludes dynamic context; this does not disable policy enforcement.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-30-current-sandbox-policy-context.md](../02-notes/implemented/feature/2026-07-30-current-sandbox-policy-context.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-30-current-sandbox-policy-context.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-30-current-sandbox-policy-context.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/core/system-prompt/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/sandbox/sandbox-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox-policy`. | `named-package-member` |
| [`packages/sandbox/sandbox-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox-policy`. | `named-package-member` |
| [`packages/core/system-prompt`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/sandbox/sandbox-policy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `resolve`, a construct named by the note. | `symbol-definition` |
| [`packages/core/system-prompt/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/README.md) | package contract and examples | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/core/system-prompt/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/package.json) | composition and configuration | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/sandbox/sandbox-policy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/README.md) | package contract and examples | Core file in the package named by the note: `packages/sandbox/sandbox-policy`. | `named-package-member` |
| [`packages/sandbox/sandbox-policy/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/package.json) | composition and configuration | Core file in the package named by the note: `packages/sandbox/sandbox-policy`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-sandbox-policy` named by the note. Contains the exact code literal `dsh-system-prompt` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |

### Tests and executable evidence

- [`packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write) — The note names this package or capability.
- [`packages/typert/generator/tests/fixtures/type-model/packages/write/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/typert/generator/tests/fixtures/type-model/packages/write/package.json) — Core file in the package named by the note: `packages/typert/generator/tests/fixtures/type-model/packages/write`.
- [`packages/sandbox/sandbox-policy/tests/policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/tests/policy.spec.ts) — A test under the owning area exercises or imports `TMPDIR`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — Contains the exact code literal `request/header` named by the note.
- [`apps/web/tests/shipped-composition.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/shipped-composition.e2e.ts) — Contains the exact code literal `dsh-sandbox-policy` named by the note.

## How to read the implementation

1. Start with [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/trust`, `domain/agent-loop`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/registry`
- Aliases: `resolve`, `read-only`, `/permission danger-full-access`, `dsh-sandbox-policy`, `ctx.sandboxPolicy.resolve`, `workspace-write`, `danger-full-access`, `/dev/null`, `dsh-system-prompt`, `user/message`, `step/start`, `request/header`, `/permission`, `WorldState`
- Regex: `(?i)(resolve|read\-only|/permission[- ]danger\-full\-access|dsh\-sandbox\-policy|ctx\.sandboxPolicy\.resolve|workspace\-write|danger\-full\-access|/dev/null)`

```bash
rg -n --pcre2 "(?i)(resolve|read\\-only|/permission[- ]danger\\-full\\-access|dsh\\-sandbox\\-policy|ctx\\.sandboxPolicy\\.resolve|workspace\\-write|danger\\-full\\-access|/dev/null)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0470. Capability-neutral sandbox policy context](0470-capability-neutral-sandbox-policy-context.md): The source note links to this decision directly.
- **`shares-code-with`** — [0210. Persistent Bash and string-replacement editor tools](0210-persistent-bash-and-string-replacement-editor-tools.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0234. Third-party memory MCP examples](0234-third-party-memory-mcp-examples.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0151. Parallel tool-call execution by per-call safety](0151-parallel-tool-call-execution-by-per-call-safety.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write`, `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.
- **`shares-code-with`** — [0252. Web transcript marks context source, recall, and steering](0252-web-transcript-marks-context-source-recall-and-steering.md): Shares source implementation: `packages/core/system-prompt/src/index.ts`, `packages/core/system-prompt/src/invariant.ts`.
- **`shares-code-with`** — [0561. Plan mode --- a logged per-agent session mode](0561-plan-mode-a-logged-per-agent-session-mode.md): Shares source implementation: `packages/core/system-prompt/README.md`.
- **`shares-code-with`** — [0313. Web agents receive explicit runtime context](0313-web-agents-receive-explicit-runtime-context.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): Shares source implementation: `packages/typert/generator/tests/fixtures/type-model/packages/write/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0212-current-sandbox-policy-context.md`.
