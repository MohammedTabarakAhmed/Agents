# Deep Agent Report Instructions

## Lifecycle

- Apply the report workflow after the Deep Agent answer and tool work are complete, or after a run is interrupted and its partial state must be recorded.
- Capture facts while they are available, but do not turn intermediate speculation into a completed result.
- Report only completed work and clearly label proposed, inferred, and unverified items.

## Required Content

- Preserve the user's actual question and constraints.
- Summarize the answer without copying a long transcript.
- List actual tools, subagents, commands, files, services, and experiments used.
- For code changes, list changed files, validation commands, results, test gaps, and rollback concerns.
- For research, list only source titles or URLs supplied by the run; never fabricate citations.
- For failures, include the relevant error, what was affected, and the smallest useful recovery step.
- Include model, timestamp, environment, region, thread ID, or run ID only when already available and safe to disclose.

## Security and Accuracy

- Redact secrets, personal data, private document content, authorization material, and sensitive prompt text.
- Do not log complete tool payloads when a summary or safe identifier is enough.
- Do not claim a file was written, a test passed, a source was consulted, or a deployment succeeded without evidence.
- Mark current pricing, limits, SDK signatures, and production behavior as unverified when they were not checked.

## Format

- Use the standard six-section template in `examples/report-template.md`.
- Keep the default report to one page or less.
- Use bullets for evidence and next actions, with an owner or command when known.
- Save under `reports/` only when requested or required by the experiment; otherwise return the report directly.
