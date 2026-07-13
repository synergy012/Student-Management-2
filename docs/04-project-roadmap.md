# Project Roadmap

The roadmap is milestone-driven. Each milestone must produce tested, reviewable, and deployable functionality plus corresponding System Bible updates.

## Stage 0 — Documentation foundation

### Deliverables

- Executive Project Plan
- System Bible index
- Product vision and scope
- Initial domain model
- Roadmap
- AI agent instructions
- Architecture decision on web forms and PDFs

### Exit criteria

The repository has a clear governing structure and future work can be expressed as bounded milestones.

## Stage 1 — Architecture and engineering foundation

### Deliverables

- Final technology decision
- Repository structure
- Local development environment
- Database and migration framework
- Authentication
- Roles and permissions
- Audit framework
- Error handling
- Logging
- Test framework
- CI pipeline
- Seed-data framework

### Exit criteria

A developer can clone, configure, run, test, and deploy a secure skeleton application.

## Stage 2 — Core records

### Deliverables

- Student
- Household and relationships
- Academic Year
- Enrollment
- Student search
- Student profile
- Annual-history view
- Basic attachments
- Basic tasks and notes

### Exit criteria

Staff can create a Student, place the Student in an academic year, and view the same Enrollment from both the Student record and academic-year list.

## Stage 3 — Application and admissions

### Deliverables

- Public secure application
- Save and resume
- Submission snapshot
- Student creation/linking
- Admissions review queue
- Request additional information
- Accept, reject, waitlist
- Communications
- Audit history

### Exit criteria

A new applicant can submit online and staff can reach a final admissions decision without a PDF or spreadsheet.

## Stage 4 — Annual enrollment and live dashboard

### Deliverables

- Carry forward prior-year students
- Add accepted applicants
- Returning/not-returning workflow
- Division/program assignment
- Live dashboard
- Combined filters
- Next required action
- Alerts and exceptions
- Bulk actions
- Enrollment completion rules
- Exports

### Exit criteria

Staff can manage the complete annual enrollment population from one live operational dashboard.

## Stage 5 — Tuition configuration and packages

### Deliverables

- Annual division pricing
- Configurable tuition components
- Registration, tuition, dormitory, meals
- Optional and required components
- Student-specific package
- Proration
- Scholarships
- Credits and adjustments
- Payment schedules
- Calculation tests

### Exit criteria

Staff can set and reproduce each standard student's annual financial obligation.

## Stage 6 — Financial aid

### Deliverables

- Invite financial-aid application
- Native web form
- Save and resume
- Secure uploads
- Submission snapshots
- Review queue
- Requests for correction
- Decisions and award versions
- Integration with tuition package

### Exit criteria

Financial aid can be requested, submitted, reviewed, decided, and applied without PDFs or side spreadsheets.

## Stage 7 — Web tuition contracts

### Deliverables

- Versioned contract templates
- Contract creation from frozen tuition-package version
- Secure recipient link
- Review and acknowledgments
- Electronic acceptance/signature
- Exact immutable acceptance snapshot
- Amendment and supersession workflow
- Contract dashboard and reminders

### Exit criteria

A responsible party can sign a binding web contract and staff can reproduce exactly what was accepted.

## Stage 8 — Kollel compensation

### Deliverables

- Kollel program rules
- Compensation component configuration
- Participant package
- Monthly pay periods
- Credits and incentives
- Bonuses and special pay
- Deductions
- Proration
- Approval
- Payment status
- Reports and history

### Exit criteria

The Kollel office can calculate and approve monthly pay using configurable components.

## Stage 9 — Configuration Portal

Configuration is developed throughout earlier stages; this stage completes and consolidates it.

### Deliverables

- Academic years
- Divisions
- Program requirements
- Tuition components and schedules
- Kollel components
- Form wording and visibility
- Contract templates
- Email templates
- Status reasons
- Payment plans
- Workflow thresholds
- Configuration versioning and audit

### Exit criteria

Routine annual operational changes do not require code changes.

## Stage 10 — Reporting and operations

### Deliverables

- Enrollment reports
- Admissions reports
- Tuition and scholarship reports
- Financial-aid reports
- Dormitory and meal reports
- Contract reports
- Kollel pay reports
- Exception reports
- Saved filters
- Excel and CSV export
- Operational monitoring

## Stage 11 — Migration

### Deliverables

- Data inventory
- Mapping rules
- Import templates
- Duplicate detection
- Validation reports
- Trial imports
- Reconciliation
- Final migration
- Legacy archive

## Stage 12 — Pilot and launch

### Deliverables

- Security review
- User acceptance testing
- Performance testing
- Backup and recovery validation
- Training
- Pilot group
- Parallel operation where needed
- Production launch
- Post-launch issue process

## Later phases

- Admire integration
- Payment integration
- Expanded family portal
- Expanded student portal
- SMS
- Advanced analytics
- Additional academic modules

## Milestone rule

No stage should be implemented as one oversized task. Each stage must be divided into small tickets with explicit acceptance criteria, test cases, migration impact, and permission requirements.
