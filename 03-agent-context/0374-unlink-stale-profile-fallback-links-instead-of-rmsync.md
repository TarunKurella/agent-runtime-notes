---
id: "dsh-note-0374"
title: "Unlink stale profile fallback links instead of rmSync"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-08-12-unlink-stale-profile-fallback-links.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "ensureSymlink"
  - "healProfilesModuleFallback"
  - "unlink"
  - "$DSH_HOME/profiles/node_modules"
  - "rmSync"
  - "ERR_FS_EISDIR"
  - "unlinkSync"
  - "rmdirSync"
  - "ENOENT"
  - "Unlink stale profile fallback links instead of rmSync"
  - "bug fix"
  - "boundary"
  - "concurrency"
  - "evidence"
search_regex: "(?i)(ensureSymlink|healProfilesModuleFallback|unlink|\\$DSH_HOME/profiles/node_modules|rmSync|ERR_FS_EISDIR|unlinkSync|rmdirSync)"
---

# 0374. Unlink stale profile fallback links instead of rmSync — implementation context

## Open this when

healProfilesModuleFallback re-points $DSH_HOME/profiles/node_modules entries when an installation moves, and Windows hosts keep those entries as junctions. ensureSymlink deleted a stale entry with rmSync(link), but Node treats a junction as a directory for removal: without recursive, rmSync throws ERR_FS_EISDIR, so every launch from a moved installation or a second worktree crashed before booting. The replaces a wrong symlink unit test reproduces that crash on Windows at the exact removal call.

## Source decision

ensureSymlink removes a stale link with unlinkSync(link). unlink deletes the reparse point or symlink itself on every platform and never descends into the target, which preserves the function's fail-loud guarantee that a real directory is never deleted. The profile-plugin-bundles decision keeps owning the fallback's two-anchor resolution; this note owns only the removal primitive.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-08-12-unlink-stale-profile-fallback-links.md](../02-notes/implemented/bug-fix/2026-08-12-unlink-stale-profile-fallback-links.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-08-12-unlink-stale-profile-fallback-links.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-08-12-unlink-stale-profile-fallback-links.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`scripts/cordis-core-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts) | repository automation | Defines `unlink`, a construct named by the note. | `symbol-definition` |
| [`packages/boot/app-boot/src/profile.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts) | runtime implementation | Defines `healProfilesModuleFallback`, a construct named by the note. Defines `ensureSymlink`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `ensureSymlink` | `function` | [`packages/boot/app-boot/src/profile.ts:171`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts#L171) | `function ensureSymlink(link: string, target: string): void {` |
| `healProfilesModuleFallback` | `function` | [`packages/boot/app-boot/src/profile.ts:223`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/profile.ts#L223) | `export function healProfilesModuleFallback(installAnchor: string, home: string = resolveDshHome()): void {` |
| `unlink` | `function` | [`scripts/cordis-core-api.ts:363`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts#L363) | `function unlink(text: string): string {` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — A test under the owning area exercises or imports `rmSync`. A test under the owning area exercises or imports `recursive`.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — A test under the owning area exercises or imports `recursive`.
- [`scripts/publint-all.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/publint-all.spec.ts) — A test under the owning area exercises or imports `rmSync`. A test under the owning area exercises or imports `recursive`.
- [`scripts/change-scope.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/change-scope.spec.ts) — A test under the owning area exercises or imports `rmSync`. A test under the owning area exercises or imports `recursive`.
- [`scripts/gen-doc-graphs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.spec.ts) — A test under the owning area exercises or imports `rmSync`.
- [`scripts/cordis-core-api.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.spec.ts) — A test under the owning area exercises or imports `rmSync`. A test under the owning area exercises or imports `recursive`.
- [`scripts/verify-md-links.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.spec.ts) — A test under the owning area exercises or imports `rmSync`. A test under the owning area exercises or imports `recursive`.
- [`scripts/test-fixture-cleanup.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/test-fixture-cleanup.ts) — A test under the owning area exercises or imports `rmSync`. A test under the owning area exercises or imports `recursive`.

## How to read the implementation

1. Start with [`scripts/cordis-core-api.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-core-api.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/boundary`, `concern/concurrency`, `concern/evidence`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/testing`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `ensureSymlink`, `healProfilesModuleFallback`, `unlink`, `$DSH_HOME/profiles/node_modules`, `rmSync`, `ERR_FS_EISDIR`, `unlinkSync`, `rmdirSync`, `ENOENT`, `Unlink stale profile fallback links instead of rmSync`, `bug fix`, `boundary`, `concurrency`, `evidence`
- Regex: `(?i)(ensureSymlink|healProfilesModuleFallback|unlink|\$DSH_HOME/profiles/node_modules|rmSync|ERR_FS_EISDIR|unlinkSync|rmdirSync)`

```bash
rg -n --pcre2 "(?i)(ensureSymlink|healProfilesModuleFallback|unlink|\\$DSH_HOME/profiles/node_modules|rmSync|ERR_FS_EISDIR|unlinkSync|rmdirSync)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0101. Profile plugin bundles replace the fixed surface overlays](0101-profile-plugin-bundles-replace-the-fixed-surface-overlays.md): The source note links to this decision directly.
- **`shares-code-with`** — [0373. Unlink fixture junctions before recursive deletion](0373-unlink-fixture-junctions-before-recursive-deletion.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/clean.spec.ts`.
- **`shares-code-with`** — [0415. Make Lefthook installation worktree-local](0415-make-lefthook-installation-worktree-local.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/verify-md-links.spec.ts`.
- **`shares-code-with`** — [0646. Wine-run Windows blocking gates on Linux runners](0646-wine-run-windows-blocking-gates-on-linux-runners.md): Shares source implementation: `scripts/clean.spec.ts`, `scripts/dev-web.spec.ts`.
- **`shares-code-with`** — [0320. The approval takeover shares the composer's text cap](0320-the-approval-takeover-shares-the-composer-s-text-cap.md): Shares source implementation: `scripts/gen-doc-graphs.spec.ts`, `scripts/publint-all.spec.ts`.
- **`shares-code-with`** — [0372. Resolve Microsoft Store pwsh aliases](0372-resolve-microsoft-store-pwsh-aliases.md): Shares source implementation: `scripts/change-scope.spec.ts`, `scripts/clean.spec.ts`.
- **`shares-code-with`** — [0627. TUI long-session render costs --- shared step-timing scan and card line caches](0627-tui-long-session-render-costs-shared-step-timing-scan-and-card-line-cach.md): Shares source implementation: `scripts/cordis-core-api.ts`, `scripts/verify-md-links.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0374-unlink-stale-profile-fallback-links-instead-of-rmsync.md`.
