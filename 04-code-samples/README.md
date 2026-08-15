# Official Code Samples

Byte-identical source files copied from deepseek-ai/deepseek-harness at commit 47f943859bef60e4160492346772ded9b24f765a.

Use this folder when the official monorepo is not reachable. Every file maps one-to-one to a source link in 03-agent-context: the original package path is preserved under packages/.

For a direct notes-to-code reading path, start with [DEEP-DIVE-MAP.md](DEEP-DIVE-MAP.md).

For the detailed event-log, persistence, recovery, and compaction model, read [EVENT-SOURCING-DURABILITY.md](EVENT-SOURCING-DURABILITY.md).

For tool-result retention, Bash spill behavior, model context pruning, and long-running job control, read [TOOL-RESULT-LIFECYCLE.md](TOOL-RESULT-LIFECYCLE.md).

## Layout

04-code-samples/packages/... matches the source path named by the notes. For example, packages/core/tools/src/index.ts in the notes resolves to 04-code-samples/packages/core/tools/src/index.ts here.

## What is included

- Core tool registry, typed schema DSL, invariants, code mode, and Python typing surfaces
- Event-sourced session log, surface projection, agent-loop orchestration, and resume behavior
- Compaction seam, checkpoint vocabulary, summarization backend, and tool-result pruning
- Persisted goal lifecycle, model-facing goal tools, and the goal-round continuation driver
- Session checkpoint policy, crash recovery coverage, durable persistence, and write-behind coordination
- Workflow engine, worker-thread runtime, general workflow tool, and the fresh-agent Ralph loop
- Continuable subagent lifecycle, durable descriptors, follow-ups, and settlement behavior
- Workspace `AGENTS.md`-style instruction loading, rendering, and context-state updates
- LLM service types plus the pi-ai provider adapter and catalog
- Host API proxy and ACP bridge surface
- Local filesystem IO, tool-fs session cwd, e2b filesystem, and bash tool
- Skill surface
- Client connection fixture plus trajectory and conversation UI samples
- ACP snapshot test support
- Deep implementation coverage for subagents, process execution, skill discovery, and real-time prompt/context assembly

## Provenance

- Source: https://github.com/deepseek-ai/deepseek-harness
- Pinned commit: 47f943859bef60e4160492346772ded9b24f765a
- License: MIT; see LICENSE.deepseek-harness and the repository root LICENSE.
- Files are unmodified copies. Refresh by re-copying from the pinned commit and recording the new SHA.

## Search examples

```bash
rg -n "ToolRegistry" 04-code-samples/packages/core/tools/src
rg -l "SessionEventMap" 04-code-samples/packages/core/session/src
```
