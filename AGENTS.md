# AGENTS.md — using this wiki

## Retrieval

- Search context files before raw notes; context files supply aliases and code paths.
- Use `03-agent-context/REGEX-SEARCH.md` for composable `rg` patterns.
- Combine tags with bounded `rg -l -0 ... | xargs -0 -r -n 40 rg -l ...` stages.
- Open linked notes of type `source-link` first, then `shares-code-with`, then `same-design-pressure`.

## Evidence discipline

- `named-file`: the source note named the file directly.
- `named-directory` or `named-package`: the source note named the owning area; inspect its entry points.
- `symbol-definition`: a construct named in the note resolves to that definition.
- `exact-code-occurrence`: the exact event/protocol literal occurs there; this is supporting evidence.
- `title-path-lead`: search lead only. Never call it the implementation without reading the code.

Status matters. `proposed`, `rejected`, and `archived` notes are not shipped implementation instructions.

## Porting to the Rust harness

Preserve pressure, ownership, contract, failure behavior, and evidence. Do not reproduce Cordis, TypeScript declaration merging, Node package topology, or every DeepSeek model-facing tool. Keep `read`, `write`, `patch`, and `exec`; build richer capabilities behind those contracts.

Never invent a target Rust file path. Inspect the current target repository first.

## Pinned source

All source links target `47f943859bef60e4160492346772ded9b24f765a`. If you compare against newer upstream code, state that you changed the evidence base.
