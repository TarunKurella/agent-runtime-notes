---
id: "dsh-note-0238"
title: "Workspace-write defaults for shipped surfaces"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-07-31-workspace-write-surface-default.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
aliases:
  - "defaultPreset"
  - "danger-full-access"
  - "base.cordis.yml"
  - "dsh-sandbox-local"
  - "dsh-sandbox-policy"
  - "dsh-bash-sandbox"
  - "dsh-fs-sandbox"
  - "dsh-user-approval"
  - "dsh-permission-presets"
  - "workspace-write"
  - "DSH_PERMISSION_MODE"
  - "permission.defaultPreset"
  - "permission/preset: workspace-write"
  - "sandbox/mode: workspace-write"
search_regex: "(?i)(defaultPreset|danger\\-full\\-access|base\\.cordis\\.yml|dsh\\-sandbox\\-local|dsh\\-sandbox\\-policy|dsh\\-bash\\-sandbox|dsh\\-fs\\-sandbox|dsh\\-user\\-approval)"
---

# 0238. Workspace-write defaults for shipped surfaces — implementation context

## Open this when

The shipped terminal and browser surfaces exposed the same coding tools under different unconfined compositions. Web mounted the sandbox and permission services but selected danger-full-access; the TUI mounted the unrestricted local bash and filesystem providers directly. A fresh coding session could therefore mutate any path its same-UID process could reach before the user deliberately chose that authority.

## Source decision

base.cordis.yml owns one sandbox and permission stack for every shipped TUI, Web, and browser-backed headless session: dsh-sandbox-local, dsh-sandbox-policy, dsh-bash-sandbox, dsh-fs-sandbox, dsh-user-approval, and dsh-permission-presets. The composition fallback is the workspace-write preset, which bundles workspace-write file effects with the ask approval policy. DSH_PERMISSION_MODE remains an explicit process override; a stored permission.defaultPreset remains the user preference for later sessions and outranks the fallback through the Settings seam.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-07-31-workspace-write-surface-default.md](../02-notes/implemented/feature/2026-07-31-workspace-write-surface-default.md)
- Pinned source: [.agents/notes/implemented/feature/2026-07-31-workspace-write-surface-default.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-07-31-workspace-write-surface-default.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/fs/fs-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-package-member` |
| [`packages/fs/fs-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-package-member` |
| [`packages/shell/bash-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |
| [`packages/sandbox/sandbox-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox-local`. | `named-package-member` |
| [`packages/sandbox/sandbox-policy/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox-policy`. | `named-package-member` |
| [`packages/shell/bash-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/bash-sandbox`. | `named-package-member` |
| [`packages/interaction/user-approval/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/index.ts) | package entry point | Core file in the package named by the note: `packages/interaction/user-approval`. | `named-package-member` |
| [`packages/interaction/user-approval/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/interaction/user-approval`. | `named-package-member` |
| [`packages/sandbox/sandbox-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox-local`. | `named-package-member` |
| [`packages/sandbox/sandbox-policy/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-policy/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox-policy`. | `named-package-member` |
| [`packages/interaction/user-approval/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/user-approval/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/interaction/user-approval`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `defaultPreset` | `const` | [`packages/interaction/permission-presets/src/index.ts:196`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/interaction/permission-presets/src/index.ts#L196) | `const defaultPreset = config.defaultPreset ?? inferredDefault` |

### Tests and executable evidence

- [`packages/shell/bash-sandbox/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`. A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts) — A test under the owning area exercises or imports `dsh-fs-sandbox`.
- [`packages/shell/bash-sandbox/tests/landlock.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/landlock.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`. A test under the owning area exercises or imports `dsh-sandbox-policy`.
- [`packages/shell/bash-sandbox/tests/seatbelt.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/seatbelt.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`. A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/sandbox/sandbox-local/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/shell/bash-sandbox/tests/sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/bash-sandbox/tests/sandbox.spec.ts) — A test under the owning area exercises or imports `dsh-bash-sandbox`.
- [`packages/sandbox/sandbox-local/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/local.spec.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/landlock.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/landlock.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- Source verification intent: The keyless shipped-TUI pseudo-terminal smoke boots the real Loader tree, reads the persisted first request, and asserts both the sandbox_permissions/justification bash schema and the initial workspace-write event triplet. The shipped-Web composition smoke asserts the same policy, approval, and Permission defaults. The assembled browser Settings snapshot opens on Workspace Write, preserves an existing workspace-write session while changing the future default, and still proves the confirmed Full-access path.

## How to read the implementation

1. Start with [`packages/bundle/base/cordis.patch.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/bundle/base/cordis.patch.yml) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`
- Aliases: `defaultPreset`, `danger-full-access`, `base.cordis.yml`, `dsh-sandbox-local`, `dsh-sandbox-policy`, `dsh-bash-sandbox`, `dsh-fs-sandbox`, `dsh-user-approval`, `dsh-permission-presets`, `workspace-write`, `DSH_PERMISSION_MODE`, `permission.defaultPreset`, `permission/preset: workspace-write`, `sandbox/mode: workspace-write`
- Regex: `(?i)(defaultPreset|danger\-full\-access|base\.cordis\.yml|dsh\-sandbox\-local|dsh\-sandbox\-policy|dsh\-bash\-sandbox|dsh\-fs\-sandbox|dsh\-user\-approval)`

```bash
rg -n --pcre2 "(?i)(defaultPreset|danger\\-full\\-access|base\\.cordis\\.yml|dsh\\-sandbox\\-local|dsh\\-sandbox\\-policy|dsh\\-bash\\-sandbox|dsh\\-fs\\-sandbox|dsh\\-user\\-approval)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`, `packages/fs/fs-sandbox/src/invariant.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`, `packages/fs/fs-sandbox/src/invariant.ts`.
- **`shares-code-with`** — [0293. Minimal profiles use the bare two-tool runtime](0293-minimal-profiles-use-the-bare-two-tool-runtime.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`, `packages/fs/fs-sandbox/src/invariant.ts`.
- **`shares-code-with`** — [0279. Windows sandbox rung: raw ACL restricted tokens over mxc and AppContainer](0279-windows-sandbox-rung-raw-acl-restricted-tokens-over-mxc-and-appcontainer.md): Shares source implementation: `packages/sandbox/sandbox-local/src/index.ts`, `packages/sandbox/sandbox-local/src/invariant.ts`.
- **`shares-code-with`** — [0229. Even out the shipped tool rosters](0229-even-out-the-shipped-tool-rosters.md): Shares source implementation: `packages/bundle/base/cordis.patch.yml`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`.
- **`shares-code-with`** — [0281. Continuable subagent policy inheritance --- the durable child log owns the delegation-time snapshot](0281-continuable-subagent-policy-inheritance-the-durable-child-log-owns-the-d.md): Shares source implementation: `packages/interaction/user-approval/src/index.ts`, `packages/sandbox/sandbox-policy/src/index.ts`.
- **`shares-code-with`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0238-workspace-write-defaults-for-shipped-surfaces.md`.
