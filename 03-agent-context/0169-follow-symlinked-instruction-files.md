---
id: "dsh-note-0169"
title: "Follow symlinked instruction files"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-21-follow-instruction-symlinks.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/security"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "nodeStatFile"
  - "fsStatFile"
  - "stat"
  - "resolve"
  - "ctx.fs.lstat"
  - "$DSH_HOME/AGENTS.md"
  - "AGENTS.md"
  - "CLAUDE.md → AGENTS.md"
  - "tools/post-execute"
  - "CLAUDE.md"
  - "ctx.fs"
  - "dsh-fs-observation-policy"
  - "$DSH_HOME"
  - "Follow symlinked instruction files"
search_regex: "(?i)(nodeStatFile|fsStatFile|stat|resolve|ctx\\.fs\\.lstat|\\$DSH_HOME/AGENTS\\.md|AGENTS\\.md|CLAUDE\\.md[- ]→[- ]AGENTS\\.md)"
---

# 0169. Follow symlinked instruction files — implementation context

## Open this when

The agent-instructions plugin probed each instruction candidate with ctx.fs.lstat before resolving, rejecting any final-component symlink so a repository-owned link could not point instruction loading at content outside the workspace. That no-follow invariant blocked a deliberate, supported setup: a user who symlinks $DSH_HOME/AGENTS.md --- or a project AGENTS.md --- to a canonical instruction file kept elsewhere, sharing one house-style file across tools and homes, saw the link silently ignored.

## Source decision

Instruction discovery no longer inspects the final component with lstat. Every candidate --- the user-global $DSH_HOME/AGENTS.md, each base candidate, and each local-overlay candidate --- is resolved and its resolved target is stat-ed, at baseline composition and at each tools/post-execute reconciliation alike. A symlink whose target is a regular file loads that target's content; a resolved non-file target (including a link to a directory) is a confirmed absence that removes the scope like a missing file; a resolve or stat exception is classified as temporarily unavailable and never removes an already-loaded.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-21-follow-instruction-symlinks.md](../02-notes/implemented/feature/2026-07-21-follow-instruction-symlinks.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-21-follow-instruction-symlinks.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-21-follow-instruction-symlinks.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs-observation-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/cordis/src/registry.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts) | runtime implementation | Defines `resolve`, a construct named by the note. | `symbol-definition` |
| [`packages/shell/pwsh-local/src/resolve.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts) | runtime implementation | Defines `stat`, a construct named by the note. | `symbol-definition` |
| [`packages/context/agent-instructions/src/files.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts) | runtime implementation | Defines `nodeStatFile`, a construct named by the note. Defines `fsStatFile`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/fs-observation-policy/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/README.md) | package contract and examples | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`packages/fs/fs-observation-policy/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/package.json) | composition and configuration | Core file in the package named by the note: `packages/fs/fs-observation-policy`. | `named-package-member` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `dsh-fs-observation-policy` named by the note. | `exact-code-occurrence` |
| [`docs/architecture.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/architecture.md) | package contract and examples | Contains the exact code literal `tools/post-execute` named by the note. | `exact-code-occurrence` |
| [`docs/tool-catalog.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/tool-catalog.md) | package contract and examples | Contains the exact code literal `dsh-fs-observation-policy` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `nodeStatFile` | `function` | [`packages/context/agent-instructions/src/files.ts:98`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L98) | `async function nodeStatFile(path: string, signal?: AbortSignal): Promise<StatFileProbe> {` |
| `fsStatFile` | `function` | [`packages/context/agent-instructions/src/files.ts:113`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/agent-instructions/src/files.ts#L113) | `async function fsStatFile(` |
| `stat` | `const` | [`packages/shell/pwsh-local/src/resolve.ts:48`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-local/src/resolve.ts#L48) | `const stat = lstatSync(candidate)` |
| `resolve` | `function` | [`vendor/cordis/src/registry.ts:71`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/registry.ts#L71) | `export function resolve(inject: Inject \| null \| undefined, result: Dict = Object.create(null)) {` |

### Tests and executable evidence

- [`packages/fs/fs-observation-policy/tests/policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/tests/policy.spec.ts) — A test under the owning area exercises or imports `dsh-fs-observation-policy`.

## How to read the implementation

1. Start with [`packages/fs/fs-observation-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/context-evidence`** — Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/security`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `nodeStatFile`, `fsStatFile`, `stat`, `resolve`, `ctx.fs.lstat`, `$DSH_HOME/AGENTS.md`, `AGENTS.md`, `CLAUDE.md → AGENTS.md`, `tools/post-execute`, `CLAUDE.md`, `ctx.fs`, `dsh-fs-observation-policy`, `$DSH_HOME`, `Follow symlinked instruction files`
- Regex: `(?i)(nodeStatFile|fsStatFile|stat|resolve|ctx\.fs\.lstat|\$DSH_HOME/AGENTS\.md|AGENTS\.md|CLAUDE\.md[- ]→[- ]AGENTS\.md)`

```bash
rg -n --pcre2 "(?i)(nodeStatFile|fsStatFile|stat|resolve|ctx\\.fs\\.lstat|\\$DSH_HOME/AGENTS\\.md|AGENTS\\.md|CLAUDE\\.md[- ]\u2192[- ]AGENTS\\.md)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0136. Workspace context instruction files](0136-workspace-context-instruction-files.md): The source note links to this decision directly.
- **`source-link`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): The source note links to this decision directly.
- **`source-link`** — [0170. Load all instruction candidates with per-directory dedup](0170-load-all-instruction-candidates-with-per-directory-dedup.md): The source note links to this decision directly.
- **`shares-code-with`** — [0356. Filesystem absence is an observation and guarded creation never replaces](0356-filesystem-absence-is-an-observation-and-guarded-creation-never-replaces.md): Shares source implementation: `packages/fs/fs-observation-policy/src/index.ts`, `packages/fs/fs-observation-policy/src/invariant.ts`.
- **`shares-code-with`** — [0246. Guarded-mutation errors append the recovery instruction at the model boundary](0246-guarded-mutation-errors-append-the-recovery-instruction-at-the-model-bou.md): Shares source implementation: `packages/fs/fs-observation-policy/src/index.ts`, `packages/fs/fs-observation-policy/src/types.ts`.
- **`shares-code-with`** — [0010. Filesystem capability seam --- ctx.fs, local backend, and model-facing filesystem tools](0010-filesystem-capability-seam-ctx-fs-local-backend-and-model-facing-filesys.md): Shares source implementation: `vendor/cordis/src/registry.ts`.
- **`shares-code-with`** — [0212. Current sandbox policy context](0212-current-sandbox-policy-context.md): Shares source implementation: `vendor/cordis/src/registry.ts`.
- **`shares-code-with`** — [0171. Default local instruction overlay](0171-default-local-instruction-overlay.md): Shares source implementation: `packages/context/agent-instructions/src/files.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0169-follow-symlinked-instruction-files.md`.
