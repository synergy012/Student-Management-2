# System Bible — Volume 2
# Student Module

**Project:** Student Management 2  
**Status:** Draft for review  
**Version:** 0.1  
**Date:** July 14, 2026

---

## 1. Purpose

This volume defines the Student Module and the core records on which later modules depend. The Student Module is the permanent institutional record for each student and must support both current operations and long-term historical lookup.

It covers:

- Permanent Student records
- Institutional Student IDs and College Program IDs
- Personal, family, educational, medical, and contact information
- Households and sibling relationships
- Address history
- Applications and application history
- Annual enrollment-history foundations
- Attachments, communications, notes, and audit history
- Duplicate detection, merging, search, filtering, and permissions

Detailed tuition, financial aid, Kollel, dormitory, academic, HES reporting, and Admire integration requirements are deferred to later volumes.

---

## 2. Governing Principles

### 2.1 One Student, One Permanent Record

Each individual has one permanent Student record. It is reused if the student applies again, changes divisions, withdraws and returns, enters Kollel, or graduates.

### 2.2 Permanent Versus Annual Information

Permanent and currently authoritative identity information belongs to Student. Information that changes by academic year belongs to Enrollment or another year-specific record.

> The student is permanent; participation, workflow, and financial information are year-specific.

### 2.3 Two-Way Access

Annual information must be accessible:

- From the Student record as year-by-year history
- From the selected Academic Year as a live workflow and report

Both views use the same underlying records.

### 2.4 Operational Record and Institutional Archive

The Student record is both an operational record and an institutional archive. Some information may rarely drive workflow but must remain available for later lookup.

### 2.5 Immutable Submissions

Submitted Applications, submitted Financial Aid Applications, and signed Tuition Contracts are immutable snapshots. Staff may update live records, but may not silently change what was originally submitted or signed.

### 2.6 Native Web Workflows

Applications, financial aid forms, contracts, acknowledgments, and signatures are native web workflows. PDFs may be supporting attachments only.

---

## 3. Scope

### 3.1 Initial Student Module

- Authentication and permissions
- Student creation, editing, search, and filtering
- Institutional Student ID
- College Program ID
- Student Status
- Personal and family information
- Parent, spouse, in-law, and grandparent sections
- Home and mailing address history
- Household backend groundwork
- Sibling linking
- Secure attachments
- Communications history
- Internal notes
- Audit history
- Duplicate warnings and merge groundwork
- Current-student import
- Student timeline
- Applications section
- Annual-history display framework

### 3.2 Deferred

- Full Academic Module
- HES report generation
- Full College Program workflow
- Kollel compensation
- Dormitory administration
- Tuition and contracts
- Financial Aid workflow
- Detailed Enrollment Dashboard
- Native replacement of Gravity Forms
- Student/family portal
- Payment processing
- Admire integration

---

## 4. Core Domain Objects

### 4.1 Student

The permanent individual.

Required characteristics:

- Technical UUID
- Permanent readable institutional Student ID
- Optional College Program ID
- Current Student Status with preserved history
- Permanent/current personal information
- Historical addresses
- Applications
- Enrollments
- Optional Household membership
- Sibling relationships
- Attachments, notes, communications, and audit events

A Student is created only when:

- An Application is submitted
- Authorized staff manually creates the Student
- A current Student is imported

Starting an incomplete Application does not create a Student.

### 4.2 Household

Backend infrastructure used primarily to associate siblings.

Initial rules:

- No standalone Household portal is required.
- A Student may belong to one primary Household initially.
- Multiple Students may share a Household.
- Household membership must not overwrite Application-submitted family information.
- Parent and family information remains visible on each Student record.

### 4.3 Application

An Application belongs to a division and intended Academic Year.

Rules:

- A Student may have unlimited Applications.
- Different divisions always require separate Applications.
- Multiple same-division/year Applications are preserved.
- The latest displays by default.
- Earlier submissions remain reviewable.
- Duplicate submissions are flagged but not deleted or automatically superseded.
- Applications remain outside the annual Enrollment history.
- Enrollment is created only after acceptance.

Initial intake may continue through Gravity Forms. Gravity Forms is an intake channel; Student Management 2 owns the normalized Application record, immutable submission snapshot, attachments, duplicate review, Student linking, admissions workflow, and acceptance/rejection outcomes.

### 4.4 Academic Year

A configurable operating year.

