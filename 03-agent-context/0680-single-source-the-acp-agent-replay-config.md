---
id: "dsh-note-0680"
title: "Single-source the acp-agent replay config"
status: "archived"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/testing/2026-07-04-single-source-acp-replay-config.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/protocols"
  - "domain/session-state"
  - "domain/storage"
  - "lifecycle/archived"
  - "mechanism/adapter"
  - "mechanism/event-sourcing"
  - "mechanism/policy"
  - "mechanism/streaming"
aliases:
  - "patches"
  - "hmr"
  - "refresh"
  - "examples/acp-agent"
  - "cordis.yml"
  - "cordis.snapshot.yml"
  - "llm-deepseek"
  - "llm-replay"
  - "internal/update"
  - "Single-source the acp-agent replay config"
  - "testing"
  - "boundary"
  - "evidence"
  - "lifecycle"
search_regex: "(?i)(patches|refresh|examples/acp\\-agent|cordis\\.yml|cordis\\.snapshot\\.yml|llm\\-deepseek|llm\\-replay|internal/update)"
---

# 0680. Single-source the acp-agent replay config — implementation context

## Open this when

examples/acp-agent shipped two hand-maintained configs: cordis.yml (the live tree) and a cordis.snapshot.yml that mirrored it entry-for-entry with only the llm backend swapped --- stripped of comments, the entire difference was the eight-line llm-deepseek stanza versus the two-line llm-replay stanza. Every app-shape change had to be made twice, and nothing gated the symmetry: if the copies drifted, the snapshot tier would silently exercise a different app than the one that ships --- the "green units, broken product" class of gap the snapshot tier exists to close, reintroduced one level up, with reviewer.

## Source decision

