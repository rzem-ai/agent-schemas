# agent-schemas

JSON Schemas for rzem-ai agent configuration, published so a bundle can point
`$schema` at a stable URL and get completion and validation in any editor that
speaks JSON Schema.

## Schemas

| File | Describes |
| --- | --- |
| [`agent.schema.json`](agent.schema.json) | An `agentd` bundle's `agent.json` — the `options` block (a verbatim mirror of the Claude Agent SDK's `Options`) and the `runtime` block (logging, telemetry, transcripts, sessions, budgets, health, MCP surface, A2A peers). |

## Using it

Add `$schema` as the first key of a bundle's `agent.json`:

```jsonc
{
  "$schema": "https://rzem-ai.github.io/agent-schemas/agent.schema.json",
  "name": "angus",
  "options": {
    "model": "claude-opus-5"
  },
  "runtime": {
    "envFile": ".env"
  }
}
```

The raw file works too, if you would rather not depend on Pages:

```
https://raw.githubusercontent.com/rzem-ai/agent-schemas/main/agent.schema.json
```

Both serve the same bytes. The Pages URL returns `application/json`; the raw URL
returns `text/plain`, which most tooling accepts but a strict validator may not.

`agentd` strips `$schema` before validating, so adding it does not upset the
loader — the config's own schema is `.strict()` and would otherwise reject an
unknown key.

## This file is generated

`agent.schema.json` is **not** hand-edited. It is generated from the zod schemas
that the runtime validates against, so the published schema and the loader can
never disagree:

```bash
# in the agentd runtime repo
bun run schema        # writes schema/agent.schema.json from src/config/schema.ts
```

A test in that repo asserts the checked-in copy matches what the generator
produces, so a schema change cannot land without regenerating.

To publish an update, regenerate there and copy the result here.

## Adding another schema

Drop it in at the root as `<name>.schema.json` and add a row to the table above.
Both URL forms follow automatically.

## Licence

MIT — see [LICENSE](LICENSE).
