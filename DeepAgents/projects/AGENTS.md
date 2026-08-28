# DeepAgents Workspace Guide

This workspace contains experiments and projects built with Deep Agents, LangGraph, Python, and cloud services.

## Skills

Reusable skills live under `skills/`. Load the relevant `SKILL.md` when a user request matches its description:

- `aws`: AWS architecture, IAM, SDK usage, deployment, and operational guidance.
- `python`: Python implementation, debugging, packaging, testing, and quality practices.
- `langgraph`: LangGraph and Deep Agents graphs, state, persistence, tools, subagents, and middleware.
- `report`: Create a concise report after a Deep Agent produces an answer or completes a workflow.

Use the smallest relevant set of skills. Combine them when a request crosses boundaries, such as deploying a LangGraph agent to AWS.

## Deep Agent Answer Reports

For every substantive Deep Agent answer in this workspace, apply `skills/report/SKILL.md` after the answer is produced. Save reports under `reports/` when the user asks for a durable artifact or when the run is part of an experiment. Do not claim that a report was saved unless a file was actually written.

Reports should preserve the original question, summarize the answer, record tools and sources, identify uncertainty, and list follow-up actions. Never include API keys, tokens, passwords, or other secrets.

## Local Conventions

- Keep notebook edits valid JSON and preserve existing cell metadata.
- Prefer deterministic examples and mocked services in tests.
- Use `MemorySaver` and `InMemoryStore` for local Deep Agents demonstrations unless persistence is the subject of the example.
- Keep credentials in environment variables; never commit `.env` files or secrets.
