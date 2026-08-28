---
name: python
description: Use when a Deep Agent request involves Python code, debugging, typing, packaging, testing, notebooks, async code, dependencies, or Python project structure.
user-invocable: false
---

# Python Skill

Implement, review, debug, and package production-quality Python for Deep Agents. Favor simple interfaces, explicit contracts, deterministic tests, and behavior that remains understandable six months later.

## Discovery and Design

1. Read the nearest implementation, tests, configuration, and call sites before editing.
2. Identify the Python version, selected interpreter, dependency manager, import style, formatter, linter, and test runner.
3. State the behavior that currently exists, the desired behavior, and one focused check that can disprove the hypothesis.
4. Preserve public APIs and compatibility unless the request explicitly permits a breaking change.
5. Choose a small design: pure functions for transformations, explicit dependency injection for I/O, and narrow modules with one reason to change.

Use `pathlib` for paths, context managers for owned resources, dataclasses or typed mappings for structured data, and standard-library types for public contracts. Prefer an existing dependency over a new one, but do not reimplement mature security, parsing, HTTP, or validation libraries.

## Types and Interfaces

Type public functions, returned data, configuration, and callback boundaries. Use meaningful names, protocols for replaceable dependencies, and discriminated result types when success and failure have different shapes. Avoid `Any` at system boundaries unless it is immediately validated. Validate external JSON, environment variables, files, model output, and tool arguments before use.

## Error Handling and Resilience

Catch only exceptions you can handle. Preserve the original exception with chaining, add useful context, and let unexpected failures reach the correct boundary. Distinguish validation, authorization, not-found, timeout, dependency, and programmer errors. Use bounded retries only for transient operations, with timeouts and idempotency for writes. Never use bare `except`, silent fallbacks, or unbounded loops.

For concurrent or async code, define cancellation behavior, limits, backpressure, and cleanup. Do not block the event loop with synchronous network or file operations. Close clients, files, tasks, and subprocesses deterministically.

## Deep Agents

Keep model calls, tools, state reducers, checkpointers, stores, file backends, and side effects behind small functions. Bound prompt size, recursion, parallel fan-out, wall-clock time, and retries. Treat user input, retrieved documents, tool results, and model output as untrusted. Serialize only stable, versioned state; keep secrets and large artifacts out of state and prompts.

## Testing

Test behavior rather than exact model wording. Cover happy paths, invalid input, dependency failure, timeout, retry exhaustion, duplicate requests, persistence boundaries, and authorization. Use deterministic fakes and mocks for model and cloud calls. Add property or parameterized tests for parsers and state transitions where edge cases are numerous. Keep integration tests isolated and label live-service tests clearly.

## Packaging and Operations

Pin or constrain dependencies according to repository policy, keep runtime and development dependencies separate, and make configuration fail fast with actionable messages. Emit structured logs without secrets, expose useful metrics, and include correlation IDs across agent and tool calls. Design imports to be side-effect free and keep startup work bounded.

## Validation Order

Run the narrowest relevant test first, then syntax/import checks, type checking, lint/format checks, and the broader suite. Report commands run and failures honestly.

## Output

Explain assumptions and the changed contract briefly. Include runnable code, focused tests, error behavior, and the exact validation command when code is requested.
