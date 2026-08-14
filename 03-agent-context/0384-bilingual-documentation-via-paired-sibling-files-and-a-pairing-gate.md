---
id: "dsh-note-0384"
title: "Bilingual documentation via paired sibling files and a pairing gate"
status: "implemented"
class: "process"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/process/2026-07-02-bilingual-docs-and-pairing-gate.md"
implementation_evidence: "high"
target_anchor: "repository tests and engineering policy"
tags:
  - "class/process"
  - "concern/boundary"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/ownership"
  - "concern/performance"
  - "concern/schema-types"
  - "domain/build-release"
  - "domain/configuration"
  - "domain/extensions"
  - "domain/llm"
  - "domain/session-state"
  - "domain/storage"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/adapter"
  - "mechanism/policy"
  - "mechanism/projection"
aliases:
  - "yaml"
  - "docs"
  - "pairedPages"
  - "foo.md"
  - "foo.zh.md"
  - "foo.i18n.yaml"
  - "verify-translation-pairing --write <pair>"
  - "--write --all"
  - "verify-translation-pairing"
  - "doc-sync"
  - ".zh.md"
  - "/en/"
  - ".md"
  - ".cordis.yml"
search_regex: "(?i)(yaml|docs|pairedPages|foo\\.md|foo\\.zh\\.md|foo\\.i18n\\.yaml|verify\\-translation\\-pairing[- ]\\-\\-write[- ]<pair>|\\-\\-write[- ]\\-\\-all)"
---

# 0384. Bilingual documentation via paired sibling files and a pairing gate — implementation context

## Open this when

This repo's documentation corpus is read by people and agents inside and outside the company, in both English and Chinese. Maintaining a second language by hand, with no mechanism, is how translations rot: one side moves on, the other silently lies, and no gate notices. The repo's standing answer to invariants of this kind is to encode them as a mechanical check (see quality gates and doc-sync enforcement), so the bilingual policy ships with one.

## Source decision

Paired sibling files with equal authority. A documentation pair is three sibling files: English foo.md, Chinese foo.zh.md, and a consistency record foo.i18n.yaml. Neither language is canonical --- a document may be authored and reviewed Chinese-first and translated to English afterwards, or the reverse; what binds the pair is that both sides must say the same thing, and pairs merge whole (both languages plus the record, never one alone). Policy: docs/i18n/README.md; translation rules: docs/i18n/translation-rules.md; terminology source of truth: docs/i18n/terminology.md.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/process/2026-07-02-bilingual-docs-and-pairing-gate.md](../02-notes/implemented/process/2026-07-02-bilingual-docs-and-pairing-gate.md)
- Pinned source: [.agents/notes/implemented/process/2026-07-02-bilingual-docs-and-pairing-gate.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/process/2026-07-02-bilingual-docs-and-pairing-gate.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/i18n/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/README.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/i18n/terminology.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/terminology.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/i18n/translation-rules.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/translation-rules.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`scripts/verify-translation-pairing.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/verify-translation-pairing.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/translation-prompt.snapshot.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-prompt.snapshot.ts) | repository automation | The source note names this file directly. | `named-file` |
| [`scripts/translation-pairing.manifest.json`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/translation-pairing.manifest.json) | repository automation | The source note names this file directly. | `named-file` |
| [`.agents/skills/dsh-translate-docs/SKILL.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/skills/dsh-translate-docs/SKILL.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`vendor/cordis/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis/src/index.ts) | package entry point | Core file in the package named by the note: `vendor/cordis`. | `named-package-member` |
| [`vendor/cordis`](https://github.com/deepseek-ai/deepseek-harness/tree/47f943859bef60e4160492346772ded9b24f765a/vendor/cordis) | package or module directory | The note names this package or capability. | `named-package` |
| [`website/docs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts) | runtime implementation | Defines `pairedPages`, a construct named by the note. | `symbol-definition` |
| [`scripts/gen-doc-graphs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts) | repository automation | Defines `docs`, a construct named by the note. | `symbol-definition` |
| [`packages/skill/skill-filesystem/src/index.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts) | package entry point | Defines `yaml`, a construct named by the note. | `symbol-definition` |

### Code constructs named by the note

