---
id: "dsh-note-0502"
title: "Keep browser storage owned by jsdom in Vitest"
status: "implemented"
class: "testing"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/testing/2026-07-30-vitest-jsdom-webstorage-ownership.md"
implementation_evidence: "medium"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/testing"
  - "concern/boundary"
  - "concern/compatibility"
  - "concern/evidence"
  - "concern/ownership"
  - "domain/build-release"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "domain/web-retrieval"
  - "lifecycle/implemented"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "Storage"
  - "globalThis.localStorage"
  - "localStorage"
  - "--localstorage-file"
  - "--webstorage"
  - "--no-webstorage"
  - "execArgv"
  - "@vitest-environment jsdom"
  - "NODE_OPTIONS=--no-webstorage"
  - "Keep browser storage owned by jsdom in Vitest"
  - "testing"
  - "boundary"
  - "compatibility"
  - "evidence"
search_regex: "(?i)(Storage|globalThis\\.localStorage|localStorage|\\-\\-localstorage\\-file|\\-\\-webstorage|\\-\\-no\\-webstorage|execArgv|@vitest\\-environment[- ]jsdom)"
---

# 0502. Keep browser storage owned by jsdom in Vitest — implementation context

## Open this when

The supported Node range includes releases that reserve a process-wide globalThis.localStorage. Node 26 exposes that property as undefined without --localstorage-file; Vitest sees the reserved key and does not project jsdom's isolated Storage object over it. Component suites then fail before exercising product behavior, while the primary Node 24 coverage lane remains green because that runtime does not reserve the key by default.

## Source decision

Vitest workers disable Node's process-wide Web Storage when the runtime advertises the --webstorage flag. The configuration passes --no-webstorage through each test project's execArgv; runtimes without that flag receive no argument. Node-environment suites therefore stay browser-free, and files selecting jsdom through @vitest-environment jsdom receive jsdom's isolated localStorage. The Node compatibility aggregate runs a dedicated jsdom smoke on every advertised compatibility line.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/testing/2026-07-30-vitest-jsdom-webstorage-ownership.md](../02-notes/implemented/testing/2026-07-30-vitest-jsdom-webstorage-ownership.md)
- Pinned source: [.agents/notes/implemented/testing/2026-07-30-vitest-jsdom-webstorage-ownership.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/testing/2026-07-30-vitest-jsdom-webstorage-ownership.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`packages/storage/storage/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/index.ts) | package entry point | Core file in the package named by the note: `packages/storage/storage`. Defines `Storage`, a construct named by the note. | `named-package-member, symbol-definition` |
| [`packages/storage/storage/src/invariant.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/invariant.ts) | runtime contract checks | Core file in the package named by the note: `packages/storage/storage`. | `named-package-member` |
| [`packages/storage/storage`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage) | package or module directory | The note names this package or capability. | `named-package` |
| [`packages/storage/storage/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/README.md) | package contract and examples | Core file in the package named by the note: `packages/storage/storage`. | `named-package-member` |
| [`packages/storage/storage/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/package.json) | composition and configuration | Core file in the package named by the note: `packages/storage/storage`. | `named-package-member` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `Storage` | `class` | [`packages/storage/storage/src/index.ts:47`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/index.ts#L47) | `export class Storage extends Service {` |

### Tests and executable evidence

- [`packages/storage/storage/tests/registry.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/tests/registry.spec.ts) — A test under the owning area exercises or imports `Storage`.

## How to read the implementation

1. Start with [`packages/storage/storage/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/storage/storage/src/index.ts) because it has the strongest evidence link to the note.
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
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/testing`, `concern/boundary`, `concern/compatibility`, `concern/evidence`, `concern/ownership`, `domain/build-release`, `domain/session-state`, `domain/storage`, `domain/testing`, `domain/web-retrieval`, `lifecycle/implemented`, `mechanism/policy`, `mechanism/projection`
- Aliases: `Storage`, `globalThis.localStorage`, `localStorage`, `--localstorage-file`, `--webstorage`, `--no-webstorage`, `execArgv`, `@vitest-environment jsdom`, `NODE_OPTIONS=--no-webstorage`, `Keep browser storage owned by jsdom in Vitest`, `testing`, `boundary`, `compatibility`, `evidence`
- Regex: `(?i)(Storage|globalThis\.localStorage|localStorage|\-\-localstorage\-file|\-\-webstorage|\-\-no\-webstorage|execArgv|@vitest\-environment[- ]jsdom)`

```bash
rg -n --pcre2 "(?i)(Storage|globalThis\\.localStorage|localStorage|\\-\\-localstorage\\-file|\\-\\-webstorage|\\-\\-no\\-webstorage|execArgv|@vitest\\-environment[- ]jsdom)" source-deepseek-harness
rg -l --fixed-strings "class/testing" 03-agent-context
```

## Connected notes

- **`shares-code-with`** — [0509. Domain KV storage capability seam and the workspace entity](0509-domain-kv-storage-capability-seam-and-the-workspace-entity.md): Shares source implementation: `packages/storage/storage/src/index.ts`, `packages/storage/storage/src/invariant.ts`.
- **`shares-code-with`** — [0383. Subsystems catalog and the `ts type-equiv` drift gate](0383-subsystems-catalog-and-the-ts-type-equiv-drift-gate.md): Shares source implementation: `packages/storage/storage/src/index.ts`, `packages/storage/storage/src/invariant.ts`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0503. Required CI gate for web browser expected outputs](0503-required-ci-gate-for-web-browser-expected-outputs.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0512. Storage root placement and derived-medium recovery](0512-storage-root-placement-and-derived-medium-recovery.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0326. The browser conversation is a log-ordered human transcript](0326-the-browser-conversation-is-a-log-ordered-human-transcript.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0511. Session projections and command lifecycle logging](0511-session-projections-and-command-lifecycle-logging.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.
- **`same-design-pressure`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares design concerns: `concern/boundary`, `concern/compatibility`, `concern/evidence`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0502-keep-browser-storage-owned-by-jsdom-in-vitest.md`.
