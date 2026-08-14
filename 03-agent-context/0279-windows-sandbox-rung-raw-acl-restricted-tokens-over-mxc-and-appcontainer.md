---
id: "dsh-note-0279"
title: "Windows sandbox rung: raw ACL restricted tokens over mxc and AppContainer"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-08-windows-acl-restricted-token-sandbox.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/performance"
  - "concern/recovery"
  - "concern/schema-types"
  - "concern/simplification"
  - "concern/trust"
  - "domain/build-release"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "DISABLE_MAX_PRIVILEGE"
  - "LUA_TOKEN"
  - "WRITE_RESTRICTED"
  - "workspaceWriteSid"
  - "tempWriteSid"
  - "PLATFORM_CHAINS.win32"
  - "read-only"
  - "workspace-write"
  - "CreateRestrictedToken"
  - "S-1-4-x-y"
  - "SetTokenInformation"
  - "@deepseek-ai/dsh-sandbox-windows-acl"
  - "dsh-sandbox-local"
  - "@deepseek-ai/dsh-pwsh-sandbox"
search_regex: "(?i)(DISABLE_MAX_PRIVILEGE|LUA_TOKEN|WRITE_RESTRICTED|workspaceWriteSid|tempWriteSid|PLATFORM_CHAINS\\.win32|read\\-only|workspace\\-write)"
---

# 0279. Windows sandbox rung: raw ACL restricted tokens over mxc and AppContainer — implementation context

## Open this when

The original sandbox decision left PLATFORM_CHAINS.win32 empty, so shipped Windows profiles degraded to danger-full-access because no confining executor existed. The win32 rung must govern the two file-effect modes in the sandbox vocabulary --- read-only (no explicit writable root) and workspace-write (writes under the workspace root plus a backend-defined temp area) --- while reporting any effects its mechanism cannot govern; reads, network, and process visibility remain outside this vocabulary.

## Source decision

