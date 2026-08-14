---
id: "dsh-note-0059"
title: "One harness home resolver"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-24-single-harness-home-resolver.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/ownership"
  - "concern/performance"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/observability"
  - "domain/shell-terminal"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "sessions"
  - "home"
  - "resolveDshHome"
  - "dshHomePath"
  - "dshHomeDisplay"
  - "resolve"
  - "@deepseek-ai/dsh-home"
  - "configured ?? $DSH_HOME ?? ~/.dsh"
  - "@deepseek-ai/dsh-home-paths"
  - "dsh-home"
  - "$DSH_HOME"
  - "dsh-app-boot"
  - "~/.dsh"
  - "AGENTS.md"
search_regex: "(?i)(sessions|home|resolveDshHome|dshHomePath|dshHomeDisplay|resolve|@deepseek\\-ai/dsh\\-home|configured[- ]\\?\\?[- ]\\$DSH_HOME[- ]\\?\\?[- ]\\~/\\.dsh)"
---

# 0059. One harness home resolver — implementation context

## Open this when

The harness had two inconsistent conventions for "where does DeepSeek Harness user data live": @deepseek-ai/dsh-home resolved configured ?? $DSH_HOME ?? ~/.dsh. @deepseek-ai/dsh-home-paths shipped a second resolveDshHome with the same precedence plus tilde expansion --- a near-duplicate of dsh-home that no gate flagged because the two lived in different packages and had already drifted (only one expanded tildes). Two resolvers for the same cross-cutting fact meant there was no single home policy.

## Source decision

