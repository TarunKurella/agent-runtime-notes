---
id: "dsh-note-0314"
title: "Web GUI changes close the loop on the existing URL"
status: "implemented"
class: "bug-fix"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/bug-fix/2026-07-28-web-gui-feedback-loop.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/bug-fix"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - "DSH_WEB_URL"
  - "html"
  - "dev"
  - "apps/web/package.json"
  - "window.__DSH_BOOT__"
  - "__DSH_BOOT__"
  - "web-runtime"
  - "app:web-surface"
  - "surfaceContext"
  - "apps/web"
  - "index.html"
  - "no-cache"
  - "$DSH_WEB_URL"
  - "Server.listen"
search_regex: "(?i)(DSH_WEB_URL|html|apps/web/package\\.json|window\\.__DSH_BOOT__|__DSH_BOOT__|web\\-runtime|app:web\\-surface|surfaceContext)"
---

# 0314. Web GUI changes close the loop on the existing URL — implementation context

## Open this when

The Web agent could identify neither the GUI hosting its session nor the URL the user was viewing. The runtime-context decision supplies the first fact, but a GUI edit still had no executable acceptance target: source edits, artifact builds, a listening process, and the user's existing page were unrelated observations. Repository affordances made a wrong substitute look valid because apps/web/package.json exposed vite as its dev script and bare Vite returned HTTP 200 even though it could not inject window.DSH_BOOT.

## Source decision

