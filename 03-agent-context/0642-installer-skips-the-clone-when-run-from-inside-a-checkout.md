---
id: "dsh-note-0642"
title: "installer skips the clone when run from inside a checkout"
status: "archived"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/process/2026-07-22-installer-in-repo-skip-clone.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/ownership"
  - "domain/filesystem"
  - "domain/shell-terminal"
  - "lifecycle/archived"
aliases:
  - "cwd"
  - "scripts/install.sh"
  - "curl ... | sh"
  - "~/.dsh/source"
  - "sh scripts/install.sh"
  - "scripts/"
  - "bin/dsh"
  - "IN_REPO=1"
  - "DSH_SOURCE"
  - "git checkout -B"
  - "DSH_REF"
  - "git rev-parse --show-toplevel"
  - "installer skips the clone when run from inside a checkout"
  - "process"
search_regex: "(?i)(scripts/install\\.sh|curl[- ]\\.\\.\\.[- ]\\|[- ]sh|\\~/\\.dsh/source|sh[- ]scripts/install\\.sh|scripts/|bin/dsh|IN_REPO=1|DSH_SOURCE)"
---

# 0642. installer skips the clone when run from inside a checkout — implementation context

## Open this when

scripts/install.sh is written for the curl ... | sh path: it clones the harness into ~/.dsh/source, then installs, links, and launches. Contributors who already have a checkout and run the same script directly (sh scripts/install.sh) got a second, unrelated clone at ~/.dsh/source --- installing and linking a different tree than the one they were working in, with no way to exercise the local script against the local source.

## Source decision

The script detects when it is executing from inside a real checkout and, in that mode, reuses that checkout and skips the clone/update step entirely, leaving the working tree untouched. Detection keys on $0: under curl ... | sh the script text arrives on stdin, so $0 is the shell name and no file path resolves; running a checked-out copy makes $0 the script file. When $0 is a readable file whose parent is a scripts/ directory inside a tree that carries both the bin/dsh launcher and scripts/install.sh, the script sets IN_REPO=1 and repoints DSH_SOURCE at that repo root.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/process/2026-07-22-installer-in-repo-skip-clone.md](../02-notes/archived/process/2026-07-22-installer-in-repo-skip-clone.md)
- Pinned source: [.agents/notes/archived/process/2026-07-22-installer-in-repo-skip-clone.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/process/2026-07-22-installer-in-repo-skip-clone.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |
| [`packages/fs/tool-fs-search/src/search-core.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts) | runtime implementation | Defines `cwd`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `cwd` | `const` | [`packages/fs/tool-fs-search/src/search-core.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs-search/src/search-core.ts#L223) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:24`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L24) | `const cwd = exec.agent?.session.header.cwd` |
| `cwd` | `const` | [`packages/fs/tool-fs/src/session-cwd.ts:41`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts#L41) | `const cwd = policyWorkspaceRoot ?? sessionCwd(exec, requestedPath)` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2180`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2180) | `const cwd = workspace?.path ?? request.payload.cwd ?? defaults.cwd` |
| `cwd` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3224`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3224) | `const cwd = session.header.cwd` |

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`packages/fs/tool-fs/src/session-cwd.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/session-cwd.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/ownership`, `domain/filesystem`, `domain/shell-terminal`, `lifecycle/archived`
- Aliases: `cwd`, `scripts/install.sh`, `curl ... | sh`, `~/.dsh/source`, `sh scripts/install.sh`, `scripts/`, `bin/dsh`, `IN_REPO=1`, `DSH_SOURCE`, `git checkout -B`, `DSH_REF`, `git rev-parse --show-toplevel`, `installer skips the clone when run from inside a checkout`, `process`
- Regex: `(?i)(scripts/install\.sh|curl[- ]\.\.\.[- ]\|[- ]sh|\~/\.dsh/source|sh[- ]scripts/install\.sh|scripts/|bin/dsh|IN_REPO=1|DSH_SOURCE)`

```bash
rg -n --pcre2 "(?i)(scripts/install\\.sh|curl[- ]\\.\\.\\.[- ]\\|[- ]sh|\\~/\\.dsh/source|sh[- ]scripts/install\\.sh|scripts/|bin/dsh|IN_REPO=1|DSH_SOURCE)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0622. Web conversation UI polish sweep](0622-web-conversation-ui-polish-sweep.md): Shares source implementation: `packages/fs/tool-fs-search/src/search-core.ts`, `packages/fs/tool-fs/src/session-cwd.ts`.
- **`shares-code-with`** — [0619. Tool-card single-row fields render inline](0619-tool-card-single-row-fields-render-inline.md): Shares source implementation: `packages/fs/tool-fs-search/src/search-core.ts`, `packages/fs/tool-fs/src/session-cwd.ts`.
- **`shares-code-with`** — [0345. Workspace New Session reuse hijacked cwd-matching unaccounted blank sessions](0345-workspace-new-session-reuse-hijacked-cwd-matching-unaccounted-blank-sess.md): Shares source implementation: `packages/fs/tool-fs-search/src/search-core.ts`, `packages/fs/tool-fs/src/session-cwd.ts`.
- **`shares-code-with`** — [0130. Multiplex concurrent ACP sessions over one connection](0130-multiplex-concurrent-acp-sessions-over-one-connection.md): Shares source implementation: `packages/fs/tool-fs/src/session-cwd.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0185. Workspace UI Complete Product Flow](0185-workspace-ui-complete-product-flow.md): Shares source implementation: `packages/fs/tool-fs/src/session-cwd.ts`, `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0325. Source checkout paths do not define working directories](0325-source-checkout-paths-do-not-define-working-directories.md): Shares source implementation: `packages/fs/tool-fs/src/session-cwd.ts`.
- **`shares-code-with`** — [0472. One route to add a Workspace](0472-one-route-to-add-a-workspace.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0642-installer-skips-the-clone-when-run-from-inside-a-checkout.md`.
