---
id: "dsh-note-0293"
title: "Minimal profiles use the bare two-tool runtime"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-11-minimal-profiles-bare-two-tool-runtime.md"
implementation_evidence: "high"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/schema-types"
  - "concern/trust"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/context"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/tools"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
aliases:
  - "model"
  - "initialize"
  - "str_replace_editor"
  - "fs-sandbox"
  - "dsh-system-prompt"
  - "@deepseek-ai/dsh-fs-local"
  - "ctx.fs"
  - "fs-local"
  - "minimal.cordis.yml"
  - "dsh-sdk-jsonrpc-server"
  - "token-meter"
  - "compaction-basic"
  - "fs-observation-policy"
  - "DSH_SYSTEM_PROMPT"
search_regex: "(?i)(model|initialize|str_replace_editor|fs\\-sandbox|dsh\\-system\\-prompt|@deepseek\\-ai/dsh\\-fs\\-local|ctx\\.fs|fs\\-local)"
---

# 0293. Minimal profiles use the bare two-tool runtime — implementation context

## Open this when

The Web minimal preset and standalone JSON-RPC minimal composition exposed persistent bash and str_replace_editor, but their supporting services did not match the intended training runtime. Both mounted context compaction, while the Web preset inherited the host's sandboxed filesystem and the JSON-RPC composition mounted fs-sandbox plus filesystem policy. A long session could therefore replace history, and the editor advertised and enforced a filesystem policy that the bare local reference runtime does not have. The two launch paths also have different configuration owners.

## Source decision