Default: September 1 through August 31.

Initial statuses:

- Enrollment Open
- Active
- Closing
- Locked
- Archived

Multiple years may operate simultaneously. Years are locked manually. Authorized users may edit historical or locked-year data, with full audit history.

### 4.5 Enrollment

The annual relationship between Student and Academic Year.

Created only:

- After Application acceptance
- Through annual carry-forward/pro forma

A Student usually has one Enrollment per Academic Year. Midyear division changes use dated Enrollment Segments.

Minimum foundation fields:

- Student
- Academic Year
- New or Returning
- Annual participation status
- Financial Aid Required
- Financial Aid Status
- FAFSA Eligible
- Tuition Contract Status
- College Program Status
- Next Action
- Internal Notes
- Enrollment Complete indicator

### 4.6 Enrollment Segment

An effective-dated division assignment inside an Enrollment.

Fields:

- Enrollment
- Division
- Effective Start Date
- Effective End Date
- Segment Status
- Change Reason
- Notes

Division reports and filters must respect the applicable segment dates.

---

## 5. Identifiers

### 5.1 Technical UUID

Used internally by the software.

### 5.2 Institutional Student ID

Recommended format: `STU-000123`

Rules:

- Automatically generated
- Unique
- Never reused
- Never changes with division
- Searchable and exportable
- Used in imports and integrations
- Staff-facing
- Format configurable

### 5.3 College Program ID

Optional HES Consulting identifier.

Rules:

- Unique when present
- Searchable and exportable
- Permission-restricted
- Audited
- Independent from institutional Student ID
- Duplicate values blocked or prominently flagged

Future HES reports will use this field.

---

## 6. Student Status

Initial values:

- Applicant
- Current
- Withdrawn
- Graduated
- Rejected Applicant

Additional values may be configured.

Rules:

- Applicant is a Student Status.
- Rejected Applicants remain stored but are hidden from ordinary lists.
- They remain accessible through explicit search/filter.
- A withdrawn or rejected Student may later return using the same record.
- Graduated is sufficient; no separate Alumnus status is required now.
- Every change is audited and historical status is preserved.

---

## 7. Names and Search

### 7.1 Stored name fields

- Legal First Name
- Legal Middle Name
- Legal Last Name
- Informal Name
- Hebrew Name

Legal name is primary. Other names are searchable.

### 7.2 Display preference

Default: `Last, First Middle`

Each staff user may choose their display preference, including:

- Last, First Middle
- First Middle Last
- First Last

### 7.3 Search fields

- Legal, informal, and Hebrew names
- Institutional Student ID
- College Program ID
- Email
- Phone
- Parent names
- Address
- Sibling
- Application identifier

---

## 8. Duplicate Detection

Duplicate matching is advisory only. Staff performs final review.

Signals may include:

- Similar legal name
- Same date of birth
- Email
- Phone
- Parent names
- Address
- College Program ID
- SSN when present

Foreign Students may not have an SSN.

Staff actions:

- Confirm separate Students
- Link Application to existing Student
- Mark for merge review
- Merge with elevated permission

---

## 9. Student Record Sections

1. Overview
2. Personal Information
3. Contact and Addresses
4. Parents
5. Spouse and In-Laws
6. Grandparents
7. Education
8. Learning Background
9. College
10. Employment and Activities
11. Medical
12. Siblings
13. Applications
14. Annual History
15. Attachments
16. Communications
17. Notes
18. Audit History

Unauthorized sections are hidden entirely.

---

## 10. Overview

Recommended current summary:

- Legal Name
- Institutional Student ID
- College Program ID when authorized
- Student Status
- Selected/active Academic Year
- Current Division
- Returning Status
- Enrollment Complete
- Financial Aid Status when authorized
- Net Tuition when authorized
- Admire Status when authorized
- Tuition Contract Status when authorized
- College Program Status when authorized
- Next Action
- Assigned Staff Member
- Alerts

Annual values are derived from annual records, not duplicated on Student.

---

## 11. Personal Information

Potential fields:

- Legal names
- Informal and Hebrew names
- Date of Birth
- Social Security Number
- Citizenship
- Marital Status
- Phone
- Email
- Additional contact information
- General notes

All relevant fields from the existing Application should be supported, even if they are mainly archival.

---

## 12. Social Security Number

A full SSN may be stored.

Requirements:

