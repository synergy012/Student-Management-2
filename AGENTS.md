# AI Agent Instructions

## Purpose

This repository is intended to be developed with substantial AI assistance. AI agents must follow the System Bible and may not invent material business rules.

## Required workflow

Before implementing any feature:

1. Read the relevant System Bible sections.
2. Identify the milestone and acceptance criteria.
3. Inspect the current code and tests.
4. State any assumptions.
5. Implement the smallest complete change.
6. Add or update tests.
7. Run linting, type checking, tests, and migrations.
8. Update documentation when behavior changes.
9. Summarize files changed, tests run, and unresolved risks.

## Governing rules

- The permanent person is the `Student`.
- Year-specific participation is stored through `Enrollment`.
- Information must be accessible both from the Student history and from academic-year workflows.
- Applications, financial-aid forms, contracts, acknowledgments, and signatures are native web workflows.
- Do not create, fill, route, or depend on PDFs.
- Supporting file uploads may be allowed, including PDFs, but files are attachments only.
- Do not hard-code division names, tuition components, Kollel pay components, status choices, contract wording, or academic years.
- Historical records and signed submissions are immutable.
- Changes requiring historical correction must create a new version or adjustment.
- All consequential changes must be audited.
- Never store secrets in application-controlled configuration tables or commit them to the repository.
- Avoid Boolean workflow fields when a versioned status record or state transition is appropriate.
- No feature is complete without acceptance tests.

## Architecture guardrails

- Use one authoritative data source.
- Do not duplicate annual data on the Student model.
- Current-year summaries must be derived from annual records.
- Business rules belong in domain services, not UI components.
- Use database constraints where practical.
- Use migrations for schema changes.
- Use money-safe decimal types.
- Use UTC timestamps internally.
- Authorization must be enforced server-side.

## Definition of done

A task is complete only when:

- Acceptance criteria pass.
- Tests pass.
- Type checking and linting pass.
- Database migration impact is documented.
- Permissions are enforced.
- Audit behavior is included.
- Error and empty states are handled.
- Documentation is updated.
