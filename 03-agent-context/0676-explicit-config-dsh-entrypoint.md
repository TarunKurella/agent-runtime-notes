---
id: "dsh-note-0676"
title: "Explicit-config dsh entrypoint"
status: "archived"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/archived/simplification/2026-08-03-explicit-config-dsh-entrypoint.md"
implementation_evidence: "medium"
target_anchor: "read/write/patch filesystem capability"
tags:
  - "class/simplification"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/recovery"
  - "concern/simplification"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/filesystem"
  - "domain/llm"
  - "domain/observability"
  - "domain/session-state"
  - "domain/shell-terminal"
  - "domain/storage"
  - "domain/testing"
  - "domain/ui-interaction"
  - "lifecycle/archived"
  - "mechanism/policy"
aliases:
  - "dsh"
  - "meta"
  - "--config"
  - "dsh --config <path>"
  - "apps/cli/config/base.cordis.yml"
  - "apps/cli/config/web.cordis.yml"
  - "$DSH_HOME/config.yaml"
  - "dsh --dump-default-config"
  - "dsh --config <path> --dump-config"
  - "--config-replace"
  - "dsh -p"
  - "bin/dsh"
  - "apps/cli"
  - "dsh --config"
search_regex: "(?i)(meta|\\-\\-config|dsh[- ]\\-\\-config[- ]<path>|apps/cli/config/base\\.cordis\\.yml|apps/cli/config/web\\.cordis\\.yml|\\$DSH_HOME/config\\.yaml|dsh[- ]\\-\\-dump\\-default\\-config|dsh[- ]\\-\\-config[- ]<path>[- ]\\-\\-dump\\-config)"
---

# 0676. Explicit-config dsh entrypoint — implementation context

## Open this when

Bare dsh selected a product TUI implicitly. That made one command own terminal lifecycle, session identity and resume handoff, onboarding, source-workspace shortcuts, guided upgrade sessions, personal config watching, and a large app-level PTY and transcript snapshot suite. The default also hid the actual composition boundary: --config was an optional third layer over a TUI overlay rather than the deployment definition a raw launcher needs. The shared base is intentionally neutral: it provides capabilities but creates no startup agent or interaction front door.

## Source decision

Raw executable use is dsh --config . The named file must be an Include patch list and is applied directly over apps/cli/config/base.cordis.yml at the same include level. It is required for boot, is not a complete replacement tree, and does not inherit apps/cli/config/web.cordis.yml or $DSH_HOME/config.yaml. Relative paths resolve from the invoking directory. Boot errors fail loud; SIGINT and SIGTERM dispose the root before exit. The raw diagnostic forms remain boot-free: dsh --dump-default-config prints the base, while dsh --config --dump-config prints base plus the required overlay.

## Decision status

Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

