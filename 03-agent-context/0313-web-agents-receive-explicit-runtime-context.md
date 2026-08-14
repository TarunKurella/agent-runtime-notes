---
id: "dsh-note-0313"
title: "Web agents receive explicit runtime context"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-28-web-agent-runtime-context.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/simplification"
  - "domain/agent-loop"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/streaming"
aliases:
  - "dsh-base"
  - "dsh-web-app"
  - "web-runtime"
  - "app:web-surface"
  - "surfaceContext"
  - "surfaceContext: false"
  - "request/header"
  - "dsh-system-prompt"
  - "Web agents receive explicit runtime context"
  - "bug fix"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "simplification"
search_regex: "(?i)(dsh\\-base|dsh\\-web\\-app|web\\-runtime|app:web\\-surface|surfaceContext|surfaceContext:[- ]false|request/header|dsh\\-system\\-prompt)"
---

# 0313. Web agents receive explicit runtime context — implementation context

## Open this when

The shared CLI base configured an empty deployment persona, the Web overlay did not replace it, and the Web launcher added no source or interaction-surface section. A session header recorded its working directory for tools and persistence, but the model prompt did not state that directory or identify the DeepSeek Harness Web GUI. A request such as "change this page's theme" therefore made the agent search the selected project for an unspecified page, even when the user meant the GUI running the session.

## Source decision

The Web profile composes the dsh-base and dsh-web-app bundles. The Web bundle supplies a concise coding-agent persona containing the resolved {{model}} and session {{cwd}}; its web-runtime plugin adds the app:web-surface section when surfaceContext is true. Before mounting the profile tree, the dsh web alias reads that same composed setting and installs the existing harness:source section only when the surface context is enabled.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-28-web-agent-runtime-context.md](../02-notes/implemented/bug-fix/2026-07-28-web-agent-runtime-context.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-28-web-agent-runtime-context.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-28-web-agent-runtime-context.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/web-app/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts) | package entry point | Core file in the package named by the note: `packages/bundle/web-app`. | `named-package-member` |
| [`packages/bundle/base/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/web-app/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/bundle/web-app`. | `named-package-member` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/core/system-prompt/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/bundle/base`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/bundle/web-app`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/system-prompt`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/bundle/base/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/README.md) | package contract and examples | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/base/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/package.json) | composition and configuration | Core file in the package named by the note: `packages/bundle/base`. | `named-package-member` |
| [`packages/bundle/web-app/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/README.md) | package contract and examples | Core file in the package named by the note: `packages/bundle/web-app`. | `named-package-member` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/bundle/base/tests/base.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/tests/base.spec.ts) — A test under the owning area exercises or imports `dsh-base`.
- [`packages/bundle/web-app/tests/web-app.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/tests/web-app.spec.ts) — A test under the owning area exercises or imports `web-runtime`. A test under the owning area exercises or imports `surfaceContext`.
- [`apps/web/tests/live-interactions.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/live-interactions.e2e.ts) — Contains the exact code literal `request/header` named by the note.
- Source verification intent: The Web runtime unit tests pin both enabled and disabled surfaceContext behavior, while the Web alias unit test pins default-on and explicit-off source-section gating from the composed row. The keyless fresh-round-trip Web scenario boots the shipped base plus Web bundle, runs a real session through the HTTP/SSE application, and snapshots the system-prompt prefix with source and working-directory paths normalized. The snapshot pins the harness identity, source checkout, Web orientation, and resolved coding-agent persona in request order.

## How to read the implementation

1. Start with [`packages/bundle/base/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/simplification`, `domain/agent-loop`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/streaming`
- Aliases: `dsh-base`, `dsh-web-app`, `web-runtime`, `app:web-surface`, `surfaceContext`, `surfaceContext: false`, `request/header`, `dsh-system-prompt`, `Web agents receive explicit runtime context`, `bug fix`, `boundary`, `discovery routing`, `evidence`, `simplification`
- Regex: `(?i)(dsh\-base|dsh\-web\-app|web\-runtime|app:web\-surface|surfaceContext|surfaceContext:[- ]false|request/header|dsh\-system\-prompt)`

```bash
rg -n --pcre2 "(?i)(dsh\\-base|dsh\\-web\\-app|web\\-runtime|app:web\\-surface|surfaceContext|surfaceContext:[- ]false|request/header|dsh\\-system\\-prompt)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0325. Source checkout paths do not define working directories](0325-source-checkout-paths-do-not-define-working-directories.md): The source note links to this decision directly.
- **`shares-code-with`** — [0115. headless is a direct core entry point](0115-headless-is-a-direct-core-entry-point.md): Shares source implementation: `packages/bundle/base`, `packages/bundle/base/src/index.ts`.
- **`shares-code-with`** — [0101. Profile plugin bundles replace the fixed surface overlays](0101-profile-plugin-bundles-replace-the-fixed-surface-overlays.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0212. Current sandbox policy context](0212-current-sandbox-policy-context.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0357. Child agents join their parent's preset composition](0357-child-agents-join-their-parent-s-preset-composition.md): Shares source implementation: `packages/bundle/base/src/index.ts`, `packages/bundle/base/src/invariant.ts`.
- **`shares-code-with`** — [0210. Persistent Bash and string-replacement editor tools](0210-persistent-bash-and-string-replacement-editor-tools.md): Shares source implementation: `packages/core/system-prompt`, `packages/core/system-prompt/src/index.ts`.
- **`shares-code-with`** — [0491. Production dsh excludes product subagent providers](0491-production-dsh-excludes-product-subagent-providers.md): Shares source implementation: `packages/bundle/base`, `packages/bundle/base/README.md`.
- **`shares-code-with`** — [0274. inline-code file mentions open the file they name](0274-inline-code-file-mentions-open-the-file-they-name.md): Shares source implementation: `packages/bundle/web-app`, `packages/bundle/web-app/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0313-web-agents-receive-explicit-runtime-context.md`.
