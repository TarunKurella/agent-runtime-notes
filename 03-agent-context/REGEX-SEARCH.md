# Composable regex search

This wiki does not require embeddings. Tags provide the concept map; aliases and regexes bridge naming differences; typed links provide traversal.

## Search one concept

```bash
rg -l --fixed-strings 'domain/context' 03-agent-context
rg -n --pcre2 '(?i)(compaction|context window|token meter|summary prefix)' 02-notes 03-agent-context
```

## Compose concepts with an AND pipeline

Example: context decisions that also discuss recovery:

```bash
rg -l -0 --fixed-strings 'domain/context' 03-agent-context \
  | xargs -0 -r -n 40 rg -l --fixed-strings 'concern/recovery' \
  | sort -u
```

Example: implemented LLM boundaries with evidence:

```bash
rg -l -0 --fixed-strings 'lifecycle/implemented' 03-agent-context \
  | xargs -0 -r -n 40 rg -l -0 --fixed-strings 'domain/llm' \
  | xargs -0 -r -n 40 rg -l --fixed-strings 'concern/evidence' \
  | sort -u
```

## Search code constructs across notes and context

```bash
rg -n --pcre2 '(ContentBlockMap|BlockAssembler|SessionEventMap)' 02-notes 03-agent-context
rg -n --pcre2 '(agent/pre-step|agent/request-error|turn/end)' 02-notes 03-agent-context
```

## Reusable concept patterns

Copy a pattern into `rg --pcre2` and combine searches with the AND pipeline above.

### LLM and providers

```regex
\b(llm|model|provider|adapter|inference|content[- ]?block|prompt|token usage)\b
```

### Session and durable state

```regex
\b(session|event[- ]sourc|event log|replay|projection|fork|history)\b
```

### Context and compaction

```regex
\b(compaction|context window|context compiler|token meter|summary prefix|prompt assembly|spill)\b
```

### Tools

```regex
\b(tool call|tool result|defineTool|model-facing tool|tool schema|tool registry)\b
```

### Filesystem and workspace

```regex
\b(filesystem|ctx\.fs|file patch|workspace|path resolution|symlink|atomic write|mode bits)\b
```

### Shell, terminal, and processes

```regex
\b(shell|bash|terminal|subprocess|pty|process tree|command output)\b
```

### Storage and persistence

```regex
\b(persistence|storage|jsonl|sqlite|checkpoint|durable|write-behind|snapshot)\b
```

### Extensions and capability seams

```regex
\b(plugin|cordis|hook|extension|service provider|capability seam|registration scope)\b
```

### Security and sandboxing

```regex
\b(sandbox|security|permission|credential|secret|untrusted|appcontainer|landlock|seatbelt|acl|dacl)\b
```

### Cancellation and timeout

```regex
\b(cancel|cancellation|timeout|deadline|abortsignal|abort signal)\b
```

### Recovery and retry

```regex
\b(retry|recover|recovery|resume|crash|repair|torn|failure path|fallback)\b
```

### Ownership

```regex
\b(own|owner|ownership|authoritative|source of truth|single writer)\b
```

### Evidence and rules that must stay true

```regex
\b(evidence|verify|verification|invariant|test|observable|reconstruct)\b
```


## Follow the note graph

Every context file has typed links:

- `source-link`: the original note linked the decision.
- `shares-code-with`: both notes point at one or more source files.
- `same-design-pressure`: shared tags or title concepts; useful for comparison, not proof of dependency.

Start with `source-link`, then inspect shared code, then broaden by design pressure.
