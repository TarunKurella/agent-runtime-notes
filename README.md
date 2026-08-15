# Agent Runtime Notes

A searchable engineering fieldbook for people building reliable software agents.

**For:** aspiring AI engineers, agent builders, harness builders, and systems engineers who want to understand why a design works—not merely what files exist.

## Why a summary is not enough

A normal summary keeps conclusions and drops the path that produced them. That is dangerous in harness work. Two designs can look identical on the happy path while behaving very differently under cancellation, retries, partial writes, provider quirks, or a full context window.

This repository keeps the engineering decision trail:

```text
pressure -> options -> decision -> code boundary -> failure case -> test evidence -> reusable lesson
```

That trail is not private model chain-of-thought. It is public engineering reasoning reconstructed from design notes, source paths, named code constructs, tests, and stated trade-offs.

## What makes this different

Each source note has a paired agent-context file that answers practical questions:

- What problem forced this design?
- What owns the behavior?
- Which source files and symbols carry it?
- What breaks when the assumption is wrong?
- Is this shipped, proposed, rejected, or historical?
- What should a Rust harness preserve without copying the TypeScript shape?
- Which nearby notes share code or face the same design pressure?

The context files use evidence labels. A named source file is strong evidence. A title-to-path match is only a search lead and is never presented as confirmed implementation.

## Built for humans and agents

Everything is Markdown. Retrieval needs only `rg`.

- Stable IDs and YAML front matter
- Broad tags that can be combined
- Aliases for symbols, events, packages, and older names
- Ready-to-copy regex patterns
- Direct links to raw notes and pinned source code
- Typed links: `source-link`, `shares-code-with`, and `same-design-pressure`
- Status gates so rejected proposals do not become accidental implementation plans
- `AGENTS.md` with a safe retrieval and handoff routine

No vector database, knowledge-graph service, website, or custom search daemon is required.

## The four folders

1. `01-book/` — the 3,050-page Judgment Edition with 705 diagrams.
2. `02-notes/` — all 687 canonical English Agent Notes copied byte-for-byte from commit `47f943859bef60e4160492346772ded9b24f765a`.
3. `03-agent-context/` — 687 tagged implementation-context files, plus Markdown indexes and regex recipes.
4. `04-code-samples/` — byte-identical TypeScript files from the pinned deepseek-harness commit for the most-cited code paths and long-running agent primitives, plus provenance and licensing.

The samples include compaction, persisted goals and goal-round continuation, workflow and Ralph-loop orchestration, session checkpointing and persistence, continuable subagents, and workspace-instruction context. They are deliberately selected source boundaries rather than a replacement for the upstream monorepo.

The third folder is a map, not a second source of truth. If a context file conflicts with its raw note or the pinned code, the raw note and code win.

## Start here

Human reader:

1. Read the PDF for the connected story and mental models.
2. Use `03-agent-context/INDEX.md` when an idea becomes useful.
3. Read the paired raw note before adopting the design.

Implementation agent:

1. Read `AGENTS.md`.
2. Search `03-agent-context/` before searching raw notes.
3. Check `status` and `implementation_evidence`.
4. Read the raw note and pinned source paths.
5. Use `04-code-samples/DEEP-DIVE-MAP.md` to find local code for the major runtime capability families.
6. Inspect the target repository before naming target files.
7. Port the contract, ownership, failure behavior, and tests—not the original package layout.

## Search examples

Find context-compaction notes:

```bash
rg -l --fixed-strings 'domain/context' 03-agent-context
```

Find context notes that also discuss recovery:

```bash
rg -l -0 --fixed-strings 'domain/context' 03-agent-context   | xargs -0 -r -n 40 rg -l --fixed-strings 'concern/recovery'   | sort -u
```

Search the vocabulary used by event and content pipelines:

```bash
rg -n --pcre2 '(ContentBlockMap|BlockAssembler|SessionEventMap)'   02-notes 03-agent-context
```

See `03-agent-context/REGEX-SEARCH.md` for reusable patterns.

## Corpus

| Note state | Count |
|---|---:|
| Implemented | 506 |
| Proposed | 25 |
| Rejected | 11 |
| Archived | 143 |
| Note-system roots | 2 |

| Code-map confidence | Count | Meaning |
|---|---:|---|
| High | 201 | A source file or code declaration is named directly. |
| Medium | 454 | An owning package, directory, or symbol is confirmed. |
| Search lead only | 32 | Useful place to inspect; not claimed as implementation. |

## Rust harness lens

The porting notes use one concrete target so advice does not stay vague:

- Tokio owns async execution.
- QuickJS/rquickjs is an orchestration DSL with explicit Rust capabilities.
- The model-facing core remains four tools: `read`, `write`, `patch`, and `exec`.
- Raw session events and evidence survive compaction.
- Provider and platform quirks stay at adapters.
- Windows sandbox support is probed at runtime and fails closed.

These are adaptation prompts, not claims that DeepSeek Harness uses this Rust design.

## Suggested GitHub topics

`agent-systems` `agent-runtime` `ai-engineering` `harness-engineering` `coding-agents` `llm-agents` `rust` `tokio` `context-management` `software-architecture` `engineering-decisions` `technical-writing`

## Source and license

- Upstream repository: https://github.com/deepseek-ai/deepseek-harness
- Pinned commit: `47f943859bef60e4160492346772ded9b24f765a`
- Upstream license: MIT; its `LICENSE` is included here.
- The raw-note layer is unchanged. Generated explanations should be checked against the pinned note and code before production use.
