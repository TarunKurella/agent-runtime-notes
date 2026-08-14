---
id: "dsh-note-0444"
title: "npm access per release sequence: the vendored framework and the native packages publish publicly"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-08-13-public-vendor-and-native-sequences.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/ownership"
  - "concern/trust"
  - "domain/build-release"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/security"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "dependency"
  - "access"
  - "publishConfig.access: restricted"
  - "@deepseek-ai"
  - "dsh@0.0.1-rc.5"
  - "vendor *-rc.4"
  - "landlock-run@0.0.1"
  - "peerDependency"
  - "dsh-sandbox-local"
  - "publishConfig.access"
  - "vendor/*"
  - "native/landlock-run/packages/*"
  - "packages/*/*"
  - "apps/*"
search_regex: "(?i)(dependency|access|publishConfig\\.access:[- ]restricted|@deepseek\\-ai|dsh@0\\.0\\.1\\-rc\\.5|vendor[- ]\\*\\-rc\\.4|landlock\\-run@0\\.0\\.1|peerDependency)"
---

# 0444. npm access per release sequence: the vendored framework and the native packages publish publicly — implementation context

## Open this when

The three release sequences shipped with publishConfig.access: restricted, so every package published to the @deepseek-ai scope was visible only inside the organization. Five rehearsal publications ran that way, through dsh@0.0.1-rc.5, vendor -rc.4, and landlock-run@0.0.1. A restricted dependency is what actually blocks a public consumer. Every harness package declares the vendored framework as a peerDependency, and dsh-sandbox-local declares the Landlock entry as a dependency.

## Source decision

Access is a property of each release sequence, not of the scope: check-workspace-constraints.ts holds every manifest to its own sequence's level, which is what stops the scope from drifting: a new vendor/ package left at restricted, or a dsh member flipped to public, fails the workspace constraints. No publish path passes --access. A single flag cannot serve sequences that disagree, and a flag overrides the manifest that owns the fact --- so publish.ts passes none, and the native workflow continues to pass none. Each packed manifest decides.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-08-13-public-vendor-and-native-sequences.md](../02-notes/implemented/process/2026-08-13-public-vendor-and-native-sequences.md)
- Pinned source: [.agents/notes/implemented/process/2026-08-13-public-vendor-and-native-sequences.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-08-13-public-vendor-and-native-sequences.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/sandbox/sandbox-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox-local`. | `named-package-member` |
| [`packages/sandbox/sandbox-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox-local`. | `named-package-member` |
| [`vendor/cordis/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `vendor/cordis`. | `named-directory-member` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Entry point or contract under the directory named by the note: `vendor/cordis`. | `named-directory-member` |
| [`vendor/cordis/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `vendor/cordis`. Contains the exact code literal `vendor/cordis` named by the note. | `exact-code-occurrence, named-directory-member` |
| [`vendor/cordis/src/logger.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/logger.ts) | runtime implementation | Entry point or contract under the directory named by the note: `vendor/cordis`. | `named-directory-member` |
| [`vendor/cordis`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`native/landlock-run/packages`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/native/landlock-run/packages) | package or module directory | The source note names this implementation area directly. | `named-directory` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/sandbox/sandbox-local`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local) | package or module directory | The note names this package or capability. | `named-package` |
| [`vendor/loader/src/config/isolate.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/isolate.ts) | runtime implementation | Defines `access`, a construct named by the note. | `symbol-definition` |
| [`packages/client/runtime/src/client/sessions/conversation-assembler.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts) | runtime implementation | Defines `dependency`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dependency` | `const` | [`packages/client/runtime/src/client/sessions/conversation-assembler.ts:576`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/runtime/src/client/sessions/conversation-assembler.ts#L576) | `const dependency = pending[index]` |
| `access` | `function` | [`vendor/loader/src/config/isolate.ts:75`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/loader/src/config/isolate.ts#L75) | `function access(entry: Entry, name: string, create: true): symbol` |

### Tests and executable evidence

- [`packages/sandbox/sandbox-local/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/local.spec.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/landlock.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/landlock.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/seatbelt.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/seatbelt.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/acl-grants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/acl-grants.spec.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/packed-install.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/packed-install.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`scripts/release/families.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/release/families.spec.ts) — Contains the exact code literal `vendor/cordis` named by the note.

## How to read the implementation

1. Start with [`packages/sandbox/sandbox-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/ownership`, `concern/trust`, `domain/build-release`, `domain/extensions`, `domain/filesystem`, `domain/security`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `dependency`, `access`, `publishConfig.access: restricted`, `@deepseek-ai`, `dsh@0.0.1-rc.5`, `vendor *-rc.4`, `landlock-run@0.0.1`, `peerDependency`, `dsh-sandbox-local`, `publishConfig.access`, `vendor/*`, `native/landlock-run/packages/*`, `packages/*/*`, `apps/*`
- Regex: `(?i)(dependency|access|publishConfig\.access:[- ]restricted|@deepseek\-ai|dsh@0\.0\.1\-rc\.5|vendor[- ]\*\-rc\.4|landlock\-run@0\.0\.1|peerDependency)`

```bash
rg -n --pcre2 "(?i)(dependency|access|publishConfig\\.access:[- ]restricted|@deepseek\\-ai|dsh@0\\.0\\.1\\-rc\\.5|vendor[- ]\\*\\-rc\\.4|landlock\\-run@0\\.0\\.1|peerDependency)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0439. Private npm publication as three independent sequences](0439-private-npm-publication-as-three-independent-sequences.md): The source note links to this decision directly.
- **`shares-code-with`** — [0379. pnpm as the package manager instead of Yarn 4](0379-pnpm-as-the-package-manager-instead-of-yarn-4.md): Shares source implementation: `vendor/cordis`, `vendor/cordis/README.md`.
- **`shares-code-with`** — [0355. Broken presets are roster rows, not gaps](0355-broken-presets-are-roster-rows-not-gaps.md): Shares source implementation: `vendor/cordis`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): Shares source implementation: `apps/cli`, `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0121. Remote event delivery (ctx.remote.$on)](0121-remote-event-delivery-ctx-remote-on.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares source implementation: `vendor/cordis/src/index.ts`, `vendor/cordis/src/logger.ts`.
- **`shares-code-with`** — [0279. Windows sandbox rung: raw ACL restricted tokens over mxc and AppContainer](0279-windows-sandbox-rung-raw-acl-restricted-tokens-over-mxc-and-appcontainer.md): Shares source implementation: `packages/sandbox/sandbox-local/src/index.ts`, `packages/sandbox/sandbox-local/src/invariant.ts`.
- **`shares-code-with`** — [0238. Workspace-write defaults for shipped surfaces](0238-workspace-write-defaults-for-shipped-surfaces.md): Shares source implementation: `packages/sandbox/sandbox-local/src/index.ts`, `packages/sandbox/sandbox-local/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0444-npm-access-per-release-sequence-the-vendored-framework-and-the-native-pa.md`.
