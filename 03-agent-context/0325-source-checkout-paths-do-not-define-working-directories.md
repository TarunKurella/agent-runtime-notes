---
id: "dsh-note-0325"
title: "Source checkout paths do not define working directories"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-30-source-checkout-workdir-distinction.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
aliases:
  - "cwd"
  - "-99"
  - "dsh-app-boot"
  - "source-checkout-workdir"
  - "/opt/dsh-source"
  - "Source checkout paths do not define working directories"
  - "bug fix"
  - "boundary"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "simplification"
  - "filesystem"
  - "llm"
search_regex: "(?i)(dsh\\-app\\-boot|source\\-checkout\\-workdir|/opt/dsh\\-source|Source[- ]checkout[- ]paths[- ]do[- ]not[- ]define[- ]working[- ]directories|bug[- ]fix|boundary|evidence|lifecycle)"
---

# 0325. Source checkout paths do not define working directories — implementation context

## Open this when

The harness:source prompt section follows the source-location decision, but its original wording called the checkout "your own source code" without distinguishing that path from the session workspace. In a normal TUI configuration that does not state {{cwd}} in its persona, this may be the only fixed absolute path near the start of the system prompt. DeepSeek V4 could therefore answer "what's the workdir?" with the harness checkout instead of determining the session's current working directory. A blanket statement that the checkout is not the working directory would also be false.

## Source decision

The section identifies the path as the "DeepSeek Harness implementation checkout." It says that the checkout location and current working directory are separate values that may differ, forbids inferring the working directory from the checkout path, directs the model to use pwd, and limits the checkout's purpose to inspecting or extending DSH itself. The path derivation, global harness:source ownership, and -99 ordering remain unchanged. Describing the values as conceptually separate rather than always unequal keeps the instruction accurate in both ordinary project sessions and dsh meta.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-30-source-checkout-workdir-distinction.md](../02-notes/implemented/bug-fix/2026-07-30-source-checkout-workdir-distinction.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-30-source-checkout-workdir-distinction.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-30-source-checkout-workdir-distinction.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/boot/app-boot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/README.md) | package contract and examples | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/boot/app-boot/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/package.json) | composition and configuration | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`examples/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/package.json) | composition and configuration | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.md) | package contract and examples | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |
| [`docs/config-catalog.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/config-catalog.zh.md) | package contract and examples | Contains the exact code literal `dsh-app-boot` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |

### Tests and executable evidence

- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `pwd`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — Contains the exact code literal `dsh-app-boot` named by the note.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — Contains the exact code literal `dsh-app-boot` named by the note.
- Source verification intent: The dsh-app-boot unit test pins the exact text and its ordering. The CLI keyless PTY smoke inspects the assembled request header. The TUI source-checkout-workdir snapshot mounts the section with /opt/dsh-source, asks "what's the workdir?" through a recorded DeepSeek V4 turn, and requires the replayed transcript to run pwd and report the generated workspace rather than the checkout.

## How to read the implementation

1. Start with [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `domain/filesystem`, `domain/llm`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `domain/ui-interaction`, `lifecycle/implemented`
- Aliases: `cwd`, `-99`, `dsh-app-boot`, `source-checkout-workdir`, `/opt/dsh-source`, `Source checkout paths do not define working directories`, `bug fix`, `boundary`, `evidence`, `lifecycle`, `ownership`, `simplification`, `filesystem`, `llm`
- Regex: `(?i)(dsh\-app\-boot|source\-checkout\-workdir|/opt/dsh\-source|Source[- ]checkout[- ]paths[- ]do[- ]not[- ]define[- ]working[- ]directories|bug[- ]fix|boundary|evidence|lifecycle)`

```bash
rg -n --pcre2 "(?i)(dsh\\-app\\-boot|source\\-checkout\\-workdir|/opt/dsh\\-source|Source[- ]checkout[- ]paths[- ]do[- ]not[- ]define[- ]working[- ]directories|bug[- ]fix|boundary|evidence|lifecycle)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0570. dsh tells the agent where its own source lives](0570-dsh-tells-the-agent-where-its-own-source-lives.md): The source note links to this decision directly.
- **`shares-code-with`** — [0660. Share the app bins' boot glue instead of maintaining twin copies](0660-share-the-app-bins-boot-glue-instead-of-maintaining-twin-copies.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0602. dsh --dump-config prints the composed config tree](0602-dsh-dump-config-prints-the-composed-config-tree.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0096. Splitting the credential store from the user environment layer](0096-splitting-the-credential-store-from-the-user-environment-layer.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/README.md`.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): Shares source implementation: `packages/boot/app-boot`, `packages/boot/app-boot/src/index.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0145. Explicit model-facing tool order](0145-explicit-model-facing-tool-order.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0325-source-checkout-paths-do-not-define-working-directories.md`.
