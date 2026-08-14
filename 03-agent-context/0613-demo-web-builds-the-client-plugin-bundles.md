---
id: "dsh-note-0613"
title: "demo:web builds the client plugin bundles"
status: "archived"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/bug-fix/2026-07-23-demo-web-builds-client-bundles.md"
implementation_evidence: "medium"
target_anchor: "exec, terminal, and process lifecycle"
tags:
  - "class/bug-fix"
  - "concern/evidence"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/archived"
aliases:
  - "build"
  - "GET /plugins/<id>/client.js"
  - "exports[\"./client\"]"
  - "lib/client.js"
  - "tsc -b"
  - "tsdown.client.ts"
  - "~/.dsh/source"
  - "/plugins/<id>/client.js"
  - "http://127.0.0.1:3080"
  - "lib/types"
  - "tsc -b && tsdown"
  - "demo:web builds the client plugin bundles"
  - "bug fix"
  - "evidence"
search_regex: "(?i)(build|GET[- ]/plugins/<id>/client\\.js|exports\\[\"\\./client\"\\]|lib/client\\.js|tsc[- ]\\-b|tsdown\\.client\\.ts|\\~/\\.dsh/source|/plugins/<id>/client\\.js)"
---

# 0613. demo:web builds the client plugin bundles — implementation context

## Open this when

dsh web serves each web-client plugin's bundle from GET /plugins//client.js, resolving the path from the package's exports["./client"] (lib/client.js). Those bundles are produced only by the root pnpm run build (tsc -b then the per-package tsdown.client.ts configs); the Vite build:web step builds the frontend shell alone. demo:web and the README's Web UI instructions ran only build:web, so on a checkout without a prior full build every plugin bundle 404s, the client loader marks every plugin failed, and the boot screen shows "Failed to load plugins".

## Source decision

demo:web runs npm run build before npm run build:web, so the plugin lib/client.js bundles exist before dsh web serves them. The README's Web UI section runs pnpm run build && pnpm run build:web for the installed ~/.dsh/source checkout, which the installer never builds.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/bug-fix/2026-07-23-demo-web-builds-client-bundles.md](../02-notes/archived/bug-fix/2026-07-23-demo-web-builds-client-bundles.md)
- Pinned source: [.agents/notes/archived/bug-fix/2026-07-23-demo-web-builds-client-bundles.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/bug-fix/2026-07-23-demo-web-builds-client-bundles.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/client/ui-slots/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts) | package entry point | Defines `build`, a construct named by the note. | `symbol-definition` |
| [`CLAUDE.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/CLAUDE.md) | package contract and examples | Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence` |
| [`AGENTS.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/AGENTS.md) | package contract and examples | Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence` |
| [`scripts/clean.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.ts) | repository automation | Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence` |
| [`scripts/dev-web.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.ts) | repository automation | Contains the exact code literal `lib/client.js` named by the note. | `exact-code-occurrence` |
| [`docs/api-gateway.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/api-gateway.md) | package contract and examples | Contains the exact code literal `lib/client.js` named by the note. | `exact-code-occurrence` |
| [`docs/api-gateway.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/api-gateway.zh.md) | package contract and examples | Contains the exact code literal `lib/client.js` named by the note. | `exact-code-occurrence` |
| [`scripts/doc-typecheck-paths.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/doc-typecheck-paths.ts) | repository automation | Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence` |
| [`packages/client/tsdown.client.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/tsdown.client.ts) | runtime implementation | Contains the exact code literal `lib/client.js` named by the note. | `exact-code-occurrence` |
| [`scripts/verify-node-next-types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-node-next-types.ts) | repository automation | Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence` |
| [`scripts/check-workspace-constraints.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/check-workspace-constraints.ts) | repository automation | Contains the exact code literal `lib/client.js` named by the note. Contains the exact code literal `lib/types` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `build` | `const` | [`packages/client/ui-slots/src/index.ts:980`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts#L980) | `const build = (name: string, seen: Set<string>): LiveSlotNode \| undefined => {` |

### Tests and executable evidence

- [`scripts/clean.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/clean.spec.ts) — Contains the exact code literal `lib/types` named by the note.
- [`scripts/dev-web.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/dev-web.spec.ts) — Contains the exact code literal `lib/client.js` named by the note.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — Contains the exact code literal `lib/client.js` named by the note.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — Contains the exact code literal `lib/client.js` named by the note.
- [`scripts/doc-typecheck-paths.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/doc-typecheck-paths.spec.ts) — Contains the exact code literal `lib/types` named by the note.
- Source verification intent: After the full build, all eight /plugins//client.js endpoints return 200 and a headless Chromium load of http://127.0.0.1:3080 renders the shell with no "Failed to load plugins" state.

## How to read the implementation

1. Start with [`packages/client/ui-slots/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-slots/src/index.ts) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** exec, terminal, and process lifecycle.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/bug-fix`, `concern/evidence`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/shell-terminal`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/archived`
- Aliases: `build`, `GET /plugins/<id>/client.js`, `exports["./client"]`, `lib/client.js`, `tsc -b`, `tsdown.client.ts`, `~/.dsh/source`, `/plugins/<id>/client.js`, `http://127.0.0.1:3080`, `lib/types`, `tsc -b && tsdown`, `demo:web builds the client plugin bundles`, `bug fix`, `evidence`
- Regex: `(?i)(build|GET[- ]/plugins/<id>/client\.js|exports\["\./client"\]|lib/client\.js|tsc[- ]\-b|tsdown\.client\.ts|\~/\.dsh/source|/plugins/<id>/client\.js)`

```bash
rg -n --pcre2 "(?i)(build|GET[- ]/plugins/<id>/client\\.js|exports\\[\"\\./client\"\\]|lib/client\\.js|tsc[- ]\\-b|tsdown\\.client\\.ts|\\~/\\.dsh/source|/plugins/<id>/client\\.js)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0380. TSC-first build and one compiler ownership](0380-tsc-first-build-and-one-compiler-ownership.md): Shares source implementation: `packages/client/ui-slots/src/index.ts`, `scripts/clean.ts`.
- **`shares-code-with`** — [0523. Supply chain checks and vendor drift verification](0523-supply-chain-checks-and-vendor-drift-verification.md): Shares source implementation: `AGENTS.md`, `CLAUDE.md`.
- **`shares-code-with`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): Shares source implementation: `AGENTS.md`, `CLAUDE.md`.
- **`shares-code-with`** — [0054. Client plugin loading --- plain packages, dsh.client plugins, and the two-phase boot](0054-client-plugin-loading-plain-packages-dsh-client-plugins-and-the-two-phas.md): Shares source implementation: `scripts/dev-web.ts`.
- **`shares-code-with`** — [0378. Vendor Cordis as source, not npm dependencies](0378-vendor-cordis-as-source-not-npm-dependencies.md): Shares source implementation: `AGENTS.md`, `CLAUDE.md`.
- **`shares-code-with`** — [0221. Read card --- the read tool's structured line window reaches the client](0221-read-card-the-read-tool-s-structured-line-window-reaches-the-client.md): Shares source implementation: `AGENTS.md`.
- **`shares-code-with`** — [0080. Full client copy rollout onto the typed locale seat, and the non-translation boundary](0080-full-client-copy-rollout-onto-the-typed-locale-seat-and-the-non-translat.md): Shares source implementation: `packages/client/ui-slots/src/index.ts`.
- **`shares-code-with`** — [0520. Task Surface for structured session interaction](0520-task-surface-for-structured-session-interaction.md): Shares source implementation: `packages/client/ui-slots/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0613-demo-web-builds-the-client-plugin-bundles.md`.
