# LangGraph Instructions

## Before Implementation

- Inspect installed `deepagents` and `langgraph` versions and verify version-sensitive signatures.
- Write down state schema, reducers, entry/finish points, conditional routes, side effects, retry boundaries, and interrupt points.
- Identify whether each datum belongs in ephemeral state, a thread checkpoint, a durable store, or an external artifact.

## Graph Discipline

- Keep nodes focused and return explicit updates; avoid in-place mutation of shared state.
- Make loops and fan-out bounded with recursion, branch, token, and wall-clock limits.
- Use structured subagent results and discard raw transcripts when the parent does not need them.
- Make every external write idempotent or protect it with a confirmation and deduplication strategy.
- Define behavior for cancellation, timeout, partial branch failure, and resume after a checkpoint.

## Persistence

- Use `MemorySaver` and `InMemoryStore` for local demonstrations unless persistence itself is being tested.
- Never confuse checkpoint state with long-term memory.
- Document `thread_id`, tenant context, namespace tuple, and backend route in every persistence example.
- Keep secrets, unbounded prompts, and raw private documents out of checkpoints and stores.

## Tools and Security

- Validate tool arguments and authorize every resource server-side.
- Treat model output, tool output, and retrieved documents as untrusted.
- Separate read/write tools and require confirmation for destructive operations.
- Redact message content, credentials, and private data from logs and reports.

## Verification

- Test routing and reducers without a live model.
- Test checkpoint resume, store isolation, loop termination, retry exhaustion, malformed tool results, and duplicate side effects.
- Add structured run/node/tool IDs and latency/error metrics without logging sensitive payloads.