One resolver owns the harness home, in @deepseek-ai/dsh-home-paths, single-root: An empty or whitespace-only $DSH_HOME is treated as unset; otherwise resolve('') would silently place the home at the current working directory. The harness keeps all user data under one root; there is no XDG config/data/cache split. dshHomePath(...segments) joins deployment-owned children onto that root, and dsh-app-boot exposes it to Loader !!js config expressions before mounting entries, so shipped compositions derive sessions and storages without copying the resolver.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-24-single-harness-home-resolver.md](../02-notes/implemented/architecture/2026-07-24-single-harness-home-resolver.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-24-single-harness-home-resolver.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-24-single-harness-home-resolver.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. Defines `home`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/shell/tool-bash/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/util/home-paths/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts) | package entry point | Core file in the package named by the note: `packages/util/home-paths`. Defines `resolveDshHome`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/boot/app-boot/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/shell/tool-bash/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/tool-bash`. | `named-package-member` |
| [`packages/util/home-paths/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/util/home-paths`. | `named-package-member` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/index.ts) | package entry point | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/skill/skill-filesystem/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/skill/skill-filesystem`. | `named-package-member` |
| [`packages/examples/agent-spine-demo/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/examples/agent-spine-demo`. | `named-package-member` |
| [`packages/boot/app-boot`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/shell/tool-bash`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash) | package or module directory | The note names this package or capability. | `named-package` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `sessions` | `const` | [`packages/acp/acp/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/acp/acp/src/index.ts#L110) | `const sessions = new Map<SessionId, SessionRecord>()` |
| `home` | `const` | [`packages/boot/app-boot/src/index.ts:181`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L181) | `const home = resolveDshHome()` |
| `resolveDshHome` | `function` | [`packages/util/home-paths/src/index.ts:87`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L87) | `export function resolveDshHome(configured?: string, env: Record<string, string \| undefined> = process.env): string {` |
| `dshHomePath` | `function` | [`packages/util/home-paths/src/index.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L98) | `export function dshHomePath(...segments: string[]): string {` |
| `dshHomeDisplay` | `function` | [`packages/util/home-paths/src/index.ts:110`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/src/index.ts#L110) | `export function dshHomeDisplay(resolvedHome: string): string {` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |

### Tests and executable evidence

- [`packages/shell/tool-bash/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/tools.spec.ts) — A test under the owning area exercises or imports `dsh-home`. Contains the exact code literal `dsh-home` named by the note.
- [`packages/boot/app-boot/tests/profile.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/profile.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/boot/app-boot/tests/app-boot.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/app-boot.spec.ts) — A test under the owning area exercises or imports `dshHomePath`.
- [`packages/util/home-paths/tests/home-paths.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/util/home-paths/tests/home-paths.spec.ts) — A test under the owning area exercises or imports `resolveDshHome`. A test under the owning area exercises or imports `$DSH_HOME`.
- [`packages/boot/app-boot/tests/user-patches.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/tests/user-patches.spec.ts) — A test under the owning area exercises or imports `dsh-app-boot`.
- [`packages/shell/tool-bash/tests/integration.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/tool-bash/tests/integration.spec.ts) — A test under the owning area exercises or imports `dsh-home`. Contains the exact code literal `dsh-home` named by the note.
- [`packages/examples/agent-spine-demo/tests/agent-core.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/examples/agent-spine-demo/tests/agent-core.spec.ts) — A test under the owning area exercises or imports `dsh-home`. A test under the owning area exercises or imports `dsh-agent-spine-demo`.
- [`packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/tests/skill-filesystem.spec.ts) — A test under the owning area exercises or imports `dsh-skill-filesystem`.

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

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/discovery-routing`, `concern/ownership`, `concern/performance`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/observability`, `domain/shell-terminal`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `sessions`, `home`, `resolveDshHome`, `dshHomePath`, `dshHomeDisplay`, `resolve`, `@deepseek-ai/dsh-home`, `configured ?? $DSH_HOME ?? ~/.dsh`, `@deepseek-ai/dsh-home-paths`, `dsh-home`, `$DSH_HOME`, `dsh-app-boot`, `~/.dsh`, `AGENTS.md`
- Regex: `(?i)(sessions|home|resolveDshHome|dshHomePath|dshHomeDisplay|resolve|@deepseek\-ai/dsh\-home|configured[- ]\?\?[- ]\$DSH_HOME[- ]\?\?[- ]\~/\.dsh)`

```bash
rg -n --pcre2 "(?i)(sessions|home|resolveDshHome|dshHomePath|dshHomeDisplay|resolve|@deepseek\\-ai/dsh\\-home|configured[- ]\\?\\?[- ]\\$DSH_HOME[- ]\\?\\?[- ]\\~/\\.dsh)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0490. Remove the SDK project toolchain](0490-remove-the-sdk-project-toolchain.md): The source note links to this decision directly.
- **`shares-code-with`** — [0166. The dsh CLI and personal config overlays from the Harness home](0166-the-dsh-cli-and-personal-config-overlays-from-the-harness-home.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `packages/boot/app-boot/src/index.ts`, `packages/boot/app-boot/src/invariant.ts`.
- **`shares-code-with`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): Shares source implementation: `packages/util/home-paths/src/index.ts`, `packages/util/home-paths/src/invariant.ts`.
- **`shares-code-with`** — [0192. Skill catalog hot refresh](0192-skill-catalog-hot-refresh.md): Shares source implementation: `packages/skill/skill-filesystem/src/index.ts`, `packages/skill/skill-filesystem/src/invariant.ts`.
- **`shares-code-with`** — [0363. Bounded background job admission](0363-bounded-background-job-admission.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.
- **`shares-code-with`** — [0512. Storage root placement and derived-medium recovery](0512-storage-root-placement-and-derived-medium-recovery.md): Shares source implementation: `packages/util/home-paths/src/index.ts`, `packages/util/home-paths/src/invariant.ts`.
- **`shares-code-with`** — [0566. Optional time-context plugin](0566-optional-time-context-plugin.md): Shares source implementation: `packages/examples/agent-spine-demo/src/index.ts`, `packages/examples/agent-spine-demo/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0059-one-harness-home-resolver.md`.
