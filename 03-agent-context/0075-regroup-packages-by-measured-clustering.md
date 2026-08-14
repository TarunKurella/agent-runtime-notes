---
id: "dsh-note-0075"
title: "Regroup packages/ by measured clustering"
status: "implemented"
class: "architecture"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/architecture/2026-07-29-package-regrouping.md"
implementation_evidence: "medium"
target_anchor: "sandbox capability boundary"
tags:
  - "class/architecture"
  - "concern/boundary"
  - "concern/cancellation-timeout"
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
  - "domain/filesystem"
  - "domain/llm"
  - "domain/multi-agent"
  - "domain/observability"
  - "domain/protocols"
  - "domain/security"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
  - "mechanism/registry"
aliases:
  - "web"
  - "json"
  - "references"
  - "sandbox"
  - "permission"
  - "commands"
  - "skill"
  - "packages/<group>/<pkg>"
  - "ui/"
  - "dsh-sdk-protocol"
  - "user-questions"
  - "user-approval"
  - "tool-ask-user"
  - "app-boot"
search_regex: "(?i)(json|references|sandbox|permission|commands|skill|packages/<group>/<pkg>|dsh\\-sdk\\-protocol)"
---

# 0075. Regroup packages/ by measured clustering — implementation context

## Open this when

The two-level packages// hierarchy (original decision) had drifted since June: 167 packages sat in 42 groups, and several group boundaries no longer matched how the packages actually cluster. ui/ mixed four unrelated planes: the human terminal channel (tui), the SDK's JSON-RPC server half (jsonrpc, whose peer dependency on dsh-sdk-protocol binds it to the SDK wire stack), the human-interaction seams (user-questions, user-approval, permission, tool-ask-user, commands), and channel-neutral boot glue (app-boot). Its own README narrated the mixture instead of stating a role.

## Source decision

