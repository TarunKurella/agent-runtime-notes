# Deep-Dive Notes To Code Map

Use this map to move from the engineering decision in `03-agent-context/` to the matching frozen source under `04-code-samples/packages/`. All paths retain the pinned upstream layout and are byte-identical copies of commit `47f943859bef60e4160492346772ded9b24f765a`.

Read the linked note first. It explains the ownership boundary, failure model, and status. Then follow the code order below; tests are included beside each package to show the executable contract.

## Subagents And Inter-Agent Communication

Notes: [subagent seam](../03-agent-context/0134-subagent-capability-seam.md), [background tasks](../03-agent-context/0148-background-subagent-tasks.md), [continuable subagents](../03-agent-context/0200-continuable-subagents.md), [report tool](../03-agent-context/0211-continuable-subagent-report-tool.md), [interrupt](../03-agent-context/0264-continuable-subagent-current-turn-interrupt.md), [settlement delivery](../03-agent-context/0265-settlement-delivery-belongs-to-the-continuation-manager.md), and [named continuation operations](../03-agent-context/0463-intent-named-subagent-continuation-operations.md).

1. Start with `packages/subagent/subagent/src/types.ts`, `index.ts`, and `descriptor.ts` for the durable service and child identity contracts.
2. Read `continuation.ts`, `child-agent.ts`, `assistant-output.ts`, `lifecycle.ts`, and `run-settlement.ts` for follow-up, report, cancellation, and cleanup ownership.
3. Read `tool-subagent/`, `tool-subagent-control/`, and `tool-subagent-report/` for the model-facing communication boundary.
4. Compare `subagent-spawn-in-process/`, `subagent-fork-in-process/`, `subagent-acp/`, `subagent-codex/`, and `subagent-claude-code/` to separate common contracts from transport providers.

## Process Execution And Terminal Lifecycle

Notes: [subprocess seam](../03-agent-context/0066-the-subprocess-service-is-its-own-seam-under-the-bash-executors-dsh-subp.md), [dispose ladder](../03-agent-context/0068-the-dispose-ladder-belongs-to-its-consumer-not-the-subprocess-seam.md), [sandbox boundary](../03-agent-context/0146-the-subprocess-sandbox-confinement-seam-native-runners-escalation-and-pe.md), [persistent PTY](../03-agent-context/0158-persistent-pty-sessions.md), and [persistent bash](../03-agent-context/0210-persistent-bash-and-string-replacement-editor-tools.md).

1. Start at `packages/subprocess/subprocess/src/types.ts`, then read `subprocess-local/src/spawn.ts`, `terminal.ts`, and `process-inspector.ts`.
2. Follow execution through `packages/shell/bash-local/`, `shell/bash-sandbox/`, `shell/tool-bash/`, and `shell/tool-bash-persistent/`.
3. Read `packages/terminal/terminal-bash/src/session.ts` and `packages/terminal/tool-terminal/src/index.ts` for persistent session ownership and model-facing terminal calls.
4. Use `process-exit.spec.ts`, `spawn.spec.ts`, `terminal.spec.ts`, and sandbox end-to-end tests to study teardown and confinement failures.

## Skill Discovery And Management

Notes: [host-held scoped registry](../03-agent-context/0116-the-skill-registry-is-host-held-and-layered-per-scope.md), [progressive disclosure](../03-agent-context/0143-skill-system-progressive-disclosure-instructions-for-agents.md), [catalog hot refresh](../03-agent-context/0192-skill-catalog-hot-refresh.md), [invocation policy](../03-agent-context/0204-independent-model-and-user-skill-invocation-policy.md), and [registry cleanup](../03-agent-context/0539-prune-unused-skill-registry-api.md).

1. Read `packages/skill/skill/src/index.ts` and `invariant.ts` for registry and scope contracts.
2. Read `packages/skill/skill-filesystem/src/index.ts` for discovery and refresh from the filesystem.
3. Read `packages/skill/tool-skill/src/index.ts` for what becomes model-visible, then use its tests to see progressive-disclosure behavior.
4. `skill-badge/` is a separate presentation/distribution example, not the registry core.

## Real-Time Prompt And Context Compilation

Notes: [prompt variables](../03-agent-context/0024-prompt-variables-and-tool-guidance-ownership.md), [reconstructable requests](../03-agent-context/0025-every-llm-request-is-reconstructable-from-the-session-log.md), [routed model context](../03-agent-context/0050-routed-model-context-and-compaction-policy.md), [workspace instructions](../03-agent-context/0136-workspace-context-instruction-files.md), [producer-declared context](../03-agent-context/0256-producer-declared-context-forms.md), [session prefix](../03-agent-context/0562-the-session-prefix-request-only-messages-in-front-of-the-derived-history.md), and [prompt/tool assembly history](../03-agent-context/0544-tool-schemas-are-part-of-the-system-prompt-assembly.md).

1. Start with `packages/core/agent/src/dispatch.ts` and `runtime-types.ts`: these turn current session state into a request assembly.
2. Read `packages/core/agent-loop/src/runtime-context.ts`, `tool-calls.ts`, and `agent.ts` for per-step compilation, ordering, and execution.
3. Read `packages/core/system-prompt/src/index.ts`, `packages/core/tools/src/json-schema.ts`, and `packages/llm/llm/src/assembler.ts` for prompt sections, tool schemas, and provider-ready messages.
4. Follow provider conversion in `packages/llm/llm-pi-ai/src/context.ts`, `adapter.ts`, and `stream.ts`.
5. Read `packages/context/agent-instructions/`, `context/session-reference/`, and `context/time-context/` for live context contributions and durable state.

## Long-Horizon Control Plane

Notes: [compaction seam](../03-agent-context/0133-compaction-as-a-capability-seam-abstract-contract-basic-backend.md), [goal execution](../03-agent-context/0157-harness-level-goal-based-execution.md), [goal rounds](../03-agent-context/0164-same-session-goal-round-driver.md), [Ralph workflow](../03-agent-context/0159-fresh-agent-ralph-workflow-tool.md), and [checkpointing](../03-agent-context/0328-compaction-checkpoints-use-an-english-engineering-register.md).

1. `packages/compaction/` contains the policy seam, summarizer, checkpoint events, and tool-result pruning.
2. `packages/goal/` contains persisted objective state, wrap-up authority, and the continuation driver.
3. `packages/workflow/` contains general worker-thread orchestration and the fixed-policy Ralph tool.
4. `packages/session/session-checkpoint-policy/` and `session-persistence/` show how long-running execution survives interruption without claiming exactly-once side effects.

## Verification Rule

This map is a retrieval guide, not a source of truth. Check the linked note's status and source-evidence table, then read the included tests. Do not treat an archived note as the current design without locating its successor.
