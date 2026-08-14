---
id: "dsh-note-0269"
title: "Web install manifest metadata"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-06-web-install-manifest.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/performance"
  - "concern/recovery"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/filesystem"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "html"
  - "scope"
  - "lang"
  - "/manifest.webmanifest"
  - "apps/web/public/"
  - "DeepSeek Harness"
  - "DSH"
  - "start_url"
  - ". It requests"
  - "/favicon.svg"
  - "window-controls-overlay"
  - "theme_color"
  - "background_color"
  - "dsh-host-frontend-static"
search_regex: "(?i)(html|scope|lang|/manifest\\.webmanifest|apps/web/public/|DeepSeek[- ]Harness|start_url|\\.[- ]It[- ]requests)"
---

# 0269. Web install manifest metadata — implementation context

## Open this when

The Web build has a document title and favicon but no manifest from which a browser can discover a stable installed identity, launch boundary, or installed presentation. Adding that metadata can also imply capabilities the app does not provide: a service worker suggests an offline contract, while a single language or palette value misrepresents a bilingual UI with resolved light and dark themes.

## Source decision

The Web entry links /manifest.webmanifest, which Vite copies from apps/web/public/ into the production build. The manifest names the product DeepSeek Harness, gives installed chrome the compact name DSH, and fixes id, start_url, and scope at /. It requests display: "fullscreen" so supporting browsers can give the installed editor-like surface the available display area while leaving ordinary tabs unchanged; browsers may apply user overrides or fall back to another display mode. Its icon entry reuses /favicon.svg as an SVG of size any and purpose any.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-06-web-install-manifest.md](../02-notes/implemented/feature/2026-08-06-web-install-manifest.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-06-web-install-manifest.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-06-web-install-manifest.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/host/frontend-static/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/host/frontend-static`. | `named-file, named-package-member` |
| [`packages/core/scope/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/store.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts) | runtime implementation | Core file in the package named by the note: `packages/core/scope`. Defines `scope`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/scope/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/core/scope`. | `named-package-member` |
| [`packages/host/frontend-static/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/frontend-static`. | `named-package-member` |
| [`packages/host/frontend-static/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/frontend-static`. | `named-package-member` |
| [`apps/web/public`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/web/public) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/core/scope`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/host/frontend-static`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/fs/tool-fs/src/read.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts) | runtime implementation | Defines `lang`, a construct named by the note. | `symbol-definition` |
| [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx) | runtime implementation | Defines `html`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `html` | `const` | [`packages/client/ui-primitives/src/markdown/CodeBlock.tsx:32`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/markdown/CodeBlock.tsx#L32) | `const html = useMemo(() => highlightToHtml(trimmed, lang), [trimmed, lang, loaded])` |
| `scope` | `function` | [`packages/core/scope/src/index.ts:121`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/index.ts#L121) | `function scope(): void {}` |
| `scope` | `const` | [`packages/core/scope/src/store.ts:231`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/scope/src/store.ts#L231) | `const scope = scopeOf(ctx)` |
| `lang` | `const` | [`packages/fs/tool-fs/src/read.ts:124`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/src/read.ts#L124) | `const lang = langFromPath(value.path)` |

### Tests and executable evidence

- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `DSH`.
- [`packages/fs/tool-fs/tests/tools.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/tools.spec.ts) — A test under the owning area exercises or imports `lang`.
- [`packages/fs/tool-fs/tests/read-render.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/tool-fs/tests/read-render.spec.ts) — A test under the owning area exercises or imports `lang`.
- [`apps/cli/tests/lazy-search-startup.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/lazy-search-startup.compat.spec.ts) — A test under the owning area exercises or imports `html`.
- [`packages/host/frontend-static/tests/frontend-static.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static/tests/frontend-static.spec.ts) — A test under the owning area exercises or imports `dsh-host-frontend-static`.
- [`packages/client/ui-primitives/tests/markdown.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown.client.spec.tsx) — A test under the owning area exercises or imports `html`.
- [`packages/client/ui-primitives/tests/code-block.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/code-block.client.spec.tsx) — A test under the owning area exercises or imports `html`.
- [`packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/tests/markdown-dom-parity.client.spec.tsx) — A test under the owning area exercises or imports `html`.
- Source verification intent: The built-Web test parses the emitted manifest and pins the complete metadata object, including the human-visible name, compact name, icon, root identity, launch boundary, and display mode, while also verifying that the production index.html retains the link. The dsh-host-frontend-static real Loader composition test serves a .webmanifest fixture and pins its application/manifest+json media type.

## How to read the implementation

1. Start with [`packages/host/frontend-static/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/frontend-static/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/evidence`, `concern/human-control`, `concern/performance`, `concern/recovery`, `domain/build-release`, `domain/configuration`, `domain/filesystem`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `html`, `scope`, `lang`, `/manifest.webmanifest`, `apps/web/public/`, `DeepSeek Harness`, `DSH`, `start_url`, `. It requests`, `/favicon.svg`, `window-controls-overlay`, `theme_color`, `background_color`, `dsh-host-frontend-static`
- Regex: `(?i)(html|scope|lang|/manifest\.webmanifest|apps/web/public/|DeepSeek[- ]Harness|start_url|\.[- ]It[- ]requests)`

```bash
rg -n --pcre2 "(?i)(html|scope|lang|/manifest\\.webmanifest|apps/web/public/|DeepSeek[- ]Harness|start_url|\\.[- ]It[- ]requests)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0201. Cross-workspace session resume](0201-cross-workspace-session-resume.md): Shares source implementation: `apps/cli`, `packages/core/scope`.
- **`shares-code-with`** — [0052. The slot system standard --- single register, four props shares, and the framework store seat](0052-the-slot-system-standard-single-register-four-props-shares-and-the-frame.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0061. Web client Agent-scope parity model and the provisioning channel (agents/scope / blank reuse / provide)](0061-web-client-agent-scope-parity-model-and-the-provisioning-channel-agents.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0173. Durable subagent catalog and list_agents](0173-durable-subagent-catalog-and-list-agents.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0116. The skill registry is host-held and layered per scope](0116-the-skill-registry-is-host-held-and-layered-per-scope.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0394. TypeScript Program-backed semantic gates](0394-typescript-program-backed-semantic-gates.md): Shares source implementation: `packages/core/scope`, `packages/core/scope/src/index.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.
- **`shares-code-with`** — [0036. Shared scoped-layer storage](0036-shared-scoped-layer-storage.md): Shares source implementation: `packages/core/scope/src/index.ts`, `packages/core/scope/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0269-web-install-manifest-metadata.md`.