Five regrouping decisions remain current; every other group keeps its prior boundary and contents (the dependency analysis confirmed the capability families --- shell/, terminal/, code-runtime/, sandbox/, subprocess/, fs/, lsp/, web/, skill/, and the rest --- were already drawn correctly). The original sixth decision collected the SDK project initializer, launcher tooling, and runtime JSON-RPC packages under scaffold/; removing that unreleased toolchain deleted the project tooling and moved the surviving runtime trio to sdk/.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/architecture/2026-07-29-package-regrouping.md](../02-notes/implemented/architecture/2026-07-29-package-regrouping.md)
- Pinned source: [.agents/notes/implemented/architecture/2026-07-29-package-regrouping.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/architecture/2026-07-29-package-regrouping.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `packages`. The source note names this file directly. | `exact-code-occurrence, named-directory-member, named-file` |
| [`docs/module-graph.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/module-graph.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`packages/sdk/server/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |
| [`packages/skill/skill/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts) | package entry point | Core file in the package named by the note: `packages/skill/skill`. Defines `skill`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/core/session/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/index.ts) | package entry point | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/core/session/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/core/session/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/core/session`. | `named-package-member` |
| [`packages/sdk/protocol/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/index.ts) | package entry point | Core file in the package named by the note: `packages/sdk/protocol`. | `named-package-member` |
| [`packages/sdk/protocol/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/protocol/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/sdk/protocol`. | `named-package-member` |
| [`packages/boot/app-boot/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts) | package entry point | Core file in the package named by the note: `packages/boot/app-boot`. | `named-package-member` |
| [`packages/plan/plan-mode/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/index.ts) | package entry point | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/plan/plan-mode/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/plan/plan-mode/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/plan/plan-mode`. | `named-package-member` |
| [`packages/sdk/server/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/sdk/server/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/sdk/server`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `web` | `const` | [`apps/cli/src/args.ts:156`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L156) | `const web = program.command('web').description('boot the web profile (alias of --profile web); the web app\'s own flags follow')` |
| `json` | `const` | [`packages/client/modules/src/index.ts:169`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L169) | `const json = JSON.stringify(graph).replaceAll('<', '\\u003c')` |
| `references` | `const` | [`packages/context/session-reference/src/uri.ts:69`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/context/session-reference/src/uri.ts#L69) | `const references: SessionReferenceInput[] = []` |
| `sandbox` | `const` | [`packages/e2b/e2b/src/index.ts:132`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/e2b/e2b/src/index.ts#L132) | `const sandbox = await this.ready` |
| `permission` | `const` | [`packages/hooks/hook-protocol/src/codec.ts:125`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hook-protocol/src/codec.ts#L125) | `const permission = permissionDecisionOf(str(hso, 'permissionDecision'))` |
| `commands` | `const` | [`packages/hooks/hooks-codex/src/config.ts:60`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/hooks/hooks-codex/src/config.ts#L60) | `const commands: MatcherGroup['hooks'] = []` |
| `skill` | `const` | [`packages/skill/skill/src/index.ts:574`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill/src/index.ts#L574) | `const skill = entry.candidate` |

### Tests and executable evidence

- [`apps/web/tests/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.md) — A test under the owning area exercises or imports `tsdown`.
- [`apps/web/tests/scaffold.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/scaffold.ts) — A test under the owning area exercises or imports `app-boot`. A test under the owning area exercises or imports `tool-cordis`.
- [`apps/web/tests/README.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/README.zh.md) — A test under the owning area exercises or imports `tsdown`.
- [`apps/web/tests/steering.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/steering.e2e.ts) — A test under the owning area exercises or imports `ask_user_question`. A test under the owning area exercises or imports `dsh-session`.
- [`apps/web/tests/hmr-live.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/hmr-live.e2e.ts) — A test under the owning area exercises or imports `subprocess`.
- [`apps/web/tests/assembled-boot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/assembled-boot.ts) — A test under the owning area exercises or imports `session-query`.
- [`apps/web/tests/smoke-real.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/smoke-real.e2e.ts) — A test under the owning area exercises or imports `session-query`.
- [`apps/web/tests/default-model.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/web/tests/default-model.e2e.ts) — A test under the owning area exercises or imports `dsh-session`.

## How to read the implementation

1. Start with [`packages/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/README.md) because it has the strongest evidence link to the note.
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

- Tags: `class/architecture`, `concern/boundary`, `concern/cancellation-timeout`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/performance`, `concern/simplification`, `concern/trust`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/multi-agent`, `domain/observability`, `domain/protocols`, `domain/security`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`, `mechanism/registry`
- Aliases: `web`, `json`, `references`, `sandbox`, `permission`, `commands`, `skill`, `packages/<group>/<pkg>`, `ui/`, `dsh-sdk-protocol`, `user-questions`, `user-approval`, `tool-ask-user`, `app-boot`
- Regex: `(?i)(json|references|sandbox|permission|commands|skill|packages/<group>/<pkg>|dsh\-sdk\-protocol)`

```bash
rg -n --pcre2 "(?i)(json|references|sandbox|permission|commands|skill|packages/<group>/<pkg>|dsh\\-sdk\\-protocol)" source-deepseek-harness
rg -l --fixed-strings "class/architecture" 03-agent-context
```

## Connected notes

- **`source-link`** — [0547. Reorganize packages into a modular hierarchy](0547-reorganize-packages-into-a-modular-hierarchy.md): The source note links to this decision directly.
- **`source-link`** — [0126. Repository naming contract and pre-release rename ledger](0126-repository-naming-contract-and-pre-release-rename-ledger.md): The source note links to this decision directly.
- **`source-link`** — [0490. Remove the SDK project toolchain](0490-remove-the-sdk-project-toolchain.md): The source note links to this decision directly.
- **`shares-code-with`** — [0016. Bounded recovery for transient LLM request failures](0016-bounded-recovery-for-transient-llm-request-failures.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0063. Web input state machine, composer slots, and the slash pipeline (ui-conversation input / ui-input-trigger)](0063-web-input-state-machine-composer-slots-and-the-slash-pipeline-ui-convers.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0035. Agent-scope runtime design and correctness](0035-agent-scope-runtime-design-and-correctness.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.
- **`shares-code-with`** — [0516. Migrate simple unary API Proxy calls to business Remote services](0516-migrate-simple-unary-api-proxy-calls-to-business-remote-services.md): Shares source implementation: `packages/core/session/src/index.ts`, `packages/core/session/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0075-regroup-packages-by-measured-clustering.md`.
