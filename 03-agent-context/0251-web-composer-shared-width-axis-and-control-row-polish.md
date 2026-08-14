---
id: "dsh-note-0251"
title: "Web composer shared width axis and control-row polish"
status: "implemented"
class: "feature"
source_commit: "47f943859bef60e4160492346772ded9b24f765a"
source_note: "../02-notes/implemented/feature/2026-08-04-web-composer-shared-width-axis.md"
implementation_evidence: "lead-only"
target_anchor: "sandbox capability boundary"
tags:
  - "class/feature"
  - "concern/concurrency"
  - "concern/evidence"
  - "concern/human-control"
  - "concern/lifecycle"
  - "concern/ownership"
  - "concern/simplification"
  - "concern/trust"
  - "domain/security"
  - "lifecycle/implemented"
aliases:
  - "--dsh-chat-content-width"
  - ".root"
  - "--dsh-composer-card-max-width"
  - "calc(var(--dsh-composer-side-clearance) + 16px)"
  - "container-type: inline-size"
  - "container-name"
  - "max-width: min"
  - ".composerStack"
  - "Web composer shared width axis and control-row polish"
  - "feature"
  - "concurrency"
  - "evidence"
  - "human control"
  - "lifecycle"
search_regex: "(?i)(\\-\\-dsh\\-chat\\-content\\-width|\\.root|\\-\\-dsh\\-composer\\-card\\-max\\-width|calc\\(var\\(\\-\\-dsh\\-composer\\-side\\-clearance\\)[- ]\\+[- ]16px\\)|container\\-type:[- ]inline\\-size|container\\-name|max\\-width:[- ]min|\\.composerStack)"
---

# 0251. Web composer shared width axis and control-row polish — implementation context

## Open this when

The web conversation column sized each surface independently: the transcript column, the input card, the todo/goal/queue dock cards, and the ask-question/approval/plan-review takeover cards each carried their own hardcoded max-width (736/752/776/800px variants) and their own side paddings. The surfaces drifted a few pixels apart at full width and diverged further on narrow viewports, where some panels kept clearance from the screen edge and others went flush.

## Source decision

One content width variable owns the whole column. --dsh-chat-content-width (748px) is declared on ConversationRoot's .root --- the transcript and the composer seat are sibling subtrees, so the declaration must sit on their common ancestor for CSS custom-property inheritance to reach both. Every other geometry derives from it: the input card caps at content + 32px (--dsh-composer-card-max-width), the dock cards subtract four dock insets (4 × 8px) from the card and land back on the content width, and the takeover cards use the content width directly.

## Decision status

Shipped at the pinned commit. Treat the listed code as evidence of one working design, not as a command to copy its TypeScript shape.

- Raw note: [implemented/feature/2026-08-04-web-composer-shared-width-axis.md](../02-notes/implemented/feature/2026-08-04-web-composer-shared-width-axis.md)
- Pinned source: [.agents/notes/implemented/feature/2026-08-04-web-composer-shared-width-axis.md](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/.agents/notes/implemented/feature/2026-08-04-web-composer-shared-width-axis.md)
- Commit: `47f943859bef60e4160492346772ded9b24f765a`

## Source implementation map

No implementation path could be confirmed from the pinned tree. Use the search handles below; do not invent a source location.

### Code constructs named by the note

The note names behavior or policy but no uniquely resolvable code declaration. Search by its aliases and inspect the listed package entry points.

### Tests and executable evidence