The ordinary dsh web composition mounts the Web bundle's web-runtime plugin, which publishes one canonical loopback URL as both model-visible orientation and a managed shell fact. The app:web-surface prompt section says that unqualified references identify this GUI and names the URL; DSH_WEB_URL carries the same fact into every foreground or managed background bash call. The section preserves the no-implicit-DOM, route, or screenshot boundary and does not claim that a LAN alias equals the browser's literal address.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/bug-fix/2026-07-28-web-gui-feedback-loop.md](../02-notes/implemented/bug-fix/2026-07-28-web-gui-feedback-loop.md)
- Pinned source: [.agents/notes/implemented/bug-fix/2026-07-28-web-gui-feedback-loop.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/bug-fix/2026-07-28-web-gui-feedback-loop.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/web`. The source note names this file directly. | `named-directory-member, named-file` |
| [`docs/postmortem/0003-web-agent-gui-feedback-loop.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0003-web-agent-gui-feedback-loop.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `apps/web/package.json` named by the note. | `exact-code-occurrence, named-file` |
| [`apps/web`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`scripts/rescope-vendor.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts) | repository automation | Defines `dev`, a construct named by the note. | `symbol-definition` |
| [`scripts/verify-md-links.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts) | repository automation | Defines `html`, a construct named by the note. | `symbol-definition` |
| [`packages/bundle/web-app/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts) | package entry point | Defines `DSH_WEB_URL`, a construct named by the note. | `symbol-definition` |
| [`scripts/check-workspace-constraints.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/check-workspace-constraints.ts) | repository automation | Defines `dev`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Defines `html`, a construct named by the note. | `symbol-definition` |
| [`knip.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/knip.json) | composition and configuration | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |
| [`docs/development.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/development.md) | package contract and examples | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |
| [`tsconfig.client.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.client.json) | composition and configuration | Contains the exact code literal `apps/web` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `DSH_WEB_URL` | `const` | [`packages/bundle/web-app/src/index.ts:67`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/web-app/src/index.ts#L67) | `const DSH_WEB_URL = 'DSH_WEB_URL' as const` |
| `html` | `const` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L32) | `const html = useMemo(() => highlightToHtml(trimmed, lang), [trimmed, lang, loaded])` |
| `dev` | `const` | [`scripts/check-workspace-constraints.ts:302`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/check-workspace-constraints.ts#L302) | `const dev = manifest.devDependencies?.['@deepseek-ai/cordis']` |
| `dev` | `const` | [`scripts/rescope-vendor.ts:155`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts#L155) | `const dev = manifest.devDependencies?.cordis` |
| `dev` | `const` | [`scripts/rescope-vendor.ts:162`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/rescope-vendor.ts#L162) | `const dev = manifest.devDependencies?.['@deepseek-ai/cordis']` |
| `html` | `const` | [`scripts/verify-md-links.ts:131`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-md-links.ts#L131) | `const html = node.value.replace(/<!--[\s\S]*?-->/g, '')` |

### Tests and executable evidence

- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — A test under the owning area exercises or imports `vite`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `web-runtime`. A test under the owning area exercises or imports `surfaceContext`.
- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `dev`.
- [`apps/web/tests/hmr-live.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/hmr-live.e2e.ts) — A test under the owning area exercises or imports `dev`.
- [`apps/web/tests/vite-entry.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/vite-entry.e2e.ts) — A test under the owning area exercises or imports `vite`. A test under the owning area exercises or imports `dev`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `__DSH_BOOT__`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `web-runtime`.
- [`apps/web/tests/replay-round-trip.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/replay-round-trip.e2e.ts) — A test under the owning area exercises or imports `$DSH_WEB_URL`.
- Source verification intent: The keyless fresh-round-trip browser scenario boots the shipped Web composition, drives a real replayed session, snapshots the URL-bearing system-prompt prefix, and invokes the assembled bash tool to prove $DSH_WEB_URL matches the actual bound runtime. The real CLI smoke launches dsh web and captures the provider request, pinning the complete two-command development contract. The dev:web watcher test rebuilds an isolated client bundle after a source change; the browser HMR scenario launches dsh web, changes an initial roster bundle, and observes the new DOM under the same page identity.

## How to read the implementation

1. Start with [`apps/web/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/package.json) because it has the strongest evidence link to the note.
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

- Tags: `class/bug-fix`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `DSH_WEB_URL`, `html`, `dev`, `apps/web/package.json`, `window.__DSH_BOOT__`, `__DSH_BOOT__`, `web-runtime`, `app:web-surface`, `surfaceContext`, `apps/web`, `index.html`, `no-cache`, `$DSH_WEB_URL`, `Server.listen`
- Regex: `(?i)(DSH_WEB_URL|html|apps/web/package\.json|window\.__DSH_BOOT__|__DSH_BOOT__|web\-runtime|app:web\-surface|surfaceContext)`

```bash
rg -n --pcre2 "(?i)(DSH_WEB_URL|html|apps/web/package\\.json|window\\.__DSH_BOOT__|__DSH_BOOT__|web\\-runtime|app:web\\-surface|surfaceContext)" source-deepseek-harness
rg -l --fixed-strings "class/bug-fix" 03-agent-context
```

## Connected notes

- **`source-link`** — [0313. Web agents receive explicit runtime context](0313-web-agents-receive-explicit-runtime-context.md): The source note links to this decision directly.
- **`shares-code-with`** — [0108. Web shell dist chunk split and directory layout](0108-web-shell-dist-chunk-split-and-directory-layout.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `apps/web/package.json`, `apps/web/tests/scaffold.ts`.
- **`shares-code-with`** — [0183. Session List Browsing and Manual Workspace Order](0183-session-list-browsing-and-manual-workspace-order.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0274. inline-code file mentions open the file they name](0274-inline-code-file-mentions-open-the-file-they-name.md): Shares source implementation: `packages/bundle/web-app/src/index.ts`, `packages/client/ui-primitives/src/markdown/CodeBlock.tsx`.
- **`shares-code-with`** — [0672. Replace tool-web's regex HTML-to-markdown converter with turndown](0672-replace-tool-web-s-regex-html-to-markdown-converter-with-turndown.md): Shares source implementation: `apps/web`, `apps/web/package.json`.
- **`shares-code-with`** — [0407. CI failover runbook --- hosted pools → in-house pool](0407-ci-failover-runbook-hosted-pools-in-house-pool.md): Shares source implementation: `vitest.e2e.config.ts`.
- **`shares-code-with`** — [0431. Dual Wine and native Windows pull-request CI](0431-dual-wine-and-native-windows-pull-request-ci.md): Shares source implementation: `vitest.e2e.config.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0314-web-gui-changes-close-the-loop-on-the-existing-url.md`.
