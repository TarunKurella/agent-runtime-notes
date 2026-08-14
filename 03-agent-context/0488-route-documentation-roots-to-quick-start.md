---
id: "dsh-note-0488"
title: "Route documentation roots to quick start"
status: "implemented"
class: "simplification"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/simplification/2026-08-11-quickstart-documentation-home.md"
implementation_evidence: "lead-only"
target_anchor: "append-only event and session store"
tags:
  - "class/simplification"
  - "concern/discovery-routing"
  - "concern/evidence"
  - "concern/lifecycle"
  - "concern/ownership"
  - "domain/session-state"
  - "domain/testing"
  - "lifecycle/implemented"
  - "mechanism/projection"
aliases:
  - ". The relative target preserves the configured"
  - "docs/user/index.md"
  - "docs/user/index.zh.md"
  - "/guide/quickstart"
  - "DOCS_BASE"
  - "Route documentation roots to quick start"
  - "simplification"
  - "discovery routing"
  - "evidence"
  - "lifecycle"
  - "ownership"
  - "session state"
  - "testing"
  - "implemented"
search_regex: "(?i)(\\.[- ]The[- ]relative[- ]target[- ]preserves[- ]the[- ]configured|docs/user/index\\.md|docs/user/index\\.zh\\.md|/guide/quickstart|DOCS_BASE|Route[- ]documentation[- ]roots[- ]to[- ]quick[- ]start|simplification|discovery[- ]routing)"
---

# 0488. Route documentation roots to quick start — implementation context

## Open this when

A separate documentation landing page duplicates product positioning and feature summaries owned by the product landing page. Those parallel claims require synchronization and review without helping readers reach technical instructions.

## Source decision

Each locale root is a redirect page. / sends readers to ./guide/quickstart, and /en/ resolves the same relative target to /en/guide/quickstart. The relative target preserves the configured DOCS_BASE when the site is hosted below an origin path. docs/user/index.md and docs/user/index.zh.md own the redirect as VitePress frontmatter. The documentation-site projector publishes only that frontmatter for locale homes, so the canonical Markdown retains its bilingual switcher without rendering a second landing page. The projector test verifies that both locale roots use the same locale-relative quick-start target.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/simplification/2026-08-11-quickstart-documentation-home.md](../02-notes/implemented/simplification/2026-08-11-quickstart-documentation-home.md)
- Pinned source: [.agents/notes/implemented/simplification/2026-08-11-quickstart-documentation-home.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/simplification/2026-08-11-quickstart-documentation-home.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

| Source path | Role | Why it is here | Evidence |
|---|---|---|---|
| [`docs/user/index.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/user/index.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`docs/user/index.zh.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/user/index.zh.md) | package contract and examples | The source note names this file directly. | `named-file` |
| [`website/docs.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/website/docs.ts) | runtime implementation | Contains the exact code literal `docs/user/index.md` named by the note. | `exact-code-occurrence` |
| [`docs/user/index.i18n.yaml`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/user/index.i18n.yaml) | composition and configuration | Contains the exact code literal `docs/user/index.md` named by the note. | `exact-code-occurrence` |

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- No test file was tied to this note with enough confidence. Read the note's verification section and search the owning package's `tests/` directory.

## How to read the implementation

1. Start with [`docs/user/index.md`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/docs/user/index.md) because it has the strongest evidence link to the note.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** append-only event and session store.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/append-log`** — Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/simplification`, `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`, `concern/ownership`, `domain/session-state`, `domain/testing`, `lifecycle/implemented`, `mechanism/projection`
- Aliases: `. The relative target preserves the configured`, `docs/user/index.md`, `docs/user/index.zh.md`, `/guide/quickstart`, `DOCS_BASE`, `Route documentation roots to quick start`, `simplification`, `discovery routing`, `evidence`, `lifecycle`, `ownership`, `session state`, `testing`, `implemented`
- Regex: `(?i)(\.[- ]The[- ]relative[- ]target[- ]preserves[- ]the[- ]configured|docs/user/index\.md|docs/user/index\.zh\.md|/guide/quickstart|DOCS_BASE|Route[- ]documentation[- ]roots[- ]to[- ]quick[- ]start|simplification|discovery[- ]routing)`

```bash
rg -n --pcre2 "(?i)(\\.[- ]The[- ]relative[- ]target[- ]preserves[- ]the[- ]configured|docs/user/index\\.md|docs/user/index\\.zh\\.md|/guide/quickstart|DOCS_BASE|Route[- ]documentation[- ]roots[- ]to[- ]quick[- ]start|simplification|discovery[- ]routing)" source-deepseek-harness
rg -l --fixed-strings "class/simplification" 03-agent-context
```

## Connected notes

- **`source-link`** — [0393. Project canonical documentation into the website](0393-project-canonical-documentation-into-the-website.md): The source note links to this decision directly.
- **`shares-code-with`** — [0641. Tutorial-style Cordis docs under docs/cordis-tutorial](0641-tutorial-style-cordis-docs-under-docs-cordis-tutorial.md): Shares source implementation: `website/docs.ts`.
- **`shares-code-with`** — [0442. Documentation-site navigation and repository chrome](0442-documentation-site-navigation-and-repository-chrome.md): Shares source implementation: `website/docs.ts`.
- **`same-design-pressure`** — [0425. The documentation site carries its own images](0425-the-documentation-site-carries-its-own-images.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/ownership`.
- **`same-design-pressure`** — [0384. Bilingual documentation via paired sibling files and a pairing gate](0384-bilingual-documentation-via-paired-sibling-files-and-a-pairing-gate.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/ownership`.
- **`same-design-pressure`** — [0496. Real-API e2e in CI against the external DeepSeek API](0496-real-api-e2e-in-ci-against-the-external-deepseek-api.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0467. One shared base config with per-surface overlays](0467-one-shared-base-config-with-per-surface-overlays.md): Shares design concerns: `concern/discovery-routing`, `concern/evidence`, `concern/lifecycle`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0488-route-documentation-roots-to-quick-start.md`.