- Optional
- Foreign Students may have none
- Encrypted at rest
- Masked by default
- Full value requires explicit permission
- Full-value access is audited
- Never included in ordinary exports
- Never logged or placed in URLs
- Never used as the only duplicate key
- Separate permission may allow last-four-only viewing

---

## 13. Addresses

Initial address types:

- Home Address
- Mailing Address, if different

Future local address comes from Dormitory assignment.

Fields:

- Address Line 1
- Address Line 2
- City
- State/Province
- Postal Code
- Country
- Effective Start Date
- Effective End Date
- Current Indicator
- Source
- Notes

When updated:

- New address becomes current
- Prior address remains historical
- Change is audited
- Application-submitted address is compared but does not silently overwrite live Student data

---

## 14. Parents

Father and mother are separate structured sections.

### 14.1 Identity

- Title
- Legal First Name
- Legal Last Name

### 14.2 Life Status

- Alive
- Deceased
- Unknown

### 14.3 Relationship Status

For living/unknown parents:

- Married to Other Parent
- Widowed
- Divorced
- Remarried
- Unknown

### 14.4 Deceased Override

When Life Status = Deceased:

- Relationship Status becomes not applicable
- Only name is required
- Phone, email, occupation, addresses, and other current information are hidden/disabled
- Existing historical information remains in audit history
- Historical Application snapshots remain unchanged

### 14.5 Living Parent Fields

- Phone
- Email
- Occupation
- Home Address
- Mailing Address, if different
- Notes

---

## 15. Grandparents

Two combined sets:

- Maternal Grandparents
- Paternal Grandparents

Separate grandfather and grandmother names are not required.

Fields:

- Names/family designation
- Living Status
- Phone
- Email
- Home Address
- Mailing Address, if different
- Notes

Suggested Living Status:

- Both Living
- One Living
- Neither Living
- Unknown

---

## 16. Spouse

Full contact information:

- Legal Name
- Informal Name
- Phone
- Email
- Date of Birth if collected
- Home Address
- Mailing Address, if different
- Occupation
- Notes

A spouse is not automatically a separate Student.

---

## 17. In-Laws

Stored as one combined set.

Fields:

- Names
- Phone
- Email
- Home Address
- Mailing Address, if different
- Living Status
- Notes

---

## 18. Education

Repeatable structured educational records should include:

- Institution
- Institution Type
- Dates Attended
- Graduation Status
- Diploma information
- Notes
- Attachments

Potential institution types include High School, Seminary/Yeshiva, College, and Other.

---

## 19. Learning Background

Preserve Application information such as:

- Last Rebbe Name
- Last Rebbe Phone
- Gemara Sedorim Daily Count
- Gemara Seder Length
- Learning Evaluation
- References
- Additional Notes

---

## 20. College Information

Initial fields:

- College Attending
- College Name
- Major
- Expected Graduation
- College Program ID
- College Program Notes

Future College Program/HES milestone:

- Semester status
- Fall/Spring levels
- Eligibility
- Program entry/completion
- HES validation
- HES exports and export history

---

## 21. Employment and Activities

Potential fields:

- Past Jobs
- Current Employment
- Summer Activities
- Other Activities
- Notes

---

## 22. Medical

All optional and permission-restricted:

- Medical Conditions
- Allergies
- Medications
- Blood Type
- Insurance Company
- Member/Policy Number
- Primary Physician
- Physician Phone
- Emergency Instructions
- Additional Notes

Unauthorized users cannot see that the section contains data. Access and changes should be auditable.

---

## 23. Households and Siblings

Authorized staff may:

1. Search for another Student
2. Select Link as Sibling
3. Confirm/create shared Household
4. See the relationship on both records

Possible sibling suggestions may use:

- Parent names
- Parent phone/email
- Home address
- Existing Household

Suggestions are never automatic.

Sibling display may include:

- Name
- Student ID
- Status
- Current Division
- Link subject to permission

---

## 24. Applications on Student

Applications appear in a separate permanent section.

List fields:

- Application ID
- Division
- Intended Academic Year
- Submission Date
- Status
- Duplicate Warning
- Decision
- Latest indicator
- Link to immutable submission
- Attachments

Initial rules:

- Division-specific Gravity Forms URLs
- Current/upcoming year selectable
- Draft/resume initially handled by Gravity Forms
- Submission creates Student and Application
- Enrollment created only after acceptance
- Staff resolves possible existing-Student matches
- Latest same-division/year Application displayed by default

