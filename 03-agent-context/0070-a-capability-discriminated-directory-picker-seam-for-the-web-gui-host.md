---
id: "dsh-note-0070"
title: "A capability-discriminated directory-picker seam for the web-GUI host"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-28-directory-picker-capability-seam.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/registry"
aliases:
  - "separatorOf"
  - "sep"
  - "scanned"
  - "hidden"
  - "busy"
  - "listDirectory"
  - "list"
  - "entries"
  - "capability"
  - "DirectoryListing"
  - "home"
  - "truncated"
  - "dialog"
  - "separator"
search_regex: "(?i)(separatorOf|scanned|hidden|busy|listDirectory|list|entries|capability)"
---

# 0070. A capability-discriminated directory-picker seam for the web-GUI host — implementation context

## Open this when

The web GUI's "Open local folder" flow was hardwired to one interaction: host.pickDirectory invoked a native OS chooser compiled into dsh-host-apiproxy (private module, test-only injection point). That shape cannot serve remote deployments --- no OS dialog reaches a browser on another machine --- and the in-app directory browser (Figma Harness 802-56979) needs listing/creation primitives, which are a different interaction contract, not a different implementation of the same one. Swapping interactions required editing gateway source, against the repo's everything-is-a-plugin stance.

## Source decision

A three-package capability seam in packages/host/ --- directory-picker (Service Definition), directory-picker-native, directory-picker-browse (backends) --- with one contract method: capability() returns a discriminated union, { kind: 'native', pick(signal) } or { kind: 'browse', list(path?), createDirectory(path, name) }. The gateway (dsh-host-apiproxy) injects directoryPicker, serves the matching RPCs, and answers directory-picker-unavailable for the other kind.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-28-directory-picker-capability-seam.md](../02-notes/implemented/architecture/2026-07-28-directory-picker-capability-seam.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-28-directory-picker-capability-seam.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-28-directory-picker-capability-seam.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/fs/fs-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-package-member` |
| [`packages/host/apiproxy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/apiproxy/src/api/host.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/host.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/host`. Core file in the package named by the note: `packages/host/apiproxy`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/host/apiproxy/src/api-proxy.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts) | runtime implementation | Entry point or contract under the directory named by the note: `packages/host`. Core file in the package named by the note: `packages/host/apiproxy`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/fs/fs-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-package-member` |
| [`packages/host/apiproxy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/apiproxy`. | `named-package-member` |
| [`packages/host/directory-picker/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/host`. Core file in the package named by the note: `packages/host/directory-picker`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/host/directory-picker/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/directory-picker`. | `named-package-member` |
| [`packages/host/directory-picker-browse/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-browse/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `packages/host`. Core file in the package named by the note: `packages/host/directory-picker-browse`. | `named-directory-member, named-package-member, symbol-definition` |
| [`packages/host/directory-picker-native/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/index.ts) | package entry point | Core file in the package named by the note: `packages/host/directory-picker-native`. | `named-package-member` |
| [`packages/host/directory-picker-browse/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-browse/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/directory-picker-browse`. | `named-package-member` |
| [`packages/host/directory-picker-native/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-native/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/host/directory-picker-native`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `separatorOf` | `function` | [`packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx:120`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx#L120) | `function separatorOf(listing: DirectoryListing): '\\' \| '/' {` |
| `sep` | `const` | [`packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx:126`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx#L126) | `const sep = separatorOf(listing)` |
| `scanned` | `const` | [`packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx:364`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-directory-picker-browse/src/client/DirectoryBrowser.tsx#L364) | `const scanned = useRef<ScannedDirectory \| null>(null)` |
| `hidden` | `const` | [`packages/client/ui-primitives/src/DiffBlock.tsx:159`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-primitives/src/DiffBlock.tsx#L159) | `const hidden = rows.length - maxLines` |
| `busy` | `const` | [`packages/extensions/ui-cordis/src/client/CordisPanel.tsx:208`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/extensions/ui-cordis/src/client/CordisPanel.tsx#L208) | `const busy = pending.has(pluginId) \|\| activity?.phase === 'orchestrating'` |
| `listDirectory` | `function` | [`packages/fs/fs-local/src/fsio.ts:282`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/fsio.ts#L282) | `export async function listDirectory(target: LocalTarget, signal?: AbortSignal): Promise<LocalDirEntry[]> {` |
| `list` | `const` | [`packages/hooks/hook-protocol/src/merge.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/merge.ts#L75) | `const list = reasonsByRank.get(r) ?? []` |
| `entries` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:335`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L335) | `const entries = await Promise.all(models.map(async (model) => {` |
| `entries` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:961`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L961) | `const entries = await ctx.subagents.listChildren(parentSessionId, signal)` |
| `entries` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2639`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2639) | `const entries = await ctx.subagents.listChildren(request.payload.parentSessionId, signal)` |
| `capability` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2942`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2942) | `const capability = ctx.directoryPicker.capability()` |
| `capability` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2970`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2970) | `const capability = ctx.directoryPicker.capability()` |
| `capability` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:2993`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L2993) | `const capability = ctx.directoryPicker.capability()` |
| `entries` | `const` | [`packages/host/apiproxy/src/api-proxy.ts:3325`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api-proxy.ts#L3325) | `const entries = await Promise.all(request.payload.refs.map(async (ref) => {` |
| `DirectoryListing` | `interface` | [`packages/host/apiproxy/src/api/host.ts:19`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/src/api/host.ts#L19) | `export interface DirectoryListing {` |
| `home` | `const` | [`packages/host/directory-picker-browse/src/index.ts:218`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker-browse/src/index.ts#L218) | `const home = homedir()` |

### Tests and executable evidence

- [`packages/fs/fs-local/tests/fsio.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/fsio.spec.ts) — A test under the owning area exercises or imports `listDirectory`.
- [`packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts) — A test under the owning area exercises or imports `homedir`.
- [`packages/jobs/tool-jobs/tests/tool-jobs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/jobs/tool-jobs/tests/tool-jobs.spec.ts) — A test under the owning area exercises or imports `separator`.
- [`packages/host/directory-picker/tests/seam.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/directory-picker/tests/seam.spec.ts) — A test under the owning area exercises or imports `directoryPicker`.
- [`packages/host/apiproxy/tests/fetch-carrier.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/fetch-carrier.spec.ts) — A test under the owning area exercises or imports `pickDirectory`. A test under the owning area exercises or imports `listDirectory`.
- [`packages/host/apiproxy/tests/client-handler.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/client-handler.spec.ts) — A test under the owning area exercises or imports `pickDirectory`. A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/api-proxy-fork.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-fork.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.
- [`packages/host/apiproxy/tests/api-proxy-blank.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/host/apiproxy/tests/api-proxy-blank.spec.ts) — A test under the owning area exercises or imports `dsh-host-apiproxy`.

## How to read the implementation

1. Start with [`packages/fs/fs-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/index.ts) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/registry`
- Aliases: `separatorOf`, `sep`, `scanned`, `hidden`, `busy`, `listDirectory`, `list`, `entries`, `capability`, `DirectoryListing`, `home`, `truncated`, `dialog`, `separator`
- Regex: `(?i)(separatorOf|scanned|hidden|busy|listDirectory|list|entries|capability)`

```bash
rg -n --pcre2 "(?i)(separatorOf|scanned|hidden|busy|listDirectory|list|entries|capability)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0209. Adaptive default for the directory-picker interaction](0209-adaptive-default-for-the-directory-picker-interaction.md): The source note links to this decision directly.
- **`shares-code-with`** — [0198. Web subagent catalog and human continuation](0198-web-subagent-catalog-and-human-continuation.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0197. Web past-session search](0197-web-past-session-search.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0224. Web result card --- a structured render intent for web_search and web_fetch](0224-web-result-card-a-structured-render-intent-for-web-search-and-web-fetch.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`, `packages/fs/fs-sandbox/src/invariant.ts`.
- **`shares-code-with`** — [0513. Record last activity in the session index](0513-record-last-activity-in-the-session-index.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`.
- **`shares-code-with`** — [0375. Bound cold blank-session verification](0375-bound-cold-blank-session-verification.md): Shares source implementation: `packages/host/apiproxy/src/api-proxy.ts`, `packages/host/apiproxy/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md`.