cordis.snapshot.yml includes the live config, disables the named DeepSeek adapter by id and name, and inserts the replay adapter. Every other entry therefore comes from the shipping tree. Replay selects the overlay; recording still boots cordis.yml, and the load guard permits the intentionally disabled entry. One vendored-plugin fact the overlay depends on, deliberately: the include applies patches when it loads the file --- its refresh()/internal/update paths re-read without re-patching --- which is exactly enough for a one-shot replay boot (the replay app loads no hmr and nothing rewrites the config mid-run).

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/testing/2026-07-04-single-source-acp-replay-config.md](../02-notes/archived/testing/2026-07-04-single-source-acp-replay-config.md)
- Pinned source: [.agents/notes/archived/testing/2026-07-04-single-source-acp-replay-config.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/testing/2026-07-04-single-source-acp-replay-config.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/postmortem/0001-acp-default-export-drops-inject.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0001-acp-default-export-drops-inject.md) | package contract and examples | The source note names this file directly. Contains the exact code literal `examples/acp-agent` named by the note. | `exact-code-occurrence, named-file` |
| [`vendor/hmr/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/hmr/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/hmr`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/index.ts) | package entry point | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/types.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/types.ts) | public types and contract | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/llm/llm-deepseek/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/llm/llm-deepseek/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/llm/llm-deepseek`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/index.ts) | package entry point | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`packages/test-support/llm-replay/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/test-support/llm-replay`. | `named-package-member` |
| [`examples/acp-agent/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `examples/acp-agent`. | `named-directory-member` |
| [`examples/acp-agent/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `examples/acp-agent`. | `named-directory-member` |
| [`examples/acp-agent/pty-snapshot-backend.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/pty-snapshot-backend.mjs) | runtime implementation | Entry point or contract under the directory named by the note: `examples/acp-agent`. | `named-directory-member` |
| [`examples/acp-agent/web-fetch-fixture-server.mjs`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/web-fetch-fixture-server.mjs) | runtime implementation | Entry point or contract under the directory named by the note: `examples/acp-agent`. | `named-directory-member` |
| [`examples/acp-agent`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent) | package or module directory | The source note names this implementation area directly. | `named-directory` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `patches` | `const` | [`apps/cli/src/args.ts:84`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/src/args.ts#L84) | `const patches = options.patch ?? []` |
| `hmr` | `const` | [`packages/boot/app-boot/src/index.ts:237`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/boot/app-boot/src/index.ts#L237) | `const hmr = ctx.get('hmr')` |
| `refresh` | `const` | [`packages/client/ui-agent-preset/src/client/index.ts:77`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-agent-preset/src/client/index.ts#L77) | `const refresh = (): void => {` |

### Tests and executable evidence

- [`apps/cli/tests/args.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/args.spec.ts) — A test under the owning area exercises or imports `patches`.
- [`apps/cli/tests/windows-shell.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/windows-shell.spec.ts) — A test under the owning area exercises or imports `patches`.
- [`apps/cli/tests/web-agent-presets.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/web-agent-presets.e2e.ts) — A test under the owning area exercises or imports `patches`.
- [`apps/cli/tests/memory-mcp-configs.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/memory-mcp-configs.spec.ts) — A test under the owning area exercises or imports `patches`.
- [`apps/cli/tests/lazy-search-startup.compat.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/lazy-search-startup.compat.spec.ts) — A test under the owning area exercises or imports `patches`.
- [`packages/test-support/llm-replay/tests/llm-replay.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/test-support/llm-replay/tests/llm-replay.spec.ts) — A test under the owning area exercises or imports `llm-replay`.
- [`examples/acp-agent/tests/acp.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/examples/acp-agent/tests/acp.e2e.ts) — Contains the exact code literal `examples/acp-agent` named by the note.

## How to read the implementation

1. Start with [`docs/postmortem/0001-acp-default-export-drops-inject.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/postmortem/0001-acp-default-export-drops-inject.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** repository tests and engineering policy.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/evidence`, `concern/lifecycle`, `concern/recovery`, `concern/simplification`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/protocols`, `domain/session-state`, `domain/storage`, `lifecycle/archived`, `mechanism/adapter`, `mechanism/event-sourcing`, `mechanism/policy`, `mechanism/streaming`
- Aliases: `patches`, `hmr`, `refresh`, `examples/acp-agent`, `cordis.yml`, `cordis.snapshot.yml`, `llm-deepseek`, `llm-replay`, `internal/update`, `Single-source the acp-agent replay config`, `testing`, `boundary`, `evidence`, `lifecycle`
- Regex: `(?i)(patches|refresh|examples/acp\-agent|cordis\.yml|cordis\.snapshot\.yml|llm\-deepseek|llm\-replay|internal/update)`

```bash
rg -n --pcre2 "(?i)(patches|refresh|examples/acp\\-agent|cordis\\.yml|cordis\\.snapshot\\.yml|llm\\-deepseek|llm\\-replay|internal/update)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0495. ACP snapshot tests --- record-once / replay-deterministic](0495-acp-snapshot-tests-record-once-replay-deterministic.md): Shares source implementation: `docs/postmortem/0001-acp-default-export-drops-inject.md`, `packages/llm/llm-deepseek/src/index.ts`.
- **`shares-code-with`** — [0213. official DeepSeek first-run credential setup](0213-official-deepseek-first-run-credential-setup.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0501. Scriptable LLM wire fault server](0501-scriptable-llm-wire-fault-server.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0371. First-run readiness reads every provider, and the setup card closes](0371-first-run-readiness-reads-every-provider-and-the-setup-card-closes.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.
- **`shares-code-with`** — [0291. DeepSeek request user and session identity headers](0291-deepseek-request-user-and-session-identity-headers.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/invariant.ts`.
- **`shares-code-with`** — [0128. A pi-ai model declares its own input modalities, and undeclared means text](0128-a-pi-ai-model-declares-its-own-input-modalities-and-undeclared-means-tex.md): Shares source implementation: `packages/test-support/llm-replay/src/index.ts`, `packages/test-support/llm-replay/src/invariant.ts`.
- **`shares-code-with`** — [0094. pi-ai routes are declared providers, not catalog lookups](0094-pi-ai-routes-are-declared-providers-not-catalog-lookups.md): Shares source implementation: `packages/llm/llm-deepseek/src/index.ts`, `packages/llm/llm-deepseek/src/types.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0680-single-source-the-acp-agent-replay-config.md`.
