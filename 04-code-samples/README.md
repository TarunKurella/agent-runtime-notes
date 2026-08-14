# Official Code Samples

Byte-identical source files copied from deepseek-ai/deepseek-harness at commit 47f943859bef60e4160492346772ded9b24f765a.

Use this folder when the official monorepo is not reachable. Every file maps one-to-one to a source link in 03-agent-context: the original package path is preserved under packages/.

## Layout

04-code-samples/packages/... matches the source path named by the notes. For example, packages/core/tools/src/index.ts in the notes resolves to 04-code-samples/packages/core/tools/src/index.ts here.

## What is included

- Core tool registry, typed schema DSL, invariants, code mode, and Python typing surfaces
- Event-sourced session log, surface projection, and invariant checks
- Agent lifecycle and agent-loop orchestration
- LLM service types plus the pi-ai provider adapter and catalog
- Host API proxy and ACP bridge surface
- Local filesystem IO, tool-fs session cwd, e2b filesystem, and bash tool
- Skill surface and goal fold
- Client connection fixture plus trajectory and conversation UI samples
- ACP snapshot test support

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
