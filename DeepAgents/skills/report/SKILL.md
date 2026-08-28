---
name: report
description: Use after a Deep Agent gives a substantive answer, completes a research or coding workflow, uses tools or subagents, or produces an experiment result that should be recorded as a report.
user-invocable: false
---

# Deep Agent Report Skill

Create a factual, auditable Markdown report after a substantive Deep Agent answer, research run, coding workflow, tool invocation, subagent task, or notebook experiment. The report is a compact record of what happened, what supports it, and what remains uncertain; it is not a transcript or a second speculative answer.

## When to Report

Use this skill when the run changes files, calls tools or services, delegates work, performs research, produces an experiment result, makes a production recommendation, or could reasonably need later review. For a trivial conversational response, a report is usually unnecessary unless the user explicitly requests one.

## Required Sections

1. **Question**: preserve the original request, constraints, and relevant context.
2. **Answer**: summarize the result and distinguish delivered work from recommendations.
3. **Work Performed**: list tools, subagents, files, APIs, commands, and experiments actually used.
4. **Evidence**: record tests, outputs, source titles/URLs supplied by the run, observed facts, and assumptions.
5. **Uncertainty**: identify unverified claims, failures, missing data, version-sensitive behavior, and residual risk.
6. **Next Actions**: list concrete owners or follow-up steps when known.

For code work, include changed files, validation commands and results, known test gaps, and rollback considerations. For research, distinguish primary sources, secondary sources, retrieved content, and model synthesis. For AWS or LangGraph runs, record environment, region/service assumptions, thread/checkpoint/store scope, and security or operational constraints without exposing sensitive values.

## Evidence Discipline

Separate these labels when useful:

- **Observed**: directly returned by a tool, test, file, command, or cited source.
- **Inferred**: a reasoned conclusion supported by observed evidence.
- **Proposed**: a suggested design or next step that was not executed.
- **Unverified**: a claim that needs a current documentation, integration, or production check.

Never invent sources, citations, tests, tool calls, file changes, successful outcomes, metrics, or timestamps. If a command failed, record the failure and its impact rather than smoothing it over.

## Privacy and Security

Redact API keys, tokens, passwords, cookies, authorization headers, private URLs, personal data, raw secrets, and sensitive prompt or document content. Preserve useful safe identifiers such as relative file paths, command names, resource types, run IDs, and truncated non-secret error codes. Do not include full model transcripts when a summary is sufficient.

## Storage and Naming

Save to `reports/<descriptive-name>.md` only when the user requests a durable artifact or the experiment workflow calls for it. Use a stable, filesystem-safe name and avoid putting secrets or personal data in filenames. If no file is requested, return the report in the answer instead of silently writing one. Do not claim that a report was saved unless the write succeeded.

## Quality Gate

Before finalizing, check that the question is represented accurately, every claim has an evidence status, failures and uncertainty are visible, secrets are redacted, links and paths are valid, and next actions are actionable. Keep the default report to one page or less; expand only when the run has enough evidence to justify detail.