- [`packages/client/ui-sidebar/tests/sidebar-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-sidebar/tests/sidebar-styles.client.spec.ts) — A test under the owning area exercises or imports `calc`.
- [`packages/client/ui-workspace/tests/browser-styles.client.spec.ts`](https://github.com/deepseek-ai/deepseek-harness/blob/47f943859bef60e4160492346772ded9b24f765a/packages/client/ui-workspace/tests/browser-styles.client.spec.ts) — A test under the owning area exercises or imports `calc`.

## How to read the implementation

1. Start from the source note and run the regex below across the pinned repository.
2. Read public types and entry points before implementation details. Write down the contract in plain language.
3. Follow the named constructs into their callers and tests. Confirm which component can actually enforce the rule.
4. Reconstruct one failure path: cancellation, crash, retry, stale state, partial write, or incompatible provider input—whichever the note is protecting against.
5. Only then map the contract into the current Rust repository. Target paths must come from that repository, never from guesswork in this wiki.

## Rust harness port

**Likely integration point:** sandbox capability boundary.

Preserve the behavior and ownership rule. Do not mechanically translate Cordis, TypeScript declaration merging, Node streams, or package topology into Rust.

Relevant standing rules:

- **`rust/capability-security`** — Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.
- **`rust/four-tool-surface`** — Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.

### Smallest safe implementation slice

1. Express the semantic contract as Rust types and one owning boundary.
2. Add the smallest adapter or service needed by the existing loop; do not add a new model-facing tool unless the four-tool surface cannot express the job.
3. Record durable evidence before deriving summaries or UI state.
4. Add one happy-path test, one interruption test, and one replay/restart test where state survives a turn.
5. Keep the patch reversible until the source note's strongest failure case is proven in the Rust harness.

## Search handles

- Tags: `class/feature`, `concern/concurrency`, `concern/evidence`, `concern/human-control`, `concern/lifecycle`, `concern/ownership`, `concern/simplification`, `concern/trust`, `domain/security`, `lifecycle/implemented`
- Aliases: `--dsh-chat-content-width`, `.root`, `--dsh-composer-card-max-width`, `calc(var(--dsh-composer-side-clearance) + 16px)`, `container-type: inline-size`, `container-name`, `max-width: min`, `.composerStack`, `Web composer shared width axis and control-row polish`, `feature`, `concurrency`, `evidence`, `human control`, `lifecycle`
- Regex: `(?i)(\-\-dsh\-chat\-content\-width|\.root|\-\-dsh\-composer\-card\-max\-width|calc\(var\(\-\-dsh\-composer\-side\-clearance\)[- ]\+[- ]16px\)|container\-type:[- ]inline\-size|container\-name|max\-width:[- ]min|\.composerStack)`

```bash
rg -n --pcre2 "(?i)(\\-\\-dsh\\-chat\\-content\\-width|\\.root|\\-\\-dsh\\-composer\\-card\\-max\\-width|calc\\(var\\(\\-\\-dsh\\-composer\\-side\\-clearance\\)[- ]\\+[- ]16px\\)|container\\-type:[- ]inline\\-size|container\\-name|max\\-width:[- ]min|\\.composerStack)" source-deepseek-harness
rg -l --fixed-strings "class/feature" 03-agent-context
```

## Connected notes

- **`same-design-pressure`** — [0467. One shared base config with per-surface overlays](0467-one-shared-base-config-with-per-surface-overlays.md): Shares design concerns: `concern/concurrency`, `concern/evidence`, `concern/human-control`.
- **`same-design-pressure`** — [0295. Web `/export` shares the streamed Session ZIP download](0295-web-export-shares-the-streamed-session-zip-download.md): Shares design concerns: `concern/concurrency`, `concern/evidence`, `concern/human-control`.
- **`same-design-pressure`** — [0314. Web GUI changes close the loop on the existing URL](0314-web-gui-changes-close-the-loop-on-the-existing-url.md): Shares design concerns: `concern/evidence`, `concern/human-control`, `concern/lifecycle`.
- **`same-design-pressure`** — [0175. Web multimodal image input and durable attachments](0175-web-multimodal-image-input-and-durable-attachments.md): Shares design concerns: `concern/concurrency`, `concern/evidence`, `concern/human-control`.
- **`same-design-pressure`** — [0338. Bounded, escalating signal shutdown for Web and headless](0338-bounded-escalating-signal-shutdown-for-web-and-headless.md): Shares design concerns: `concern/concurrency`, `concern/evidence`, `concern/human-control`.
- **`same-design-pressure`** — [0500. Keyless browser e2e lane for the web GUI](0500-keyless-browser-e2e-lane-for-the-web-gui.md): Shares design concerns: `concern/concurrency`, `concern/evidence`, `concern/human-control`.
- **`same-design-pressure`** — [0605. Web composer stats detail and input-zone polish](0605-web-composer-stats-detail-and-input-zone-polish.md): Shares design concerns: `concern/concurrency`, `concern/evidence`, `concern/lifecycle`.
- **`same-design-pressure`** — [0162. Persisted same-session goal domain](0162-persisted-same-session-goal-domain.md): Shares design concerns: `concern/concurrency`, `concern/evidence`, `concern/human-control`.

## Agent handoff

> Read the raw note, then the strongest implementation paths above at the pinned commit. Inspect the current Rust repository before naming target files. State the pressure, owner, contract, failure schedule, and evidence. Preserve the four model tools (`read`, `write`, `patch`, `exec`), lossless session evidence, Tokio-owned concurrency, explicit QuickJS capabilities, and fail-closed sandboxing. Propose the smallest reversible patch and tests; do not copy a proposed, rejected, or archived mechanism as if it shipped.

---

Generated from the pinned source note and repository tree. The source note remains authority. Context file: `0251-web-composer-shared-width-axis-and-control-row-polish.md`.
