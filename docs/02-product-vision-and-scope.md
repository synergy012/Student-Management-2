# Product Vision and Scope

## 1. Users

Initial internal users:

- System Administrator
- Admissions Staff
- Registrar
- Tuition/Finance Staff
- Financial Aid Reviewer
- Kollel Administrator
- Read-Only Staff

External users:

- Applicant
- Student
- Parent, guardian, spouse, or financial responsible party

Future roles may include faculty, committee members, and external auditors.

## 2. Major modules

### Student Record

One permanent record containing authoritative personal information and access to complete history.

### Application Portal

Native web application that creates or links a Student record upon submission and enters the applicant into admissions review.

### Admissions Review

Supports review, requests for information, acceptance, rejection, waitlist, and financial-aid invitation.

### Annual Enrollment

Includes prior-year students and newly accepted students for the selected year. Supports returning, not returning, deferred, withdrawn, and complete outcomes.

### Live Enrollment Dashboard

The primary operational workspace. Supports filters, search, next action, status, alerts, bulk actions, exports, and academic-year selection.

### Tuition Configuration

Defines annual division pricing and configurable components, including registration, dormitory, meals, and tuition.

### Student Tuition Package

Stores the individual annual financial package, including defaults, proration, scholarship, credits, adjustments, and payment schedule.

### Tuition Contract

A versioned native web contract with exact financial terms, acknowledgments, signer identity, timestamp, and immutable acceptance snapshot.

### Financial Aid

A year-specific native web application, supporting secure uploads, review, decisions, award history, and transfer of approved awards to tuition.

### Kollel

Handles Kollel enrollment and replaces tuition workflow with compensation workflow.

### Kollel Compensation

Configurable pay components, effective dates, monthly calculations, adjustments, deductions, approvals, payment status, and history.

### Configuration Portal

Manages academic years, divisions, program requirements, tuition components, Kollel components, forms, templates, statuses, payment plans, workflow rules, and lookup values.

### Communications

Email templates, secure links, reminders, bulk messages, and complete communication history.

### Tasks and Follow-Up

Assignments, due dates, waiting-on status, reminders, internal notes, and work queues.

### Reporting

Enrollment, admissions, tuition, scholarships, financial aid, dormitory, meals, contracts, Kollel pay, exceptions, and exports.

### Audit and Security

Permissions, sensitive-data restrictions, complete change history, soft deletion, locking, and access logs where appropriate.

## 3. MVP scope

The first operational release must include:

- Authentication and permissions
- Academic-year management
- Student records and annual history
- Application Portal
- Admissions review
- Annual enrollment creation
- Live enrollment dashboard
- Tuition configuration and student tuition packages
- Financial-aid invitation and web application
- Tuition contracts with web acceptance
- Kollel classification and basic compensation setup
- Configuration Portal
- Communications and reminders
- Audit history
- CSV/Excel import and export

## 4. Explicit non-goals for the initial release

- PDF generation or PDF form filling
- Dropbox Sign document workflows
- Full accounting general ledger
- Payment processing
- Full learning management system
- Gradebook
- Parent portal beyond required forms and signatures
- Student portal beyond required forms and status
- Fully generic drag-and-drop form builder
- Real-time Admire integration

## 5. Native web-form rule

Applications, financial-aid applications, tuition contracts, acknowledgments, signatures, corrections, and amendments must be native web workflows.

Supporting files may be uploaded. A PDF may be accepted as evidence, but the system may not treat a PDF as the form, contract, workflow, or authoritative structured record.

## 6. Configuration rule

Items expected to vary between academic years should be configurable. This includes amounts, components, wording, deadlines, applicable divisions, payment schedules, and workflow requirements.

Configuration must not expose infrastructure secrets or allow ordinary administrators to edit deployment credentials.