Both shipped minimal profiles expose exactly persistent bash and str_replace_editor, mount no context-compaction provider, suppress every dsh-system-prompt runtime-context contribution for fresh sessions, and run the editor against @deepseek-ai/dsh-fs-local. The Web preset isolates ctx.fs inside the agent entry and mounts fs-local beside the editor, so other Web agents retain the host filesystem provider. Its persona remains the fixed complete prompt owned by the earlier minimal-preset composition decision and applies runtime-context suppression only to that agent scope.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-11-minimal-profiles-bare-two-tool-runtime.md](../02-notes/implemented/feature/2026-08-11-minimal-profiles-bare-two-tool-runtime.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-11-minimal-profiles-bare-two-tool-runtime.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-11-minimal-profiles-bare-two-tool-runtime.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`examples/jsonrpc-agent/minimal.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/minimal.py) | runtime implementation | The source note names this file directly. | `named-file` |
| [`examples/jsonrpc-agent/minimal.cordis.yml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/minimal.cordis.yml) | composition and configuration | The source note names this file directly. | `named-file` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/fs/fs-local/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/fs/fs-sandbox/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/index.ts) | package entry point | Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-package-member` |
| [`packages/sdk/server/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/fs/fs-local/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-local`. | `named-package-member` |
| [`packages/llm/token-meter/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/llm/token-meter/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |
| [`packages/fs/fs-sandbox/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/fs/fs-sandbox`. | `named-package-member` |
| [`packages/core/system-prompt/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/system-prompt/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/system-prompt`. | `named-package-member` |
| [`packages/llm/token-meter/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/token-meter/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/token-meter`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `model` | `const` | [`packages/compaction/compaction-basic/src/config.ts:260`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/compaction/compaction-basic/src/config.ts#L260) | `const model = config.summarizationModel` |
| `initialize` | `def` | [`python/sdk/src/deepseek_harness/client.py:117`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/src/deepseek_harness/client.py#L117) | `def initialize(` |

### Tests and executable evidence

- [`python/sdk/tests/test_client.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_client.py) — A test under the owning area exercises or imports `initialize`.
- [`python/sdk/tests/test_bundled_runtime.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/python/sdk/tests/test_bundled_runtime.py) — A test under the owning area exercises or imports `initialize`.
- [`packages/fs/fs-local/tests/filesystem.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-local/tests/filesystem.spec.ts) — A test under the owning area exercises or imports `fs-local`.
- [`packages/sdk/server/tests/plugin-apply.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/plugin-apply.spec.ts) — A test under the owning area exercises or imports `dsh-sdk-jsonrpc-server`.
- [`packages/sdk/server/tests/plugin-shape.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/plugin-shape.spec.ts) — A test under the owning area exercises or imports `dsh-sdk-jsonrpc-server`.
- [`packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-sandbox/tests/fs-sandbox.spec.ts) — A test under the owning area exercises or imports `sandboxMode`.
- [`packages/sdk/server/tests/built-scope-carrier.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/tests/built-scope-carrier.e2e.ts) — A test under the owning area exercises or imports `dsh-sdk-jsonrpc-server`.
- [`packages/fs/fs-observation-policy/tests/policy.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/fs/fs-observation-policy/tests/policy.spec.ts) — A test under the owning area exercises or imports `fs-observation-policy`.
- Source verification intent: The Web replay boots the complete Web host, creates the agent through the preset service, and asserts that the scoped filesystem is bare, no scoped compaction service exists, no system-prompt-owned runtime-context message was appended, and the assembled request contains exactly the fixed prompt and two tools. It then executes persistent Bash and the editor against the real scoped services.

## How to read the implementation

1. Start with [`examples/jsonrpc-agent/minimal.py`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/jsonrpc-agent/minimal.py) because it has the strongest evidence link to the note.
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

- Tags: `class/feature`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/schema-types`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/context`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/tools`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`
- Aliases: `model`, `initialize`, `str_replace_editor`, `fs-sandbox`, `dsh-system-prompt`, `@deepseek-ai/dsh-fs-local`, `ctx.fs`, `fs-local`, `minimal.cordis.yml`, `dsh-sdk-jsonrpc-server`, `token-meter`, `compaction-basic`, `fs-observation-policy`, `DSH_SYSTEM_PROMPT`
- Regex: `(?i)(model|initialize|str_replace_editor|fs\-sandbox|dsh\-system\-prompt|@deepseek\-ai/dsh\-fs\-local|ctx\.fs|fs\-local)`

```bash
rg -n --pcre2 "(?i)(model|initialize|str_replace_editor|fs\\-sandbox|dsh\\-system\\-prompt|@deepseek\\-ai/dsh\\-fs\\-local|ctx\\.fs|fs\\-local)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`source-link`** — [0358. The minimal preset owns the complete RL agent composition](0358-the-minimal-preset-owns-the-complete-rl-agent-composition.md): The source note links to this decision directly.
- **`shares-code-with`** — [0178. Web UI permission presets and approval answering](0178-web-ui-permission-presets-and-approval-answering.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-local/src/invariant.ts`.
- **`shares-code-with`** — [0542. Dependency swaps rejected by the 2026-07 NIH audit](0542-dependency-swaps-rejected-by-the-2026-07-nih-audit.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/sdk/server/src/index.ts`.
- **`shares-code-with`** — [0155. Cross-family file sandbox --- one policy home, a sandboxed fs provider, and fs escalation parity](0155-cross-family-file-sandbox-one-policy-home-a-sandboxed-fs-provider-and-fs.md): Shares source implementation: `packages/fs/fs-local/src/index.ts`, `packages/fs/fs-sandbox/src/index.ts`.
- **`shares-code-with`** — [0070. A capability-discriminated directory-picker seam for the web-GUI host](0070-a-capability-discriminated-directory-picker-seam-for-the-web-gui-host.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`, `packages/fs/fs-sandbox/src/invariant.ts`.
- **`shares-code-with`** — [0146. The subprocess sandbox --- confinement seam, native runners, escalation, and per-session modes](0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`.
- **`shares-code-with`** — [0238. Workspace-write defaults for shipped surfaces](0238-workspace-write-defaults-for-shipped-surfaces.md): Shares source implementation: `packages/fs/fs-sandbox/src/index.ts`, `packages/fs/fs-sandbox/src/invariant.ts`.
- **`shares-code-with`** — [0456. Remove the stdio and Echo agents](0456-remove-the-stdio-and-echo-agents.md): Shares source implementation: `packages/sdk/server/src/index.ts`, `packages/sdk/server/src/invariant.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0293-minimal-profiles-use-the-bare-two-tool-runtime.md`.
