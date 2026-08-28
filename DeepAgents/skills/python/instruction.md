# Python Instructions

## Implementation

- Follow the existing Python version, project layout, formatter, linter, dependency manager, and naming conventions.
- Read before editing: inspect the owning function, its callers, neighboring tests, and configuration.
- Prefer small functions, explicit inputs/outputs, `pathlib`, context managers, dataclasses, and dependency injection.
- Keep imports side-effect free and avoid module-level network calls, model calls, or filesystem mutation.
- Validate configuration and external data at the boundary; use environment variables for credentials.
- Preserve compatibility and document intentional breaking changes.

## Errors and Security

- Catch narrow exception types, preserve context with `raise ... from error`, and never use bare `except`.
- Do not silently swallow errors or return plausible fake data after a dependency failure.
- Add timeouts, bounded retries, cancellation, and cleanup to I/O and concurrency code.
- Redact secrets and sensitive prompts from logs, exceptions, snapshots, and reports.
- Treat model output, tool arguments, retrieved content, and deserialized data as untrusted.

## Deep Agents

- Keep state serializable and versionable; keep large artifacts in files or stores.
- Bound recursion, parallel tasks, tool calls, tokens, and wall-clock duration.
- Separate thread-scoped checkpoints from cross-thread stores.
- Make tool permissions and destructive actions explicit.

## Tests and Notebooks

- Use deterministic fakes or mocks instead of live model and cloud calls in unit tests.
- Cover invalid input, dependency errors, retries, timeouts, persistence, and duplicate calls.
- Keep notebook cells independently understandable and preserve valid notebook JSON and existing cell metadata.
- Run the narrowest focused test before the full suite, then report the exact command and result.
