---
id: "dsh-note-0309"
title: "Sample over-cap glob results across the tree"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-27-glob-sampling.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/protocols"
  - "domain/security"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "list"
  - "root"
  - "path"
  - "globMaxResults"
  - "sampleOverCapGlobResults"
  - "ctx.fs.listDir"
  - "listDir"
  - "fs-glob-sampling"
  - "Sample over-cap glob results across the tree"
  - "bug fix"
  - "boundary"
  - "discovery routing"
  - "evidence"
  - "recovery"
search_regex: "(?i)(list|root|path|globMaxResults|sampleOverCapGlobResults|ctx\\.fs\\.listDir|listDir|fs\\-glob\\-sampling)"
---

# 0309. Sample over-cap glob results across the tree — implementation context

## Open this when

Asked what a workspace contained, an agent described one subfolder as if it were the whole project. The workspace held 22 top-level entries and 11,485 files. glob {"pattern":""} matched 10,030 paths, but all 100 inline paths sat under one recently unpacked subtree, so the model never saw the other 21 entries. Three individually valid behaviors composed into the false impression. A glob without / matches basenames at any depth, so means every file in the tree rather than the shell's current-directory expansion. Ripgrep's --sort=modified is ascending, so an archive's restored old timestamps put that subtree first.

## Source decision

A result that fits within globMaxResults remains complete and byte-for-byte modification-time ordered. The required sampleOverCapGlobResults config has no fallback: false retains the modification-time head for an over-cap result, while true samples round-robin across the complete result's top-level entries. In sampling mode, every entry receives one slot before any receives a second, exhausted groups drop out, relative order remains stable within each group, and grouping is relative to the actual search root, including an explicit path.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-27-glob-sampling.md](../02-notes/implemented/bug-fix/2026-07-27-glob-sampling.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-27-glob-sampling.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-27-glob-sampling.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`.`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Defines `path`, a construct named by the note. Defines `root`, a construct named by the note. | `symbol-definition` |
| [`packages/hooks/hook-protocol/src/merge.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts) | runtime implementation | Defines `list`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `root` | `let` | [`vendor/hmr/src/index.ts:65`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L65) | `let root = dirname(filename)` |
| `path` | `const` | [`vendor/hmr/src/index.ts:514`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts#L514) | `const path = relative(this.baseDir, fileURLToPath(filename))` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.
- Source verification intent: Package tests pin the required config, both over-cap modes, their prompt and schema descriptions, concentrated and flat results, explicit roots, more groups than the JavaScript argument limit, exhausted groups, fewer slots than groups, and paths outside the workdir. The fs-glob-sampling ACP scenario explicitly enables sampling, boots a minimal real Loader/app/local-bash composition, and executes the real search plugin against a deterministic rg process fixture; its result spans four top-level entries instead of returning one subtree's head.

## How to read the implementation

1. Start with [`.`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/.) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/protocols`, `domain/security`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `list`, `root`, `path`, `globMaxResults`, `sampleOverCapGlobResults`, `ctx.fs.listDir`, `listDir`, `fs-glob-sampling`, `Sample over-cap glob results across the tree`, `bug fix`, `boundary`, `discovery routing`, `evidence`, `recovery`
- Regex: `(?i)(list|root|path|globMaxResults|sampleOverCapGlobResults|ctx\.fs\.listDir|listDir|fs\-glob\-sampling)`

```bash
rg -n --pcre2 "(?i)(list|root|path|globMaxResults|sampleOverCapGlobResults|ctx\\.fs\\.listDir|listDir|fs\\-glob\\-sampling)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): Shares source implementation: `.`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `.`, `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): Shares source implementation: `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0573. TUI banner brand gradient](0573-tui-banner-brand-gradient.md): Shares source implementation: `vendor/hmr/src/index.ts`.
- **`shares-code-with`** — [0044. GUI layering and the RPC protocol --- host/client layering by capability provider, the four-quadrant message model, and the fetch carrier](0044-gui-layering-and-the-rpc-protocol-host-client-layering-by-capability-pro.md): Shares source implementation: `vendor/hmr/src/index.ts`.
- **`same-design-pressure`** — [0564. Bash-backed grep and glob discovery tools](0564-bash-backed-grep-and-glob-discovery-tools.md): Shares design concerns: `concern/boundary`, `concern/discovery-routing`, `concern/evidence`.
- **`shares-code-with`** — [0204. Independent model and user skill invocation policy](0204-independent-model-and-user-skill-invocation-policy.md): Shares source implementation: `packages/hooks/hook-protocol/src/merge.ts`.
- **`shares-code-with`** — [0546. Extract example apps into packages](0546-extract-example-apps-into-packages.md): Shares source implementation: `vendor/hmr/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0309-sample-over-cap-glob-results-across-the-tree.md`.
