# Rust porting rules

These are standing constraints referenced by the per-note context files.

## `rust/newtype-ids`

Represent identity domains with Rust newtypes; do not pass bare strings across session, call, turn, step, and agent boundaries.

## `rust/tagged-enums`

Use serde-tagged enums for a closed wire vocabulary. Add an explicit extension envelope only where runtime extension is truly required.

## `rust/trait-seam`

Use a small trait at the capability boundary; keep policy and model-facing schemas outside the backend implementation.

## `rust/tokio-cancellation`

Thread one cancellation token through async work, close owned resources before reuse, and test cancellation at every await boundary.

## `rust/append-log`

Keep durable facts append-only and derive views from them. Separate the synchronous fact boundary from asynchronous persistence.

## `rust/serde-validation`

Parse untrusted input into strict serde types and validate semantic constraints before executing side effects.

## `rust/provider-adapter`

Keep provider wire quirks inside adapters; the loop and durable event vocabulary remain provider-neutral.

## `rust/capability-security`

Make granted capabilities explicit. On the Windows no-admin path, probe support at runtime and fail closed rather than silently running unsandboxed.

## `rust/context-evidence`

Compaction may shrink presentation, but it must not erase the evidence ledger, file activity, or durable session facts.

## `rust/four-tool-surface`

Preserve the four model tools: read, write, patch, and exec. Reuse internal capabilities without automatically exposing new model-facing tools.

## `rust/quickjs-boundary`

QuickJS holds orchestration variables; Rust/Tokio owns async execution and exposes only explicitly granted capabilities.

## `rust/process-discipline`

Treat this as repository or release policy unless it changes a runtime contract. Do not add runtime machinery for a process-only lesson.