---

## 25. Acceptance and Rejection

On acceptance:

1. Application becomes Accepted
2. Student Status becomes Current
3. Enrollment is created
4. Enrollment enters annual workflow
5. Configured next actions are generated
6. Acceptance email may send automatically if configured

On rejection:

- Student Status becomes Rejected Applicant
- Record is hidden from ordinary lists
- It remains searchable
- Future reapplication reuses the same Student after match confirmation

---

## 26. Annual History

Student page provides:

- Chronological timeline
- Selected-year detail
- Optional side-by-side year comparison

Timeline events may include:

- Student creation
- Application submission/decision
- Enrollment creation
- Returning decision
- Division change
- Financial aid activity
- Tuition/Admire changes
- Contract activity
- Enrollment completion
- Withdrawal
- Graduation
- Kollel transition
- Significant notes/corrections

Applications remain a separate history area even though they reference division and intended year.

---

## 27. Enrollment Workflow Foundation

Enrollment uses multiple workflow tracks, not one overloaded status.

Initial tracks:

- Returning Decision
- Financial Aid
- Tuition
- Admire
- Tuition Contract
- College Program
- Enrollment Completion

### Returning Decision

- Not Applicable
- Pending Decision
- Returning
- Not Returning

### Enrollment Complete

Calculated separately from annual participation status.

Authorized staff may waive individual requirements. A waiver counts as complete and requires:

- Permission
- User
- Timestamp
- Reason
- Prior and new status

### Next Action

Calculated automatically, but authorized staff may override it. Overrides are audited.

---

## 28. Academic Year Pro Forma Foundation

When preparing a new year:

- Include prior-year Students except withdrawn Students
- Graduated exclusion configurable
- Default proposed status: Pending Decision
- Staff may mark Returning, Not Returning, Deferred, or Excluded
- Pro Forma remains usable after year creation for late additions

Year creation page may copy and allow editing of:

- Divisions
- Tuition components and amounts
- Registration fees
- Dormitory and meal options
- Kollel pay components
- Contract wording
- Application and Financial Aid settings
- Email templates
- Payment plans
- Workflow requirements

---

## 29. Attachments

May link to Student, Application, Enrollment, Medical, Notes, and later financial records.

Requirements:

- Secure storage
- Permission inheritance
- No public guessable URLs
- File-type and size validation
- Malware scanning where available
- Original filename preserved
- Internal generated storage name
- Upload user/time recorded
- Sensitive access auditable

PDFs are attachments only.

---

## 30. Communications

Initial types:

- Application confirmation
- Staff new-Application alert
- Request for information
- Acceptance
- Rejection
- Financial Aid invitation
- Contract link
- Reminder
- Enrollment confirmation
- Manually logged correspondence

The organization uses Google Workspace.

Each message records:

- Recipient
- Subject
- Template version
- Related record
- Initiating staff/automation
- Sent time
- Delivery result

Infrastructure credentials remain deployment secrets.

---

## 31. Internal Notes

Notes support:

- Category
- Department
- Author
- Created Date
- Optional Academic Year
- Follow-Up Date
- Assigned Staff Member
- Attachments
- Pinning
- Restricted Visibility
- Resolved Status

Edits preserve full version history. Deletion should ordinarily be replaced by archive/retraction.

---

## 32. Audit History

Audit fields:

- Actor
- Timestamp
- Object Type/ID
- Action
- Previous Value
- New Value
- Reason
- Related Academic Year
- Request metadata where appropriate

Audited actions include:

- Student creation
- Status/identifier changes
- SSN access or change
- Address changes
- Parent status changes
- Medical access/change
- Sibling links
- Application linking
- Enrollment creation
- Division segment changes
- Waivers
- Merge/archive/delete
- Permission changes

---

## 33. Permissions

Potential granular permissions:

- View/Create/Edit Students
- View Rejected Applicants
- View Archived Students
- View SSN Last Four
- View Full SSN
- View/Edit Medical
- View/Edit College Program ID
- View Financial Information
- View Financial Aid
- View Kollel Compensation
- View Internal/Restricted Notes
- View Audit History
- Link Siblings
- Merge Students
- Archive Students
- Permanently Delete Test Records
- Edit Locked-Year Data
- Waive Workflow Requirements

Permissions are enforced server-side.

---

## 34. Archive, Merge, and Delete

Ordinary users cannot permanently delete Students.