| Construct | Kind | Definition | Source signature |
|---|---|---|---|
| `yaml` | `const` | [`packages/skill/skill-filesystem/src/index.ts:917`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/skill/skill-filesystem/src/index.ts#L917) | `const yaml = raw.slice(start, closing.start)` |
| `docs` | `const` | [`scripts/gen-doc-graphs.ts:1378`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/gen-doc-graphs.ts#L1378) | `const docs: GraphDoc[] = [` |
| `pairedPages` | `function` | [`website/docs.ts:91`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts#L91) | `function pairedPages(pages: PairedPage[]): DocsPage[] {` |

### Tests and executable evidence

- [`scripts/project-doc-site.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/project-doc-site.spec.ts) — The source note names this file directly.
- [`scripts/cordis-config-files.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/scripts/cordis-config-files.spec.ts) — The source note names this file directly.
- Source verification intent: The verification contract covers each boundary independently. verify-translation-pairing pins pair completeness, hashes, switchers, and structure; project-doc-site.spec.ts pins locale-specific source selection for published pairs; cordis-config-files.spec.ts pins discovery of Loader YAML and exclusion of translation records; and the translation-prompt runnable snapshot pins the rendered system message, five reviewed example pairs, source request, and consumed response. Together these checks make pair drift, publication drift, configuration misclassification, and model-visible prompt drift review-visible.

## How to read the implementation

1. Start with [`docs/i18n/README.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/i18n/README.md) because it has the strongest evidence link to the note.
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
- **`rust/newtype-ids`** — Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.
- **`rust/process-discipline`** — Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
- **`rust/provider-adapter`** — Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.
- **`rust/quickjs-boundary`** — QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.
- **`rust/serde-validation`** — Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.
- **`rust/tagged-enums`** — Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.
- **`rust/trait-seam`** — Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/process`, `concern/boundary`, `concern/discovery-routing`, `concern/evidence`, `concern/human-control`, `concern/ownership`, `concern/performance`, `concern/schema-types`, `domain/build-release`, `domain/configuration`, `domain/extensions`, `domain/llm`, `domain/session-state`, `domain/storage`, `domain/testing`, `lifecycle/implemented`, `mechanism/adapter`, `mechanism/policy`, `mechanism/projection`
- Aliases: `yaml`, `docs`, `pairedPages`, `foo.md`, `foo.zh.md`, `foo.i18n.yaml`, `verify-translation-pairing --write <pair>`, `--write --all`, `verify-translation-pairing`, `doc-sync`, `.zh.md`, `/en/`, `.md`, `.cordis.yml`
- Regex: `(?i)(yaml|docs|pairedPages|foo\.md|foo\.zh\.md|foo\.i18n\.yaml|verify\-translation\-pairing[- ]\-\-write[- ]<pair>|\-\-write[- ]\-\-all)`

```bash
rg -n --pcre2 "(?i)(yaml|docs|pairedPages|foo\\.md|foo\\.zh\\.md|foo\\.i18n\\.yaml|verify\\-translation\\-pairing[- ]\\-\\-write[- ]<pair>|\\-\\-write[- ]\\-\\-all)" source-deepseek-harness
rg -l --fixed-strings "class/process" 03-agent-context
```

## Connected notes

- **`source-link`** — [0630. Doc-sync enforcement](0630-doc-sync-enforcement.md): The source note links to this decision directly.
- **`source-link`** — [0377. Mechanical quality gates over prose guidelines](0377-mechanical-quality-gates-over-prose-guidelines.md): The source note links to this decision directly.
- **`source-link`** — [0406. Briefed minimal translation updates](0406-briefed-minimal-translation-updates.md): The source note links to this decision directly.
- **`source-link`** — [0430. Lightweight routine documentation translation](0430-lightweight-routine-documentation-translation.md): The source note links to this decision directly.
- **`shares-code-with`** — [0514. Cordis Host/Client Dynamic Plugin Runtime](0514-cordis-host-client-dynamic-plugin-runtime.md): Shares source implementation: `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0131. Code Mode --- the model writes TypeScript against the tool registry](0131-code-mode-the-model-writes-typescript-against-the-tool-registry.md): Shares source implementation: `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0093. A session's agent is composed from a preset cordis.yml](0093-a-session-s-agent-is-composed-from-a-preset-cordis-yml.md): Shares source implementation: `vendor/cordis/src/index.ts`.
- **`shares-code-with`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): Shares source implementation: `scripts/project-doc-site.spec.ts`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0384-bilingual-documentation-via-paired-sibling-files-and-a-pairing-gate.md`.
