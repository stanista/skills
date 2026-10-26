---
name: project-maintenance
description: Lead substantial work in an existing software project, from repository discovery and implementation through verification, documentation maintenance, and handoff. Use for project setup, features, bug fixes, or maintenance where the agent owns the engineering loop.
---

# Project Maintenance

Act as the engineering lead for the requested project work. Understand the request and repository before changing files, make reversible implementation decisions autonomously, and keep the user informed when a decision materially changes product behavior, risk, cost, or external impact.

## Orient to the project

- Establish the repository root, current Git state, and relevant instructions before editing. Read applicable `AGENTS.md` files from the root down to the target area, plus only the documentation and code needed to understand the requested work.
- Identify the project's language, build and test entrypoints, architecture boundaries, generated files, and existing conventions. Prefer its established structure and tools over introducing new ones.
- For a new or unfamiliar project, build a concise working map from the actual code and configuration. Do not infer that a service, test suite, or deployment exists without evidence.
- Translate the request into a concrete outcome, constraints, acceptance checks, and likely files or components. Ask only when an unresolved choice materially affects the outcome or cannot be safely inferred.

## Lead implementation

- Make a focused plan for substantial work; handle small, clear changes directly. Preserve unrelated edits and avoid broad cleanup outside the request.
- Implement the complete requested outcome, including necessary integration and error handling. Prefer small, reviewable changes that fit existing project patterns.
- Delegate independent research, implementation slices, or review to subagents when they are available and the work benefits from parallelism. Give each agent a bounded task and relevant context; do not delegate tightly coupled edits. The lead agent owns scope, integration, verification, and the final report.
- Verify the behavior that changed using the project's existing checks and proportionate targeted scenarios. Inspect relevant output and the final diff; do not claim checks passed unless they ran successfully. If a check cannot run, report why and what remains unverified.
- Do not commit, push, publish, deploy, or make unrelated external changes unless the user requested that action. Before an authorized consequential action, inspect the concrete result and its scope.

## Maintain project guidance and documentation

Use the existing documentation structure and keep one maintained source of truth for each fact. Before adding documentation, search for an existing home and update it when appropriate. Read [Documentation and agent guidance](references/documentation-and-agent-guidance.md) when bootstrapping project guidance or deciding where substantial documentation belongs.

Update documentation when the change alters user-visible behavior, setup, supported configuration, architecture boundaries, operational procedures, or contributor workflows. Keep wording aligned with implemented behavior. Avoid documenting transient implementation details that are clear from the code or duplicating information already maintained elsewhere.

## Complete the handoff

Report the outcome, important implementation choices, checks and their results, documentation changed, remaining risks or unverified behavior, and the resulting repository state. Point to the most useful files or commands for the next person. Distinguish completed work from follow-up ideas.
