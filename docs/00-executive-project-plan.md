# Executive Project Plan

## 1. Project mission

Build a modern, web-based Student Management System that becomes the single source of truth for the complete student lifecycle: application, admissions, enrollment, tuition, financial aid, tuition contracts, Kollel participation and compensation, annual rollover, communications, reporting, and historical records.

The system will replace disconnected spreadsheets, manual tracking, and PDF-dependent workflows with structured web forms, configurable rules, live dashboards, and auditable history.

## 2. Core outcomes

The completed system must:

- Maintain one permanent Student record per individual.
- Preserve complete history across academic years.
- Make annual data accessible from both the Student record and academic-year workflows.
- Create Student records from submitted applications.
- Manage acceptance, rejection, additional information, and financial-aid invitations.
- Carry prior-year accepted/enrolled students into the next enrollment cycle.
- Allow students to be marked returning, not returning, deferred, withdrawn, or complete.
- Configure tuition by academic year and division.
- Build tuition from configurable components such as tuition, registration, dormitory, and meals.
- Support scholarships, proration, adjustments, and payment schedules.
- Present and sign tuition contracts as native web forms.
- Exclude Kollel students from tuition and tuition-contract requirements.
- Manage Kollel pay through configurable compensation components and monthly calculations.
- Provide a live enrollment dashboard with filtering, status, next action, alerts, and bulk actions.
- Provide a Configuration Portal for business variables.
- Preserve complete audit history.
- Support secure attachments without making PDFs part of the workflow.
- Support future integration with Admire.

## 3. Governing design principle

> The student is permanent; participation, workflow, and financial information are year-specific.

Year-specific information is stored under an Enrollment or related annual object, but must be accessible in two directions:

- From the Student record as complete history.
- From the selected academic year as a live operational workflow.

Both views must use the same underlying records.

## 4. Project workstreams

### A. Product and business analysis

Define modules, workflows, statuses, rules, forms, reports, permissions, and acceptance criteria.

### B. System Bible

Maintain the authoritative documentation for the project. The Bible develops alongside the software and is updated whenever behavior changes.

### C. Architecture and platform

Establish the application stack, database, API standards, security, configuration model, audit model, background jobs, storage, testing, and deployment.

### D. Functional implementation

Build the system incrementally through deployable milestones.

### E. Data migration

Import existing student, enrollment, financial, and Kollel history from spreadsheets and selected legacy sources.

### F. Validation and rollout

Pilot, reconcile, train, run parallel where necessary, and transition to production.

## 5. High-level project sequence

1. Establish documentation and development standards.
2. Define the domain model and MVP boundaries.
3. Build platform foundation.
4. Build permanent Student and annual Enrollment records.
5. Build Application Portal and admissions workflow.
6. Build annual enrollment workflow and live dashboard.
7. Build tuition configuration, student tuition packages, and web contracts.
8. Build financial-aid application and decision workflow.
9. Build Kollel compensation and monthly pay processing.
10. Build communications, reporting, and exports.
11. Build academic-year rollover.
12. Build migration tooling.
13. Pilot and launch.
14. Add integrations and later enhancements.

## 6. Success measures

The project is successful when:

- Staff can see every active student's enrollment position in one live dashboard.
- Student history is complete and not overwritten.
- No operational process depends on generated or fillable PDFs.
- A new academic year can be prepared primarily through configuration.
- Tuition and Kollel pay calculations are reproducible and auditable.
- Signed contracts preserve exact terms and values.
- AI coding agents can implement milestones without guessing core requirements.
- The codebase remains modular, tested, and documented.
