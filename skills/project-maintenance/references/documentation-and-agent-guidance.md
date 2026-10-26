# Documentation and Agent Guidance

Use this reference when establishing or reshaping project guidance. Follow repository conventions when they already work; this is a decision guide, not a required document template.

## Choose the right home

- Put durable instructions for coding agents in `AGENTS.md`: repository conventions, important invariants, safe commands, validation entrypoints, and pointers to deeper references. Keep root guidance broadly applicable and place specialized instructions near the relevant subproject when the repository supports scoped instructions.
- Put audience-facing setup, usage, and contribution steps in the project's README or contributor documentation.
- Put architecture rationale and system boundaries in an architecture document when those facts span components and are not readily discoverable from the code.
- Put operational procedures in a runbook and interface or configuration contracts alongside their existing API or schema documentation.
- Keep decision records for choices whose rationale will matter after the implementation context is gone. Do not create an ADR for routine implementation choices.

## Bootstrap without over-documenting

1. Inventory existing guidance and documentation, and check for nested `AGENTS.md` files before creating anything.
2. Derive commands, constraints, and architecture statements from source, manifests, CI, and existing workflows. Mark uncertainty instead of turning guesses into instructions.
3. Add only the missing guidance that will change how future work is performed. Make agent instructions concise and link to deeper documents rather than copying them.
4. Check that every new document has a clear owner or update trigger, is discoverable from an existing entrypoint, and does not contradict another source.

## Maintain accuracy

When a code or configuration change affects documented behavior, update the existing source of truth in the same change where practical. Remove or revise obsolete instructions, examples, and commands. Prefer concrete commands and prerequisites verified in the project. Avoid claims about tested platforms, security properties, production behavior, or support guarantees unless the repository provides evidence.

Do not add documentation solely to make a project look complete. A small project may need only focused agent guidance and a useful README; add architecture, operations, or decision records when their content has a real maintenance use.