### Archive

Archive/hide from operational views.

### Merge

Authorized administrators may merge duplicates while preserving:

- Original identifiers
- Applications
- Enrollments
- Attachments
- Communications
- Notes
- Address history
- Audit history
- Source lineage

The source record is marked merged and archived.

### Permanent Delete

Only for test or clearly erroneous records with no valid immutable submissions.

Requires elevated permission, reason, confirmation, and audit event.

---

## 35. Current-Student Import

Initial production import covers current Students.

Workflow:

1. Upload Excel/CSV
2. Map columns
3. Validate
4. Detect possible duplicates
5. Preview
6. Correct/exclude invalid rows
7. Import
8. Produce success/error report
9. Preserve source file and import audit

No silent overwrites.

---

## 36. UI Requirements

### Student List

Default columns:

- Name
- Student ID
- Student Status
- Current Division
- Current Academic Year
- College Program ID when authorized
- Next Action
- Alerts

Capabilities:

- Search, sort, filters, saved filters
- Configurable columns
- Permission-aware export
- Rejected/archived hidden by default

### Student Profile

Header:

- Legal Name
- Student ID
- Status
- Current Division
- Alerts
- Primary actions

Navigation follows the approved Student sections.

### Editing

- Section-based
- Validated before save
- Unsaved-change warning
- Permission-aware
- Audit reason where required
- Immutable submissions never overwritten

---

## 37. Core Validation Rules

- Institutional Student ID unique
- College Program ID unique when present
- SSN format validated when present
- Deceased parent requires name only
- Deceased parent cannot have active Relationship Status
- Only one current Home Address
- Enrollment segments cannot overlap without approved exception
- Sibling cannot link to self
- Merge cannot discard immutable submissions
- Unauthorized API access to sensitive fields is denied

---

## 38. Acceptance Criteria

### Student Creation

- Manual, submitted-Application, and import creation paths work
- UUID and institutional ID assigned
- Creation audited

### Search

- Searches names, IDs, contact information, parent names
- Rejected Applicants hidden by default
- Sensitive data never leaked

### Parent Rules

- Independent Life and Relationship Status per parent
- Deceased override hides non-name fields
- History preserved and audited

### Siblings

- Manual sibling link appears both ways
- Shared Household created/used
- Suggestions require confirmation

### Addresses

- Home/Mailing supported
- Prior address retained
- Application data does not silently overwrite current address

### Applications

- Multiple submissions preserved
- Same-division/year warning
- Latest displayed by default
- Enrollment only after acceptance
- Submission immutable

### Annual History

- Timeline and year detail work
- Uses same annual records as workflow views
- Authorized comparison supported

### Security

- SSN masked and audited
- Medical hidden without permission
- Backend authorization matches UI
- Sensitive attachments restricted

### Notes

- Supports category, assignment, follow-up, restriction, pinning, resolution
- Revisions preserved

### Merge

- All linked records preserved
- Source lineage maintained
- Merge audited

---

## 39. Initial Build Tickets

- **SM-001:** Student schema and identifiers
- **SM-002:** Authentication and Student permissions
- **SM-003:** Student list and search
- **SM-004:** Student profile shell
- **SM-005:** Personal/contact information and SSN security
- **SM-006:** Address history
- **SM-007:** Parent sections and deceased override
- **SM-008:** Spouse, in-laws, grandparents
- **SM-009:** Education, learning, college, employment
- **SM-010:** Medical information
- **SM-011:** Household and siblings
- **SM-012:** Secure attachments
- **SM-013:** Internal notes
- **SM-014:** Communications history
- **SM-015:** Application model and Gravity Forms adapter contract
- **SM-016:** Academic Year, Enrollment, Segment, and history foundation
- **SM-017:** Duplicate review and merge
- **SM-018:** Current-student import

---

## 40. Open Items for Later Volumes

- Exact Gravity Forms field mapping
- Exact division Application questions
- Tuition rules
- Financial Aid workflow
- Admire integration
- Responsible-party contract allocations
- Kollel pay formulas
- Dorm assignments
- HES report formats
- Final technical stack and encryption provider
- Final workflow decision tables

---

## 41. Final Rule

> The Student record must remain useful after decades of Applications, division changes, withdrawals, reenrollments, financial changes, and graduation. Current information must be easy to work with, historical information must never be lost, and sensitive information must never be exposed without explicit permission.
