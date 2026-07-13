# System Bible Index

The System Bible is the authoritative product and engineering reference. It is a living part of the repository, not a separate planning exercise.

## Volume 1 — Product Vision and Scope

Defines mission, users, goals, non-goals, guiding principles, module boundaries, and success criteria.

## Volume 2 — Domain Model

Defines Student, Household, Academic Year, Enrollment, Application, Financial Aid Application, Tuition Package, Contract, Kollel Compensation, Task, Communication, Attachment, and Audit Event.

## Volume 3 — Business Workflows

Planned workflow documents:

- New student application
- Admissions review
- Accepted student enrollment
- Returning student enrollment
- Not-returning and withdrawal
- Financial-aid invitation and submission
- Financial-aid review and award
- Tuition setting
- Contract generation and acceptance
- Kollel enrollment
- Monthly Kollel pay
- Academic-year rollover
- Midyear division/program change
- Graduation and alumni transition

## Volume 4 — Business Rules Catalog

Every rule must have:

- Identifier
- Description
- Trigger
- Inputs
- Result
- Exceptions
- Permission requirements
- Audit behavior
- Test cases

## Volume 5 — Data Dictionary

For every table and field:

- Name
- Type
- Required status
- Source
- Validation
- Visibility
- Edit permissions
- Historical behavior
- Sensitive-data classification

## Volume 6 — UI and Screen Specifications

Planned screen specifications:

- Global dashboard
- Student search
- Student profile
- Student annual-history view
- Application form
- Admissions review
- Enrollment dashboard
- Tuition setup
- Financial-aid form and review
- Contract review and signature
- Kollel monthly pay
- Configuration Portal
- Reports and exports
- User and permission management
- Audit viewer

## Volume 7 — Roles and Permissions

Defines role templates and granular permissions.

## Volume 8 — Reporting Catalog

Defines operational dashboards, reports, exports, filters, columns, totals, and access restrictions.

## Volume 9 — API and Integration Specification

Defines internal APIs, external integrations, webhooks, Admire integration, email delivery, storage, and identity.

## Volume 10 — Testing and Quality Standards

Defines unit, integration, workflow, authorization, migration, and end-to-end tests.

## Volume 11 — Deployment and Operations

Defines environments, secrets, backups, recovery, logging, monitoring, and release process.

## Volume 12 — Decision Records

Architecture Decision Records capture major choices and their rationale.

Initial decision:

- ADR-001: Native web forms and no PDF workflows

## Documentation rule

No material behavior may exist only in code. When behavior changes, the relevant Bible section and acceptance tests must change in the same pull request.
