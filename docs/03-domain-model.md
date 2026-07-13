# Initial Domain Model

This document defines the high-level business objects. It is not yet the final database schema.

## 1. Student

Represents the permanent individual.

Contains current authoritative identity and contact information. It must not contain annual tuition, annual division, annual contract, or annual financial-aid workflow fields.

Relationships:

- Has many Applications
- Has many Enrollments
- May belong to one or more Households through relationships
- Has many Communications, Tasks, Attachments, and Audit Events

## 2. Household

Represents a family or financial household.

Supports multiple students and multiple adults. It should not assume that every responsible party is a parent.

## 3. Academic Year

Represents an operating year with start/end dates, enrollment dates, status, and configuration version.

States may include:

- Planning
- Open
- Active
- Closing
- Locked
- Archived

## 4. Application

Represents an application for a particular academic year and intended division/program.

An application has:

- Current workflow state
- One or more immutable submission snapshots
- Review history
- Decision
- Links to requested corrections and communications

Submitting an application creates or links a Student record.

## 5. Enrollment

The central annual relationship between Student and Academic Year.

Contains annual participation information such as:

- Division
- Program type
- New or returning
- Enrollment status
- Returning decision
- Start/end dates
- Dormitory selection
- Meal-plan selection
- Registration completion
- Workflow state
- Completion state

A Student may have at most one primary Enrollment per academic year unless a documented exception supports multiple concurrent programs.

## 6. Financial Aid Application

Year-specific application connected to Enrollment.

Contains versioned submissions, uploaded evidence, review status, and requested information.

## 7. Financial Aid Decision

One or more review decisions connected to the Financial Aid Application and Enrollment.

Contains award amount or method, decision date, approver, conditions, notes, and supersession history.

## 8. Tuition Schedule

Defines standard annual pricing by academic year and division/program.

## 9. Tuition Component

Configurable component type, such as:

- Tuition
- Registration fee
- Dormitory
- Meals
- Other fee

Defines whether it is required, optional, proratable, taxable if relevant, and applicable to selected programs.

## 10. Student Tuition Package

The individual annual package connected to Enrollment.

Contains component lines, standard amounts, approved overrides, scholarships, proration, credits, additions, net obligation, and payment schedule.

The calculation must be reproducible from stored line items and rules.

## 11. Tuition Contract

Versioned agreement connected to Enrollment and a specific Tuition Package version.

Contains:

- Exact displayed terms
- Exact component and payment values
- Template version
- Status history
- Signer(s)
- Acceptance evidence
- Immutable signed snapshot
- Supersession or amendment links

## 12. Program Type

Defines workflow requirements rather than relying on hard-coded division checks.

Examples:

- Standard Student
- Kollel

Configurable flags may include:

- Requires tuition
- Requires tuition contract
- Allows financial aid
- Uses Kollel compensation
- Requires dormitory decision
- Requires meal-plan decision

## 13. Kollel Compensation Package

Annual or effective-dated compensation package connected to Enrollment.

Contains applicable recurring components and special eligibility settings.

## 14. Kollel Pay Component

Configurable positive or negative compensation element.

Examples may include:

- Base stipend
- Credit-based incentive
- Program bonus
- Chaburah pay
- Housing allowance
- Special pay
- Missed-time deduction
- Other deduction

Names and rules must be configurable.

## 15. Kollel Pay Period

Defines a monthly or other payment period.

## 16. Kollel Payment Calculation

Stores the frozen calculation for a participant and pay period, including inputs, component lines, approvals, payment status, and adjustments.

## 17. Task

Represents assigned work with owner, due date, status, waiting-on party, and related object.

## 18. Communication

Records every system-generated or manually logged communication, recipients, template version, delivery status, and related object.

## 19. Attachment

Stores secure supporting files linked to Student, Application, Financial Aid Application, Enrollment, or another permitted object.

## 20. Configuration Version

Stores versioned business configuration by academic year or effective date.

## 21. Audit Event

Records consequential changes:

- Actor
- Timestamp
- Object type and identifier
- Action
- Previous value
- New value
- Reason
- Request metadata where appropriate

## Access pattern requirement

Every annual record must be accessible:

- From the Student's chronological and academic-year history
- From the selected academic-year workflow and reports

These must be two views of the same underlying record, never separate copies.
