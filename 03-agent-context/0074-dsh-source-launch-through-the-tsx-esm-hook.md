---
id: "dsh-note-0074"
title: "dsh source launch through the tsx ESM hook"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-29-dsh-source-launch-tsx-esm.md"
implementation_evidence: "high"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/performance"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/generation"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "dsh"
  - "paths"
  - "constructor"
  - "apps/cli/src/bin.ts"
  - "node --experimental-transform-types"
  - "--experimental-transform-types"
  - "@Inject"
  - "vendor/hmr"
  - "vendor/"
  - "packages/workflow"
  - "^22.19.0 || >=24.0.0"
  - "module.register"
  - "makeSyncRequest"
  - "--import tsx"
search_regex: "(?i)(paths|constructor|apps/cli/src/bin\\.ts|node[- ]\\-\\-experimental\\-transform\\-types|\\-\\-experimental\\-transform\\-types|@Inject|vendor/hmr|vendor/)"
---

# 0074. dsh source launch through the tsx ESM hook — implementation context

## Open this when

Startup latency also mattered: the off-thread module.register() hooks worker serialized every resolution across threads (~440ms of makeSyncRequest wait during TUI boot), and the full tsx default (--import tsx) pays ~0.4s in its CJS hook's resolution amplification.

## Source decision

The dsh TUI, Web, and headless source launches run node --import tsx/esm: tsx's ESM-only hook owns both TypeScript transformation and tsconfig paths projection. The root dsh script uses that vector directly from the repository root; artifact generation is a separate operation under the source-launch/build separation decision. The CJS hook stays off because the CLI source graph is ESM-only; measured runtime launch to the TUI banner is ~0.7s versus ~1.1s under the full tsx default and ~0.75s under the removed native chain. scripts/tspath-loader.ts and apps/cli/src/tsconfig-paths-loader.ts are deleted.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-29-dsh-source-launch-tsx-esm.md](../02-notes/implemented/architecture/2026-07-29-dsh-source-launch-tsx-esm.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-29-dsh-source-launch-tsx-esm.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-29-dsh-source-launch-tsx-esm.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/src/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/bin.ts) | runtime implementation | The source note names this file directly. | `named-file` |
| [`packages/llm/llm/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/llm/llm/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm`. | `named-package-member` |
| [`packages/jobs/tool-jobs/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/index.ts) | package entry point | Core file in the package named by the note: `packages/jobs/tool-jobs`. | `named-package-member` |
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/plan/plan-mode/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/jobs/tool-jobs/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/jobs/tool-jobs`. | `named-package-member` |
| [`packages/plan/plan-mode/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`vendor/hmr/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `vendor/hmr`. | `named-directory-member` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `vendor/hmr`. | `named-directory-member` |
| [`vendor/hmr/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `vendor/hmr`. Contains the exact code literal `vendor/hmr` named by the note. | `exact-code-occurrence, named-directory-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `paths` | `const` | [`packages/shell/tool-bash/src/render.ts:82`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/render.ts#L82) | `const paths = [read.stdoutSpillPath, read.stderrSpillPath].filter((path): path is string => path !== undefined)` |
| `constructor` | `let` | [`vendor/cordis/src/service.ts:106`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/service.ts#L106) | `let constructor = instance.constructor` |

### Tests and executable evidence

- [`apps/cli/tests/source-launch.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/source-launch.compat.spec.ts) — The source note names this file directly. Contains the exact code literal `apps/cli/src/bin.ts` named by the note.
- [`packages/plan/plan-mode/tests/plan-mode.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/plan-mode.spec.ts) — A test under the owning area exercises or imports `dsh-plan-mode`.
- [`packages/plan/plan-mode/tests/invariant.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/invariant.spec.ts) — A test under the owning area exercises or imports `dsh-plan-mode`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `dsh-tool-jobs`.
- [`packages/plan/plan-mode/tests/projection.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/projection.spec.ts) — A test under the owning area exercises or imports `dsh-plan-mode`.
- [`packages/plan/plan-mode/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/tests/integration.spec.ts) — A test under the owning area exercises or imports `dsh-plan-mode`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — Contains the exact code literal `apps/cli/src/bin.ts` named by the note.
- [`examples/headless-agent/tests/headless.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/headless-agent/tests/headless.snapshot.ts) — Contains the exact code literal `apps/cli/src/bin.ts` named by the note.

## How to read the implementation

1. Start with [`apps/cli/src/bin.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/bin.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/architecture`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/performance`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/generation`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `dsh`, `paths`, `constructor`, `apps/cli/src/bin.ts`, `node --experimental-transform-types`, `--experimental-transform-types`, `@Inject`, `vendor/hmr`, `vendor/`, `packages/workflow`, `^22.19.0 || >=24.0.0`, `module.register`, `makeSyncRequest`, `--import tsx`
- Regex: `(?i)(paths|constructor|apps/cli/src/bin\.ts|node[- ]\-\-experimental\-transform\-types|\-\-experimental\-transform\-types|@Inject|vendor/hmr|vendor/)`

```bash
rg -n --pcre2 "(?i)(paths|constructor|apps/cli/src/bin\\.ts|node[- ]\\-\\-experimental\\-transform\\-types|\\-\\-experimental\\-transform\\-types|@Inject|vendor/hmr|vendor/)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0556. Native TypeScript source launch for dsh](0556-native-typescript-source-launch-for-dsh.md): The source note links to this decision directly.
- **`source-link`** — [0492. Separate source launch from repository build](0492-separate-source-launch-from-repository-build.md): The source note links to this decision directly.
- **`shares-code-with`** — [0256. Producer-declared context forms](0256-producer-declared-context-forms.md): Shares source implementation: `packages/jobs/tool-jobs/src/index.ts`, `packages/jobs/tool-jobs/src/invariant.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0458. Plan-specific collaboration state](0458-plan-specific-collaboration-state.md): Shares source implementation: `packages/plan/plan-mode/src/index.ts`, `packages/plan/plan-mode/src/invariant.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0037. Provider-routed LLM adapters and a generic pi-ai backend](0037-provider-routed-llm-adapters-and-a-generic-pi-ai-backend.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/llm/llm/src/index.ts`, `packages/llm/llm/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0074-dsh-source-launch-through-the-tsx-esm-hook.md`.