- Raw note: [archived/simplification/2026-08-03-explicit-config-dsh-entrypoint.md](../02-notes/archived/simplification/2026-08-03-explicit-config-dsh-entrypoint.md)
- Pinned source: [.agents/notes/archived/simplification/2026-08-03-explicit-config-dsh-entrypoint.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/archived/simplification/2026-08-03-explicit-config-dsh-entrypoint.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) | package contract and examples | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `named-directory-member, named-package-member` |
| [`apps/cli/package.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/package.json) | composition and configuration | Entry point or contract under the directory named by the note: `apps/cli`. Core file in the package named by the note: `apps/cli`. | `named-directory-member, named-package-member` |
| [`apps/cli`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/apps/cli) | package or module directory | The source note names this implementation area directly. The note names this package or capability. | `named-directory, named-package` |
| [`vendor/cordis/src/fiber.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts) | runtime implementation | Defines `meta`, a construct named by the note. | `symbol-definition` |
| [`packages/client/modules/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts) | package entry point | Defines `dsh`, a construct named by the note. | `symbol-definition` |
| [`knip.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/knip.json) | composition and configuration | Contains the exact code literal `apps/cli` named by the note. | `exact-code-occurrence` |
| [`pnpm-lock.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-lock.yaml) | composition and configuration | Contains the exact code literal `apps/cli` named by the note. | `exact-code-occurrence` |
| [`tsdown.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsdown.config.ts) | runtime implementation | Contains the exact code literal `apps/cli` named by the note. | `exact-code-occurrence` |
| [`tsconfig.host.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/tsconfig.host.json) | composition and configuration | Contains the exact code literal `apps/cli` named by the note. | `exact-code-occurrence` |
| [`docs/testing.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/testing.zh.md) | package contract and examples | Contains the exact code literal `apps/cli` named by the note. | `exact-code-occurrence` |
| [`pnpm-workspace.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/pnpm-workspace.yaml) | composition and configuration | Contains the exact code literal `apps/cli` named by the note. | `exact-code-occurrence` |
| [`scripts/verify-cordis-config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-cordis-config.ts) | repository automation | Contains the exact code literal `apps/cli` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `dsh` | `const` | [`packages/client/modules/src/index.ts:345`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/modules/src/index.ts#L345) | `const dsh = pkg.dsh` |
| `meta` | `const` | [`vendor/cordis/src/fiber.ts:444`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/fiber.ts#L444) | `const meta: EffectMeta = { label, children: [] }` |

### Tests and executable evidence

- [`apps/cli/tests/built-bin.e2e.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/tests/built-bin.e2e.ts) — A test under the owning area exercises or imports `upgrade`.
- [`vitest.e2e.config.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vitest.e2e.config.ts) — Contains the exact code literal `apps/cli` named by the note.
- Source verification intent: Parser tests require --config for raw boot and reject the removed command names and incompatible option combinations. Built-bin acceptance runs the published JavaScript entry without tsx, checks base-only and base-plus-overlay dumps, and drives an invalid raw provider overlay to prove a boot failure settles and exits rather than hanging. Source-launch compatibility checks the same required-config diagnostic through bin/dsh. No apps/cli TUI demo or test remains.

## How to read the implementation

1. Start with [`apps/cli/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/apps/cli/README.md) because it has the strongest evidence link to the note.
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
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/tokio-cancellation`** — Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.
- **Status gate** — Historical design. Use it to understand an older pressure or failure; locate the replacing decision before implementing anything.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `concern/recovery`, `concern/simplification`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/filesystem`, `domain/llm`, `domain/observability`, `domain/session-state`, `domain/shell-terminal`, `domain/storage`, `domain/testing`, `domain/ui-interaction`, `lifecycle/archived`, `mechanism/policy`
- Aliases: `dsh`, `meta`, `--config`, `dsh --config <path>`, `apps/cli/config/base.cordis.yml`, `apps/cli/config/web.cordis.yml`, `$DSH_HOME/config.yaml`, `dsh --dump-default-config`, `dsh --config <path> --dump-config`, `--config-replace`, `dsh -p`, `bin/dsh`, `apps/cli`, `dsh --config`
- Regex: `(?i)(meta|\-\-config|dsh[- ]\-\-config[- ]<path>|apps/cli/config/base\.cordis\.yml|apps/cli/config/web\.cordis\.yml|\$DSH_HOME/config\.yaml|dsh[- ]\-\-dump\-default\-config|dsh[- ]\-\-config[- ]<path>[- ]\-\-dump\-config)`

```bash
rg -n --pcre2 "(?i)(meta|\\-\\-config|dsh[- ]\\-\\-config[- ]<path>|apps/cli/config/base\\.cordis\\.yml|apps/cli/config/web\\.cordis\\.yml|\\$DSH_HOME/config\\.yaml|dsh[- ]\\-\\-dump\\-default\\-config|dsh[- ]\\-\\-config[- ]<path>[- ]\\-\\-dump\\-config)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0567. Dedicated full-screen TUI front door](0567-dedicated-full-screen-tui-front-door.md): The source note links to this decision directly.
- **`source-link`** — [0596. `dsh migrate`/`dsh upgrade` seed the first turn with a skill](0596-dsh-migrate-dsh-upgrade-seed-the-first-turn-with-a-skill.md): The source note links to this decision directly.
- **`source-link`** — [0597. `dsh meta` boots the TUI over the harness checkout](0597-dsh-meta-boots-the-tui-over-the-harness-checkout.md): The source note links to this decision directly.
- **`source-link`** — [0602. dsh --dump-config prints the composed config tree](0602-dsh-dump-config-prints-the-composed-config-tree.md): The source note links to this decision directly.
- **`shares-code-with`** — [0604. Versioned TUI first-run welcome](0604-versioned-tui-first-run-welcome.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0579. Product-level TUI session resume](0579-product-level-tui-session-resume.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0527. Artifact-first NPM baseline publication](0527-artifact-first-npm-baseline-publication.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.
- **`shares-code-with`** — [0402. Product-first root README](0402-product-first-root-readme.md): Shares source implementation: `apps/cli`, `apps/cli/README.md`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0676-explicit-config-dsh-entrypoint.md`.
