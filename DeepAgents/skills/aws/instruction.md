# AWS Instructions

## Required Behavior

- Start with the smallest relevant AWS surface and name assumptions about region, runtime, data, scale, and availability.
- Distinguish control-plane setup, infrastructure as code, runtime application code, and operational procedures.
- Use the repository's existing SDK and infrastructure patterns. Verify version-sensitive APIs against installed packages or current official documentation.
- Use placeholders such as `AWS_REGION`, `YOUR_RESOURCE_ARN`, and `YOUR_BUCKET_NAME`; never use realistic secrets.
- Prefer workload roles, short-lived credentials, KMS encryption, private connectivity where justified, and CloudWatch observability.

## Failure Handling

- Configure connect/read timeouts and bounded retries with exponential backoff and jitter only for transient failures.
- Handle throttling, expired credentials, access denial, missing resources, malformed input, and service outages as distinct cases.
- Use idempotency for writes and explain duplicate-event behavior.
- Do not log authorization headers, credentials, raw sensitive prompts, or unbounded model/tool payloads.

## Deep Agent Guardrails

- Treat tools as privileged APIs, not as trusted model suggestions. Validate authorization on the server for every call.
- Keep tenant IDs and store namespaces derived from authenticated context, never from arbitrary model output.
- Bound agent recursion, tool fan-out, token usage, wall-clock time, and AWS spend.
- Make destructive actions require explicit user intent and a confirmation step when appropriate.

## Verification

- Prefer mocked AWS clients for unit tests and disposable resources for integration tests.
- Include at least one negative-path test for permissions, timeout/throttling, and malformed input when changing an AWS integration.
- Flag service limits, pricing, regional support, and SDK signatures as claims requiring current documentation checks.
