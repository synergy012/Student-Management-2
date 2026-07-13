# Development Workflow

## 1. Work item lifecycle

Every feature follows:

1. Define requirement.
2. Update or reference System Bible.
3. Write acceptance criteria.
4. Identify domain objects and permissions.
5. Design schema/API/UI changes.
6. Implement backend.
7. Implement frontend.
8. Add tests.
9. Run quality checks.
10. Update documentation.
11. Review.
12. Merge.

## 2. Ticket template

Each implementation ticket must include:

- Objective
- User role
- Background
- In scope
- Out of scope
- Business rules
- Data affected
- Permissions
- Audit requirements
- UI behavior
- Error states
- Acceptance criteria
- Required tests
- Migration impact
- Documentation references

## 3. AI task prompt standard

A coding prompt should state:

- Repository and branch
- Milestone and ticket identifier
- Exact Bible sections
- Expected files or layers
- Required behavior
- Non-goals
- Tests to add
- Commands to run
- Required final summary

## 4. Pull request standard

Every pull request must explain:

- What changed
- Why
- User impact
- Data-model impact
- Security/permission impact
- Audit impact
- Tests run
- Screenshots for UI changes
- Migration and rollback notes
- Documentation updated
- Known limitations

## 5. Quality gates

Before merge:

- Formatting passes
- Linting passes
- Type checking passes
- Unit tests pass
- Integration tests pass
- Authorization tests pass
- Migrations apply cleanly
- No secrets are committed
- Documentation is updated
- Acceptance criteria are demonstrated

## 6. Change control

Material changes to the domain model, workflow, security model, or architecture require an Architecture Decision Record.

Business-rule changes require:

- Updated rule documentation
- Effective date or academic year
- Historical-impact analysis
- Tests
- Audit behavior