Implement the rung directly on the raw ACL mechanism: duplicate the caller's token into a WRITE_RESTRICTED token (CreateRestrictedToken with WRITE_RESTRICTED + DISABLE_MAX_PRIVILEGE + LUA_TOKEN) whose restricting SIDs carry distinct workspace and private-temp capabilities. WRITE_RESTRICTED intersects write accesses only, so reads keep the caller's ambient access while a write must also match one of these capability ACEs. The mechanism is the one huoyaoyuan/windows-acl-restrict-poc (10e4dfb) demonstrates; this port checks every API call and fails closed (the POC fail-opened on ignored failures).

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-08-windows-acl-restricted-token-sandbox.md](../02-notes/implemented/feature/2026-08-08-windows-acl-restricted-token-sandbox.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-08-windows-acl-restricted-token-sandbox.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-08-windows-acl-restricted-token-sandbox.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/shell/pwsh-sandbox/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/shell/pwsh-sandbox`. | `named-file, named-package-member` |
| [`packages/sandbox/sandbox-local/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/sandbox/sandbox-local`. | `named-file, named-package-member` |
| [`packages/sandbox/sandbox-windows-acl/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/README.md) | package contract and examples | The source note names this file directly. Core file in the package named by the note: `packages/sandbox/sandbox-windows-acl`. | `named-file, named-package-member` |
| [`packages/shell/pwsh-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/shell/pwsh-sandbox`. | `named-package-member` |
| [`packages/sandbox/sandbox-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox-local`. | `named-package-member` |
| [`packages/shell/pwsh-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/shell/pwsh-sandbox`. | `named-package-member` |
| [`packages/sandbox/sandbox-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox-local`. | `named-package-member` |
| [`packages/sandbox/sandbox-windows-acl/src/ffi.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/ffi.ts) | runtime implementation | Core file in the package named by the note: `packages/sandbox/sandbox-windows-acl`. | `named-package-member` |
| [`packages/sandbox/sandbox-windows-acl/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sandbox/sandbox-windows-acl`. | `named-package-member` |
| [`packages/sandbox/sandbox-windows-acl/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sandbox/sandbox-windows-acl`. | `named-package-member` |
| [`packages/sandbox/sandbox-windows-acl/src/win32-abi.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/win32-abi.ts) | runtime implementation | Core file in the package named by the note: `packages/sandbox/sandbox-windows-acl`. Defines `WRITE_RESTRICTED`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/sandbox/sandbox-windows-acl/src/workspace-sid.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/workspace-sid.ts) | runtime implementation | Core file in the package named by the note: `packages/sandbox/sandbox-windows-acl`. Defines `workspaceWriteSid`, a construct named by the note. | `named-package-member, symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `DISABLE_MAX_PRIVILEGE` | `const` | [`packages/sandbox/sandbox-windows-acl/src/win32-abi.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/win32-abi.ts#L84) | `export const DISABLE_MAX_PRIVILEGE = 0x1` |
| `LUA_TOKEN` | `const` | [`packages/sandbox/sandbox-windows-acl/src/win32-abi.ts:86`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/win32-abi.ts#L86) | `export const LUA_TOKEN = 0x4` |
| `WRITE_RESTRICTED` | `const` | [`packages/sandbox/sandbox-windows-acl/src/win32-abi.ts:88`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/win32-abi.ts#L88) | `export const WRITE_RESTRICTED = 0x8` |
| `workspaceWriteSid` | `function` | [`packages/sandbox/sandbox-windows-acl/src/workspace-sid.ts:35`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/workspace-sid.ts#L35) | `export function workspaceWriteSid(workspaceRoot: string): string {` |
| `tempWriteSid` | `function` | [`packages/sandbox/sandbox-windows-acl/src/workspace-sid.ts:49`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/src/workspace-sid.ts#L49) | `export function tempWriteSid(tempDir: string): string {` |

### Tests and executable evidence

- [`packages/shell/pwsh-sandbox/tests/acl.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/tests/acl.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/bwrap.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/bwrap.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/local.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/local.spec.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/landlock.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/landlock.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-local/tests/seatbelt.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/seatbelt.e2e.ts) — A test under the owning area exercises or imports `dsh-sandbox-local`.
- [`packages/sandbox/sandbox-windows-acl/tests/acl.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/tests/acl.spec.ts) — A test under the owning area exercises or imports `tempWriteSid`. A test under the owning area exercises or imports `SetNamedSecurityInfoW`.
- [`packages/sandbox/sandbox-local/tests/acl-grants.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-local/tests/acl-grants.spec.ts) — A test under the owning area exercises or imports `workspaceWriteSid`. A test under the owning area exercises or imports `tempWriteSid`.
- [`packages/sandbox/sandbox-windows-acl/tests/probe.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sandbox/sandbox-windows-acl/tests/probe.spec.ts) — A test under the owning area exercises or imports `WRITE_RESTRICTED`. A test under the owning area exercises or imports `tempWriteSid`.
- Source verification intent: The product-visible Windows roster flip is win32-only, so keyless snapshots that must replay on macOS/Linux cannot cover it; bundle composition specs plus the win32 real-runner suites are the substitute evidence, and the CI Windows lane owns the assembled signal. sandbox-local/tests/acl-grants.spec.ts pins random temp allocation, per-session/workspace reuse, fork/workspace separation, crash-resume non-collision, paired argv SIDs, failure cleanup, and standing-versus-revocable lifecycle with Win32 mocked.

## How to read the implementation

1. Start with [`packages/shell/pwsh-sandbox/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/shell/pwsh-sandbox/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/recovery`, `concern/schema-types`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/filesystem`, `domain/llm`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/testing`, `lifecycle/implemented`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `DISABLE_MAX_PRIVILEGE`, `LUA_TOKEN`, `WRITE_RESTRICTED`, `workspaceWriteSid`, `tempWriteSid`, `PLATFORM_CHAINS.win32`, `read-only`, `workspace-write`, `CreateRestrictedToken`, `S-1-4-x-y`, `SetTokenInformation`, `@deepseek-ai/dsh-sandbox-windows-acl`, `dsh-sandbox-local`, `@deepseek-ai/dsh-pwsh-sandbox`
- Regex: `(?i)(DISABLE_MAX_PRIVILEGE|LUA_TOKEN|WRITE_RESTRICTED|workspaceWriteSid|tempWriteSid|PLATFORM_CHAINS\.win32|read\-only|workspace\-write)`

```bash
rg -n --pcre2 "(?i)(DISABLE_MAX_PRIVILEGE|LUA_TOKEN|WRITE_RESTRICTED|workspaceWriteSid|tempWriteSid|PLATFORM_CHAINS\\.win32|read\\-only|workspace\\-write)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): The source note links to this decision directly.
- **`source-link`** — [0240. PowerShell executor and pwsh tool](0240-powershell-executor-and-pwsh-tool.md): The source note links to this decision directly.
- **`source-link`** — [0532. Evaluate landstrip before building a Windows sandbox launcher](0532-evaluate-landstrip-before-building-a-windows-sandbox-launcher.md): The source note links to this decision directly.
- **`shares-code-with`** — [0238. Workspace-write defaults for shipped surfaces](0238-workspace-write-defaults-for-shipped-surfaces.md): Shares source implementation: `packages/sandbox/sandbox-local/src/index.ts`, `packages/sandbox/sandbox-local/src/invariant.ts`.
- **`shares-code-with`** — [0444. npm access per release sequence: the vendored framework and the native packages publish publicly](0444-npm-access-per-release-sequence-the-vendored-framework-and-the-native-pa.md): Shares source implementation: `packages/sandbox/sandbox-local/src/index.ts`, `packages/sandbox/sandbox-local/src/invariant.ts`.
- **`same-design-pressure`** — [0158. persistent PTY sessions](0158-persistent-pty-sessions.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`same-design-pressure`** — [0557. Agent Client Protocol (ACP) support --- drive the coding agent from external editors](0557-agent-client-protocol-acp-support-drive-the-coding-agent-from-external-e.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/discovery-routing`.
- **`shares-code-with`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): Shares source implementation: `packages/sandbox/sandbox-windows-acl/src/index.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0279-windows-sandbox-rung-raw-acl-restricted-tokens-over-mxc-and-appcontainer.md`.
