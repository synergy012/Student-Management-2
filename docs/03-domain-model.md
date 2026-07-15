# Domain Model
## Student Management 2

**Version:** 1.0 Draft  
**Status:** Living Architecture Document

---

# 1. Purpose

This document defines the core business objects, relationships, ownership rules, lifecycle rules, effective-dating rules, and invariants for Student Management 2.

It is the primary reference for:

- Database design
- API design
- Business logic
- Workflow implementation
- Permissions
- Audit behavior
- Module specifications
- AI coding agents

This document must be read together with:

- `docs/02-system-constitution.md`
- `docs/modules/02-Student-Module.md`
- Future module specifications

If implementation behavior conflicts with this document, the implementation is incorrect unless this document is intentionally revised.

---

# 2. Core Architectural Principle

> The Student is permanent. Participation, workflows, and financial activity are annual or effective-dated.

The system must support both:

1. A permanent Student-centered history
2. Academic-year and workflow-centered operational views

These views must use the same underlying records.

The system must not maintain separate copies of the same information for the Student view and the academic-year workflow view.

---

# 3. Domain Areas

The system is divided into the following business domains.

## 3.1 Core Identity Domain

- Student
- Student Identifier
- Student Status
- Household
- Household Membership
- Parent Information
- Address
- Spouse
- In-Laws
- Grandparents

## 3.2 Application and Admissions Domain

- Application
- Application Submission
- Application Attachment
- Admissions Decision
- Duplicate Candidate

## 3.3 Academic-Year and Enrollment Domain

- Academic Year
- Division
- Division-Year Configuration
- Enrollment
- Enrollment Segment
- Enrollment Workflow
- Enrollment Requirement
- Requirement Waiver

## 3.4 Financial Aid Domain

- Student Financial Aid Profile
- Financial Aid Submission
- Enrollment Financial Aid Requirement
- Financial Aid Review

## 3.5 Tuition and Contract Domain

- Tuition Component
- Division Tuition Schedule
- Tuition Package
- Tuition Package Version
- Tuition Package Line
- Responsibility Allocation
- Household Payer
- External Responsible Party
- Admire Status
- Tuition Contract
- Contract Acceptance
- Signature Waiver

## 3.6 Kollel Domain

- Kollel Enrollment State
- Kollel Time Sheet
- Monthly Kollel Calculation
- Kollel Calculation Revision
- Kollel Credit
- Kollel Bonus
- Kollel Deduction
- Kollel Override

## 3.7 Supporting Operations Domain

- Note
- Task
- Communication
- Attachment
- Related Object Link
- Audit Event
- Configuration Item
- Configuration Version

## 3.8 Security Domain

- User
- Role
- Permission
- User Preference
- Sensitive Data Access Event

---

---

# 4. Domain Relationship Overview

The Student is the root entity of the system.

All operational workflows ultimately relate back to a Student but generally belong to an Academic Year or another effective-dated entity.

```text
Student
│
├── Student Identifiers
├── Parent Information
├── Household Memberships
│      └── Household
├── Applications
│      └── Application Submission
├── Enrollments
│      └── Enrollment Segments
├── Student Financial Aid Profile
├── Notes
├── Tasks
├── Communications
├── Attachments
└── Audit Events
```

Future sections of this document expand Enrollment into:

- Tuition
- Contracts
- Financial Aid
- Dormitory
- Meal Plans
- College Program
- Kollel

---

# 5. Student

## 5.1 Purpose

The Student represents one permanent individual.

A Student is the highest-level business entity in the system.

Every other major operational object ultimately references a Student.

Students may:

- Apply multiple times
- Attend different divisions
- Withdraw
- Return years later
- Graduate
- Become Kollel members
- Change addresses
- Change legal names
- Submit multiple Applications

None of these actions create a new Student.

---

## 5.2 Student Creation

A Student may be created by:

- Application submission
- Manual staff creation
- Data migration
- Future API integrations

Beginning an unfinished Application does **not** create a Student.

Only submission creates the permanent record.

---

## 5.3 Student Identity

Every Student contains three identifiers.

### Technical Identifier

A UUID used internally.

Properties:

- Never changes
- Never exposed to users
- Used by APIs
- Used by database relationships

---

### Institutional Student ID

Primary institutional identifier.

Properties:

- Permanent
- Unique
- Human readable
- Searchable
- Never reused
- Never changes because of division changes

Suggested format:

```
STU-000123
```

The format should eventually be configurable.

---

### College Program ID

Optional.

Used for future HES Consulting integration.

Properties:

- Unique
- Searchable
- Permission restricted
- Audited
- Independent of Student ID

---

## 5.4 Student-Owned Information

The Student owns permanent institutional information.

Examples include:

### Identity

- Legal Name
- Informal Name
- Hebrew Name
- Date of Birth
- Citizenship
- Gender
- SSN

### Contact

- Current Address
- Mailing Address
- Phone Numbers
- Email Addresses

### Family

- Parents
- Household Memberships
- Spouse
- In-Laws
- Grandparents
- Siblings

### Education

- High School
- Graduation Status
- College
- FAFSA Eligibility
- Academic History

### Medical

- Medical Information
- Allergies
- Emergency Information

### Administrative

- Student Status
- Notes
- Attachments
- Communications
- Audit History

---

## 5.5 Information NOT Owned by Student

The Student intentionally does **not** own annual operational information.

These belong elsewhere.

Examples:

- Current Division
- Tuition
- Scholarships
- Financial Aid workflow
- Tuition Contracts
- Dorm Assignment
- Meal Plan
- Admire Status
- College Program workflow
- Enrollment workflow

The Student page may display summaries of these values.

The authoritative records live elsewhere.

---

## 5.6 Student Status

Student Status describes the individual's overall relationship with the institution.

Initial statuses:

- Applicant
- Current
- Withdrawn
- Graduated
- Rejected Applicant

Additional statuses are configurable.

Student Status is **not** tied to an Academic Year.

---

## 5.7 Rejected Applicants

Rejected Applicants remain institutional records.

They:

- Keep all Applications
- Keep all history
- Are hidden from ordinary operational screens
- Remain searchable
- May later become Applicants or Current Students

The original Student record is always reused.

---

## 5.8 Student Invariants

The following rules must never be violated.

A Student:

- Has exactly one UUID.
- Has exactly one Institutional Student ID.
- May have zero or one College Program ID.
- Never receives a new Student ID.
- Is never recreated because of a new Application.
- May belong to multiple Households.
- May submit multiple Applications.
- May have one Enrollment per Academic Year.
- Must preserve all historical records.
- Is never physically deleted during ordinary operations.

---

## 5.9 Student Lifecycle

```
Created
    │
Applicant
    │
 ├──────────────┐
 │              │
Accepted    Rejected
 │              │
 │              │
Current   Rejected Applicant
 │
 ├──────────────┐
 │              │
Graduated   Withdrawn
                │
                └──────► Current
```

---

## 5.10 Student Domain Events

Examples:

- StudentCreated
- StudentMerged
- StudentRestored
- StudentStatusChanged
- StudentIdentifierAssigned
- CollegeProgramAssigned
- AddressChanged
- ParentUpdated
- HouseholdLinked
- HouseholdRemoved

---

## 5.11 AI Implementation Notes

Preferred primary key:

- UUID

Preferred aggregate root:

- Student

Never:

- Cascade delete
- Overwrite history
- Store annual workflow values directly on Student

Always:

- Soft delete
- Preserve merge lineage
- Audit significant changes
- Enforce permissions in backend
---

# 6. Student Names

## 6.1 Purpose

The Student record stores multiple forms of a person's name while maintaining one authoritative legal identity.

The legal name is used for all official records.

Alternate names exist to improve communication and searchability.

---

## 6.2 Name Fields

The Student supports:

### Legal Name

- Legal First Name
- Legal Middle Name
- Legal Last Name

This is the official institutional name.

---

### Alternate Names

Optional fields include:

- Informal / Preferred Name
- Hebrew Name
- Former Legal Name(s)
- Other Known Name(s)

Alternate names improve searching but never replace the legal name.

---

## 6.3 Display Preferences

Default display:

```
Last, First Middle
```

Each User may choose a preferred display format.

Examples:

- Last, First
- Last, First Middle
- First Last
- First Middle Last

Changing display preferences never modifies stored name fields.

---

## 6.4 Search

Student search should match:

- Legal Name
- Preferred Name
- Hebrew Name
- Former Names
- Student ID
- College Program ID
- Parent Names
- Email
- Phone

---

## 6.5 Invariants

The legal name is always authoritative.

Alternate names:

- never replace legal names
- remain searchable
- remain historical

---

# 7. Social Security Number

## 7.1 Purpose

The system may securely store a Social Security Number.

Foreign students may not have one.

---

## 7.2 Rules

SSN is:

- Optional
- Encrypted
- Permission protected
- Audited
- Never displayed by default

Users without permission should only see:

```
***-**-1234
```

or no value at all depending on permission.

---

## 7.3 Security

The full SSN:

- must never appear in logs
- must never appear in URLs
- must never be exported without permission
- must never be searchable by unauthorized users

---

## 7.4 Duplicate Detection

SSN may contribute to duplicate detection.

It must never be the sole matching criterion.

---

# 8. Household

## 8.1 Purpose

Household is the backend representation of a family unit.

Its primary purposes are:

- sibling relationships
- tuition responsibility
- future parent portal support
- future household reporting

The initial application will expose very little Household UI.

---

## 8.2 Multiple Household Membership

A Student may belong to multiple Households simultaneously.

Typical example:

```
Father Household

Mother Household
```

after divorce.

This is intentional.

---

## 8.3 Household Ownership

A Household is derived from Parent information.

It does not maintain independent copies of:

- addresses
- phones
- email

These are shared records.

Editing Parent information or Household information updates the same underlying data.

---

## 8.4 Household Membership

Household Membership is the join entity.

Suggested fields:

- Student
- Household
- Membership Type
- Effective Start
- Effective End
- Primary Household
- Notes

---

## 8.5 Household Types

Examples:

- Joint Parents
- Father's Household
- Mother's Household
- Guardian
- Other

Future versions may allow additional configurable types.

---

## 8.6 Siblings

Students become siblings only after staff confirmation.

The system may suggest siblings based on:

- parent names
- phone
- email
- address
- existing Household

Suggestions are never accepted automatically.

---

## 8.7 Household Invariants

A Household:

- contains one or more Students
- may exist without tuition responsibility
- may be responsible for multiple Students
- may overlap another Household

A Student:

- may belong to multiple Households
- may never be automatically linked as a sibling

---

## 8.8 AI Implementation Notes

Use a many-to-many relationship.

Never place a single household_id on Student.

---

# 9. Parent Information

## 9.1 Purpose

Parent information is embedded within the Student.

Parents are **not** reusable Person entities.

This keeps the model simple while preserving all required institutional history.

---

## 9.2 Parent Records

Each Student has:

- Father
- Mother

Each parent is managed independently.

---

## 9.3 Parent Identity

Fields include:

- Name
- Phone
- Email
- Occupation
- Home Address
- Mailing Address
- Notes

---

## 9.4 Life Status

Choices:

- Alive
- Deceased
- Unknown

Each parent has an independent value.

---

## 9.5 Relationship Status

Available only when the parent is Alive or Unknown.

Choices:

- Married to Other Parent
- Widowed
- Divorced
- Remarried
- Unknown

---

## 9.6 Deceased Override

When Life Status = Deceased:

Relationship Status becomes unavailable.

Only the parent's name is required.

Other fields become unnecessary:

- phone
- email
- occupation
- addresses

Historical values remain in Audit History.

Submitted Applications remain unchanged.

---

## 9.7 Parent Addresses

Parents support:

- Home Address
- Mailing Address

These use the shared Address object.

No duplicate copies should exist.

---

## 9.8 Parent Invariants

Each parent:

- has independent life status
- has independent relationship status
- may have different addresses
- shares underlying Address records with Household

---

# 10. Address

## 10.1 Purpose

Addresses are reusable value objects.

Current supported types:

- Home
- Mailing

Future:

- Dormitory
- Local
- Business

---

## 10.2 Effective Dating

Addresses maintain history.

Fields include:

- Effective Start
- Effective End
- Current Indicator
- Source
- Notes

Only one Home Address may be current for a given owner.

---

## 10.3 Address Updates

When an address changes:

1. create a new current Address
2. preserve the previous Address
3. create an Audit Event

Applications never overwrite live Address records automatically.

Staff chooses whether to accept submitted address changes.

---

## 10.4 Address Ownership

Addresses may belong to:

- Student
- Parent
- Household
- Spouse
- In-Laws
- Grandparents
- External Responsible Party

The same Address record may be shared where appropriate.

---

## 10.5 Address Invariants

Address history is permanent.

Only one current Home Address exists per owner.

Dormitory addresses will later be derived from Dormitory Assignments.

---

# 11. Spouse, In-Laws, and Grandparents

## 11.1 Spouse

The Student may have a spouse.

Fields include:

- Name
- Phone
- Email
- Occupation
- Home Address
- Mailing Address

The spouse is not automatically a Student.

---

## 11.2 In-Laws

One combined In-Law record stores:

- Names
- Phone
- Email
- Addresses
- Living Status
- Notes

Separate father-in-law and mother-in-law entities are unnecessary.

---

## 11.3 Grandparents

Two records exist:

- Maternal Grandparents
- Paternal Grandparents

Each contains:

- Family Name(s)
- Living Status
- Phone
- Email
- Address
- Notes

Separate grandfather and grandmother entities are not required.

Suggested Living Status values:

- Both Living
- One Living
- Neither Living
- Unknown

---

## 11.4 Future Considerations

The architecture intentionally leaves open the possibility of converting family members into reusable Person entities in a future major version.

Version 1 will keep these embedded within Student for simplicity.
---

# 12. Application

## 12.1 Purpose

An Application represents one request for admission to one Division for one Academic Year.

Applications are immutable historical records once submitted.

An Application is **not** an Enrollment.

An accepted Application may create an Enrollment, but an Application always exists independently of the Enrollment that may eventually result.

---

## 12.2 Business Objectives

The Application system exists to:

- Collect applicant information
- Create or identify the Student record
- Begin the Admissions workflow
- Preserve the submitted application permanently
- Support multiple applications over time
- Support applications to multiple Divisions
- Detect duplicate Students
- Drive acceptance and enrollment

---

## 12.3 Ownership

Each Application belongs to:

- exactly one Student
- one Academic Year
- one Division

Applications never move between Academic Years.

Applications never move between Divisions.

If either is incorrect, a new Application must be submitted.

---

## 12.4 Application Creation

Applications may originate from:

- Native Student Management web forms
- Gravity Forms (initial implementation)
- Staff entry
- Future API integrations

Regardless of the source, every Application is converted into the internal Application model.

The external system is never considered the authoritative record.

---

## 12.5 Student Matching

When an Application is submitted, the system attempts to identify an existing Student.

Possible matching fields include:

- SSN
- Legal Name
- Date of Birth
- Email
- Phone
- Parent Names
- Parent Email
- Parent Phone

The system may assign a confidence score.

No automatic merge is permitted.

Staff must decide whether the Application belongs to:

- an existing Student
- a newly created Student

---

## 12.6 Duplicate Applications

The system supports multiple Applications.

Examples include:

- Reapplication after rejection
- Returning after absence
- Applying to another Division
- Updated application
- Multiple submissions by mistake

Duplicate submissions are never deleted.

---

## 12.7 Same Division / Same Year

When multiple Applications exist for the same Division and Academic Year:

- every submission is preserved
- the newest submission becomes the default view
- staff is warned that multiple submissions exist
- older submissions remain accessible

Nothing is overwritten.

---

## 12.8 Different Division

Applications to different Divisions always require separate Applications.

Example:

```
YZA Application

↓

Later

↓

YOH Application
```

Both remain permanent records.

---

## 12.9 Reapplication

Rejected Applicants may submit another Application.

The new Application belongs to the existing Student.

The Student record is never recreated.

Previous Applications remain unchanged.

---

# 13. Application Submission

## 13.1 Purpose

Application Submission represents the exact web form submitted by the applicant.

This record is immutable.

---

## 13.2 Stored Information

Submission stores:

- every answer
- uploaded files
- submission timestamp
- source system
- source form version
- IP address (optional)
- browser metadata (optional)

Future versions may include digital signatures.

---

## 13.3 Immutability

Submitted answers are never edited.

If staff wishes to correct information:

The Student record is updated.

The submitted Application remains exactly as originally received.

This preserves legal and historical accuracy.

---

## 13.4 Attachments

Attachments remain associated with the Submission.

Examples:

- transcripts
- identification
- recommendation letters
- supporting documents

Future modules may attach additional documents directly to the Student.

---

## 13.5 Versioning

Each submission is permanent.

If a corrected Application is required:

A completely new Application Submission is created.

Previous submissions remain available.

---

# 14. Application Workflow

Applications move through a controlled workflow.

Recommended initial states:

```
Draft

↓

Submitted

↓

Under Review

↓

Additional Information Requested

↓

Ready for Decision

↓

Accepted
Rejected
Waitlisted
Withdrawn
```

Only authorized users may change workflow states.

---

## 14.1 Draft

Draft exists only while completing the web form.

Drafts are temporary.

No Student record exists yet.

---

## 14.2 Submitted

Submission creates:

- Student
- Application
- Application Submission

Duplicate matching begins.

Admissions review begins.

---

## 14.3 Under Review

Admissions staff review:

- completeness
- duplicate matches
- supporting documents
- admissions criteria

---

## 14.4 Additional Information Requested

Admissions may request:

- missing documents
- clarification
- updated information

Once received:

Application returns to Under Review.

---

## 14.5 Ready for Decision

Application is complete.

Awaiting admissions decision.

---

## 14.6 Accepted

Acceptance performs domain actions.

See Section 16.

---

## 14.7 Rejected

Rejection performs domain actions.

See Section 17.

---

## 14.8 Waitlisted

Application remains active.

Future decision may be:

- Accepted
- Rejected
- Withdrawn

---

## 14.9 Withdrawn

Applicant withdraws before decision.

History remains preserved.

---

# 15. Admissions Queue

The Admissions Queue is the operational work list.

It is not a database entity.

It is a filtered view.

Admissions staff should be able to filter by:

- Academic Year
- Division
- Current Status
- Duplicate Flag
- Missing Information
- Assigned Reviewer
- Submission Date
- Returning/New
- Financial Aid Requested

---

## 15.1 Next Action

Each Application has a configurable Next Action.

Examples:

- Review Application
- Request Transcript
- Send Financial Aid Application
- Schedule Interview
- Accept
- Reject
- Waitlist

Next Action is configurable.

It is not hard-coded.

---

# 16. Acceptance

Acceptance is a business event.

Acceptance is not merely changing Application Status.

Acceptance performs system actions.

---

## 16.1 Automatic Actions

Acceptance:

- changes Application Status
- changes Student Status to Current
- creates Enrollment
- creates first Enrollment Segment
- generates Admissions Audit Event
- creates configured workflow tasks
- optionally sends Acceptance Email

---

## 16.2 Enrollment Creation

Enrollment is created only after Acceptance.

Application submission never creates Enrollment.

This distinction is fundamental.

---

## 16.3 Financial Aid

If Financial Aid is required:

Acceptance may automatically generate:

- Financial Aid workflow
- Financial Aid request
- Next Action

This behavior is configurable.

---

## 16.4 Tuition

Acceptance does not automatically create Tuition.

Tuition is created later during Enrollment.

---

# 17. Rejection

Rejection performs the following:

- Application Status → Rejected
- Student Status → Rejected Applicant
- preserves all history
- hides Student from operational screens
- keeps Student searchable

The Student record remains available for future Applications.

---

# 18. Business Rules

The following rules must always hold.

An Application:

- belongs to one Student
- belongs to one Division
- belongs to one Academic Year
- never changes Division
- never changes Academic Year
- never loses Submission history
- may exist without Enrollment

An accepted Application:

- creates one Enrollment

A rejected Application:

- never deletes the Student

A Student:

- may have multiple Applications

Multiple submissions:

- never overwrite one another

Duplicate Students:

- are never merged automatically

---

# 19. Future Integrations

The Application module is designed to integrate with:

- Native Application Portal
- Parent Portal
- Email
- SMS
- Google Workspace
- Workflow Engine
- Document Management
- Financial Aid
- Enrollment
- CRM
- Future API integrations

---

# 20. AI Implementation Notes

Application is an Aggregate Root.

Application Submission is immutable.

Admissions Queue is a query model.

Never:

- overwrite submissions
- delete submissions
- edit submitted answers

Always:

- preserve history
- audit workflow changes
- normalize external form data
- keep Gravity Forms behind an adapter layer

Future implementations should support replacing Gravity Forms without changing the Application domain model.
---

# 21. Academic Year

## 21.1 Purpose

The Academic Year is the primary organizational boundary for all annual operations.

Nearly every operational workflow in the system belongs to an Academic Year rather than directly to the Student.

Examples include:

- Enrollment
- Financial Aid
- Tuition
- Scholarships
- Dormitory
- Meal Plan
- College Program
- Tuition Contracts
- Admire Status
- Enrollment Workflow

The Student provides permanent identity.

The Academic Year provides operational context.

---

## 21.2 Standard Academic Year

The default Academic Year is:

```
September 1
↓

August 31
```

The beginning and ending dates must be configurable.

Different schools using the system may define different Academic Year boundaries.

---

## 21.3 Academic Year States

Suggested lifecycle:

- Planning
- Enrollment Open
- Active
- Closing
- Closed
- Archived

Only one Academic Year should normally be Active.

However, the system must support the upcoming Academic Year being open for Enrollment while the current year is still Active.

---

## 21.4 Academic Year Ownership

The Academic Year owns operational records, including:

- Enrollments
- Division-Year Configuration
- Tuition Schedules
- Enrollment Requirements
- Contract Templates
- Email Templates
- Tuition Components
- Payment Plans
- Financial Aid Configuration

The Academic Year does **not** own Students.

Students exist independently of Academic Years.

---

## 21.5 Year Selection

Every major screen should either:

- derive the Academic Year automatically, or
- require the user to select it.

The selected Academic Year becomes part of the page context.

Example:

```
Student

↓

2026–2027 Enrollment

↓

Tuition

↓

Contracts
```

---

## 21.6 Historical Years

Historical Academic Years remain fully accessible.

Authorized users may edit historical records.

Every edit must be audited.

Locking an Academic Year limits editing but does not permanently prevent authorized corrections.

---

## 21.7 Design Decision

> Students are permanent.
>
> Academic Years are operational.
>
> This separation allows a Student to participate in multiple years without duplicating identity information while preserving complete historical workflows.

---

# 22. Division

## 22.1 Purpose

A Division represents an administrative unit within the institution.

Examples today include:

- YZA
- YOH
- Kollel

Future Divisions may be added without changing application code.

---

## 22.2 Division Philosophy

A Division is **not** merely a label.

Each Division may have different:

- admissions process
- tuition rules
- contracts
- financial aid requirements
- workflow
- permissions
- reporting
- email templates

The system must never hard-code Division behavior.

---

## 22.3 Global Division

A Division exists globally.

Example:

```
YZA
```

exists independently of any Academic Year.

Its yearly behavior is configured separately.

---

## 22.4 Division-Year Configuration

Each Academic Year contains a configuration for every active Division.

Example:

```
2026–2027

↓

YZA Configuration

↓

Tuition
Application Form
Workflow
Emails
```

This allows the same Division to behave differently in different Academic Years.

---

## 22.5 Configurable Items

Division-Year Configuration may include:

- Active
- Application Form
- Tuition Schedule
- Tuition Components
- Registration Fee
- Dormitory Allowed
- Meal Plan Allowed
- Tuition Required
- Financial Aid Allowed
- Financial Aid Required Rules
- Tuition Contract Required
- Admire Required
- College Program Enabled
- Email Templates
- Workflow Requirements
- Default Payment Plan

---

## 22.6 Division Activity

A Division may be:

- Active
- Inactive

for a particular Academic Year.

Historical records remain unchanged.

---

## 22.7 Simultaneous Divisions

A Student may not belong to two Divisions simultaneously.

Instead:

Division changes create new Enrollment Segments.

---

## 22.8 Design Decision

> Division is global.
>
> Division behavior is annual.
>
> This avoids creating duplicate Division records every Academic Year while allowing complete yearly flexibility.

---

# 23. Enrollment

## 23.1 Purpose

Enrollment is the annual operational record for a Student.

Everything staff manage during a school year belongs to the Enrollment.

The Enrollment is the center of the entire system.

---

## 23.2 Enrollment Philosophy

Student answers:

```
Who is this person?
```

Enrollment answers:

```
What is happening this Academic Year?
```

---

## 23.3 Enrollment Creation

An Enrollment may be created by:

- Application Acceptance
- Annual Pro Forma generation
- Manual Staff Creation

Application submission never creates an Enrollment.

---

## 23.4 One Enrollment Per Year

A Student normally has one Enrollment per Academic Year.

Division changes occur inside that Enrollment through Enrollment Segments.

---

## 23.5 Core Enrollment Fields

Each Enrollment stores:

- Academic Year
- Student
- New or Returning
- Enrollment Status
- Financial Aid Required
- Financial Aid Status
- FAFSA Eligible
- Tuition Contract Status
- Admire Status
- College Program Status
- Next Action
- Internal Notes

These are operational summaries.

Detailed records exist in their respective modules.

---

## 23.6 Enrollment Status

Enrollment Status is different from Student Status.

Examples:

Student Status

```
Current
```

Enrollment Status

```
Pending Decision
Returning
Not Returning
Withdrawn
Completed
```

Student Status describes the person.

Enrollment Status describes the year.

These must never be confused.

---

## 23.7 Design Decision

> Student Status is permanent.
>
> Enrollment Status is annual.
>
> Separating these avoids corrupting long-term history while supporting yearly workflows.

---

## 23.8 Enrollment Dashboard

Every Enrollment should be viewable from a single dashboard showing:

- Student
- Current Division
- Workflow Progress
- Outstanding Items
- Next Action
- Internal Notes

This becomes the primary operational screen for administrative staff.
---

## 23.9 Enrollment Workflow Philosophy

An Enrollment does not have a single linear workflow.

Instead, it consists of multiple independent workflows that together determine whether enrollment is complete.

For example:

- Returning Decision
- Financial Aid
- Tuition
- Tuition Contract
- Admire
- College Program
- Dormitory (future)
- Meal Plan (future)

Each workflow progresses independently.

Completion of one workflow must not automatically imply completion of another.

---

## 23.10 Workflow Requirements

Every Enrollment contains a configurable list of required workflow items.

Examples:

| Requirement | Required? | Status |
|-------------|-----------|--------|
| Returning Decision | Yes | Complete |
| Financial Aid | Yes | Pending |
| Tuition Package | Yes | Complete |
| Tuition Contract | Yes | Pending Signature |
| Admire | Yes | Not Started |
| College Program | No | N/A |

Future modules may add additional workflow requirements without changing the Enrollment model.

---

## 23.11 Requirement Status

Every requirement supports the same lifecycle.

Initial values:

- Not Applicable
- Not Started
- In Progress
- Pending Review
- Completed
- Waived

Waived is considered complete for Enrollment purposes.

---

## 23.12 Requirement Waivers

Authorized staff may waive individual requirements.

Examples:

- Tuition Contract signature waived
- Financial Aid waived
- Admire waived

Every waiver records:

- Staff member
- Date
- Reason
- Audit Event

Permissions determine who may create waivers.

---

## 23.13 Enrollment Completion

Enrollment Completion is never entered manually.

Instead, it is calculated.

Enrollment is Complete only when every required workflow is either:

- Completed

or

- Waived

The calculation should run automatically whenever a workflow changes.

---

## 23.14 Outstanding Requirements

The Enrollment Dashboard should clearly display remaining work.

Example:

```
Outstanding

• Tuition Contract
• Financial Aid Review
• Admire Entry
```

Staff should never need to investigate multiple modules to determine what remains.

---

## 23.15 Next Action

Each Enrollment contains one configurable Next Action.

Examples:

- Send Tuition Contract
- Review Financial Aid
- Update Admire
- Contact Family
- Waiting for Parent
- Waiting for School
- Complete Enrollment

The available Next Actions are configurable.

---

## 23.16 Internal Notes

Every Enrollment contains operational notes.

These notes belong to the Enrollment rather than the Student because they relate only to the current Academic Year.

Historical Enrollment notes remain with their respective Academic Year.

---

## 23.17 Design Decision

> Enrollment intentionally summarizes operational progress.
>
> The detailed records remain in their respective modules.
>
> This keeps Enrollment lightweight while allowing staff to understand the complete state of a Student without opening multiple screens.

---

# 24. Enrollment Segments

## 24.1 Purpose

Enrollment Segments represent effective-dated participation within an Enrollment.

A Student has one Enrollment for the Academic Year.

Within that Enrollment the Student may participate in one or more Divisions over time.

Each period of participation is represented by an Enrollment Segment.

---

## 24.2 Why Segments Exist

Students occasionally:

- transfer between Divisions
- withdraw mid-year
- return later
- begin Kollel
- end Kollel

Creating multiple Enrollments for the same Academic Year would fragment history.

Enrollment Segments preserve a single annual Enrollment while accurately representing changes during the year.

---

## 24.3 Segment Fields

Each Segment stores:

- Enrollment
- Division
- Effective Start Date
- Effective End Date
- Status
- Change Reason
- Notes

---

## 24.4 Segment Status

Suggested values:

- Planned
- Active
- Ended
- Cancelled

Only one Segment may be Active at a time.

---

## 24.5 Effective Dating

Every Segment has:

- Start Date
- End Date

Segment dates may not overlap.

A gap between Segments is allowed.

Example:

```
Sept 1

↓

YZA

↓

Jan 15

(no enrollment)

↓

Feb 10

↓

YOH
```

---

## 24.6 Division Changes

Division changes are implemented by:

1. Ending the current Segment.
2. Creating a new Segment.
3. Creating a new Tuition Package.
4. Recalculating Enrollment workflows.

The Student record is unchanged.

The Enrollment remains unchanged.

---

## 24.7 Mid-Year Withdrawal

Withdrawal closes the active Segment.

The Enrollment remains as the annual record.

Historical workflows remain attached to that Enrollment.

---

## 24.8 Current Division

The current Division is always derived from the active Enrollment Segment.

Division must never be duplicated as a manually maintained field on Student.

---

## 24.9 Segment Invariants

Segments:

- never overlap
- belong to one Enrollment
- belong to one Division
- remain historical after ending

A Student may have multiple Segments during one Academic Year.

Only one Segment may be active at any moment.

---

## 24.10 Design Decision

> Enrollment is annual.
>
> Segments are effective-dated.
>
> This allows every operational module to reference the correct period of participation without duplicating Enrollment records.---

# 25. Annual Pro Forma

## 25.1 Purpose

The Annual Pro Forma process prepares Students for the upcoming Academic Year.

Rather than immediately creating Enrollments, the system first creates a planning workspace where staff review each Student and determine whether they are expected to return.

This allows administrative staff to make enrollment decisions before operational records are created.

---

## 25.2 Design Philosophy

Every Academic Year begins with the previous Academic Year.

The system should never require staff to recreate the student population from scratch.

Instead:

```
2026–2027

↓

Generate Pro Forma

↓

Review Students

↓

Launch Enrollment

↓

2027–2028
```

---

## 25.3 Source Population

By default, the Pro Forma process includes:

- Current Students
- Active Students
- Students who completed the Academic Year

By default, it excludes:

- Withdrawn Students
- Rejected Applicants

Graduated Students are configurable.

Some schools may choose to include them.

---

## 25.4 Generated Information

Each Pro Forma record contains:

- Student
- Previous Division
- Previous Enrollment
- Previous Tuition
- Previous Financial Aid Requirement
- Previous Dorm
- Previous Meal Plan
- Previous College Program Status

These are reference values only.

Nothing has yet been copied into the new Academic Year.

---

## 25.5 Initial Status

Every generated record begins as:

```
Pending Decision
```

No Enrollment exists yet.

---

## 25.6 Staff Review

Staff review each Student individually.

Possible decisions:

- Returning
- Not Returning
- Deferred
- Pending Decision

Future statuses may be added through configuration.

---

## 25.7 Returning

When marked Returning:

The Student becomes eligible for Enrollment creation.

No Enrollment is created until the Pro Forma process is launched.

---

## 25.8 Not Returning

Students marked Not Returning:

- remain historical Students
- receive no Enrollment
- remain searchable
- may later be changed back to Returning before or after launch

---

## 25.9 Deferred

Deferred allows staff to postpone the decision.

The Student remains in the Pro Forma workspace.

---

## 25.10 Bulk Review

The Pro Forma page should support bulk actions.

Examples:

- Mark Returning
- Mark Not Returning
- Assign Division
- Set Tuition Default
- Set Financial Aid Required

Bulk operations must be audited.

---

## 25.11 Editing After Launch

The Pro Forma process remains available after launch.

Late Students may still be:

- marked Returning
- marked Not Returning
- added
- removed

Launching does not permanently close the planning process.

---

## 25.12 Design Decision

> The Pro Forma process is intentionally separate from Enrollment.
>
> It provides a planning layer that allows staff to make operational decisions before creating the Academic Year's working records.

---

# 26. Enrollment Generation

## 26.1 Purpose

Enrollment Generation converts approved Pro Forma records into operational Enrollment records.

This is the beginning of the Academic Year's working data.

---

## 26.2 Generated Records

For every Returning Student, the system creates:

- Enrollment
- Initial Enrollment Segment
- Enrollment Workflow
- Configured Requirements

Nothing financial is created automatically.

---

## 26.3 Default Values

The system may copy default information from the previous Academic Year.

Examples:

- Division
- Dorm preference
- Meal preference
- Financial Aid Required
- College Program participation

These copied values remain editable.

---

## 26.4 Tuition

Tuition Packages are **not** automatically generated.

Instead, the Enrollment workflow will later require staff to establish Tuition.

This supports:

- annual tuition changes
- scholarship negotiations
- changing family circumstances

---

## 26.5 Financial Aid

The Enrollment references the Student's current Financial Aid Profile.

If configuration requires a new submission:

The Enrollment Financial Aid Requirement becomes:

```
New Submission Required
```

Otherwise the current submission satisfies the requirement.

---

## 26.6 Workflow Creation

Enrollment Generation automatically creates the configured workflow requirements for that Division and Academic Year.

Example:

```
Returning Decision

✓

Financial Aid

Pending

↓

Tuition

Pending

↓

Contract

Pending

↓

Admire

Pending
```

The workflow definition comes from configuration.

---

## 26.7 Audit

Enrollment Generation creates Audit Events recording:

- Academic Year
- Student
- User
- Generation Date
- Source Pro Forma

---

## 26.8 Design Decision

> Enrollment Generation creates operational records.
>
> Financial records remain intentionally absent until staff complete the Tuition workflow.
---

# 27. Financial Architecture Overview

The financial architecture separates four related but distinct concerns:

1. Financial Aid information
2. Tuition calculation
3. Responsibility allocation
4. Contract and Admire completion

These domains interact, but they must not be collapsed into one record.

```text
Student
│
├── Financial Aid Profile
│   └── Financial Aid Submissions
│
└── Enrollment
    ├── Financial Aid Requirement
    ├── Admire Status
    │
    └── Enrollment Segment
        └── Tuition Package Versions
            ├── Tuition Package Lines
            ├── Responsibility Allocations
            └── Tuition Contracts
# 28. Student Financial Aid Profile

## 28.1 Purpose

The Student Financial Aid Profile is the permanent institutional record of a Student's Financial Aid history.

Unlike Tuition, which is recreated each Academic Year, Financial Aid information is generally long-lived.

A Student will normally complete one Financial Aid Application that remains valid until staff determine that updated information is required.

The Financial Aid Profile provides continuity across Academic Years while allowing each Enrollment to independently determine whether the existing information satisfies that year's requirements.

---

## 28.2 Design Philosophy

The Financial Aid Profile answers:

> **What Financial Aid information do we currently have on file for this Student?**

The Enrollment Financial Aid Requirement answers:

> **Does this Academic Year's Enrollment require new Financial Aid information?**

These are intentionally different questions.

The system must never duplicate Financial Aid history inside annual Enrollment records.

---

## 28.3 Ownership

The Financial Aid Profile belongs directly to one Student.

Relationship:

```text
Student
│
└── Financial Aid Profile
        │
        ├── Submission 1
        ├── Submission 2
        ├── Submission 3
        └── Current Submission
```

A Student may have:

- no Financial Aid Profile
- one Financial Aid Profile

The Profile may contain multiple historical submissions.

Only one submission may be Current.

---

## 28.4 Responsibilities

The Financial Aid Profile is responsible for:

- Maintaining Financial Aid history
- Identifying the Current Submission
- Recording whether a replacement has been requested
- Recording review history
- Providing Financial Aid information to future Enrollments

It is **not** responsible for:

- Annual workflow
- Scholarships
- Tuition
- Contracts
- Admire

Those belong elsewhere.

---

## 28.5 Core Fields

Suggested fields:

- Student
- Current Submission
- Current Submission Date
- Current Submission Status
- Replacement Required
- Replacement Requested Date
- Replacement Requested By
- Replacement Reason
- Last Reviewed Date
- Last Reviewed By
- Internal Notes

The detailed answers belong to the Financial Aid Submission.

---

## 28.6 Current Submission

The Current Submission is the submission staff consider valid today.

Example:

```text
Financial Aid Profile

Current Submission

↓

March 2026 Submission
```

Multiple future Enrollments may reference the same Current Submission.

Example:

```text
2026–2027 Enrollment

↓

March 2026 Submission

2027–2028 Enrollment

↓

March 2026 Submission
```

The system should not create duplicate Financial Aid records simply because a new Academic Year begins.

---

## 28.7 Replacement Required

Staff may determine that the existing submission is no longer sufficient.

Reasons include:

- Family financial circumstances changed
- Policy requires updated information
- Missing information
- Supporting documentation expired
- Staff request
- Other administrative reason

When Replacement Required is set:

- The existing Submission remains Current.
- The Student Financial Aid history remains unchanged.
- Future Enrollments require a replacement before Financial Aid workflow can be completed.

---

## 28.8 Historical Integrity

The Financial Aid Profile must preserve complete historical information.

Example:

```text
Submission A
March 2026

↓

Current

↓

Submission B
June 2028

↓

Current

↓

Submission A

Superseded
```

Submission A is never deleted.

---

## 28.9 Future Integration

The Financial Aid Profile is expected to integrate with:

- Enrollment
- Tuition
- Scholarship recommendations
- Secure parent portal
- Email notifications
- Workflow engine

without changing its ownership.

---

## 28.10 Business Rules

A Financial Aid Profile:

- belongs to exactly one Student
- may contain many submissions
- has only one Current Submission
- preserves every prior submission
- never overwrites historical submissions
- survives Student status changes
- survives Enrollment changes

---

## 28.11 Design Decision

> Financial Aid belongs to the Student because Financial Aid history spans multiple Academic Years.
>
> Annual Enrollment determines whether that history is sufficient for the current year's workflow.

---

## 28.12 AI Implementation Notes

Aggregate Root:

```
Student
```

Child Entity:

```
FinancialAidProfile
```

Preferred relationships:

```
Student

↓

FinancialAidProfile

↓

FinancialAidSubmission
```

Never:

- duplicate submissions into Enrollment
- overwrite historical submissions
- store Financial Aid answers on Student

Always:

- preserve every submission
- reference the current submission
- audit replacement requests
# 29. Financial Aid Submission

## 29.1 Purpose

A Financial Aid Submission represents the exact Financial Aid application submitted by the responsible party.

Unlike the Student Financial Aid Profile, which is a living institutional record, the Submission is an immutable snapshot.

Once submitted, it becomes part of the permanent institutional record.

---

## 29.2 Design Philosophy

The submitted application is a historical document.

Staff may:

- review it
- annotate it
- reference it
- request a replacement

Staff may **never** alter what was originally submitted.

If information changes, the Student record is updated or a new Submission is requested.

---

## 29.3 Ownership

Every Financial Aid Submission belongs to exactly one Student Financial Aid Profile.

Relationship:

```text
Student
│
└── Financial Aid Profile
        │
        ├── Submission A
        ├── Submission B
        └── Submission C
```

The Profile determines which Submission is Current.

---

## 29.4 Submission Contents

Each Submission contains:

- Submission Date
- Submission Time
- Submitted By
- Source Form
- Source Form Version
- All submitted answers
- Uploaded documents
- Supporting attachments
- Digital acknowledgements
- Submission metadata

Future versions may include digital signatures.

---

## 29.5 Submission Status

Suggested statuses:

- Draft
- Submitted
- Current
- Superseded
- Withdrawn

Drafts exist only prior to submission.

Submitted records immediately become immutable.

---

## 29.6 Immutability

Once submitted:

The following may never change:

- submitted answers
- uploaded files
- timestamps
- acknowledgements
- calculated values stored within the submission

Corrections must be handled through:

- Student record updates
- Staff review notes
- Replacement Submission

---

## 29.7 Replacement Submission

When updated Financial Aid information is required:

1. A new Submission is created.
2. The previous Current Submission becomes Superseded.
3. The new Submission becomes Current.
4. Historical references remain unchanged.

Nothing is overwritten.

---

## 29.8 Historical References

Historical Enrollments continue referencing the Submission that satisfied that year's requirements.

Example:

```text
2026–2027 Enrollment

↓

Submission A

2027–2028 Enrollment

↓

Submission A

2028–2029 Enrollment

↓

Submission B
```

Changing the Current Submission does not rewrite historical Enrollment records.

---

## 29.9 Attachments

Attachments belong to the Submission.

Examples include:

- tax returns
- W-2s
- pay stubs
- supporting letters
- additional documentation

Future attachments added by staff belong to the Student or Review record rather than altering the original Submission.

---

## 29.10 Staff Review

Staff may associate review information with a Submission.

Examples:

- reviewed by
- review date
- notes
- missing information
- clarification requested

These are separate records.

The Submission itself never changes.

---

## 29.11 Business Rules

A Financial Aid Submission:

- belongs to one Financial Aid Profile
- is immutable
- may become Superseded
- may never be edited
- may never be physically deleted through ordinary operations
- preserves every attachment
- preserves every submitted answer

---

## 29.12 Design Decision

> The submitted Financial Aid application is treated like a signed paper document.
>
> Staff may evaluate it, but they do not rewrite history.

---

## 29.13 AI Implementation Notes

Aggregate:

```
FinancialAidProfile
    │
    └── FinancialAidSubmission
```

Never:

- edit submitted answers
- replace attachments
- overwrite historical metadata

Always:

- create new submissions
- preserve prior submissions
- audit supersession
- reference submissions rather than copying their data
# 30. Enrollment Financial Aid Requirement

## 30.1 Purpose

The Enrollment Financial Aid Requirement represents the Financial Aid workflow for one Academic Year.

It answers a different question than the Student Financial Aid Profile.

The Student Financial Aid Profile answers:

> "What Financial Aid information do we have on file?"

The Enrollment Financial Aid Requirement answers:

> "Has this Enrollment satisfied its Financial Aid requirement?"

This distinction is fundamental to the architecture.

---

## 30.2 Ownership

Each Enrollment owns exactly one Financial Aid Requirement.

Relationship:

```text
Enrollment
│
└── Financial Aid Requirement
```

The Requirement references:

- Student Financial Aid Profile
- Current Financial Aid Submission

It does not own either.

---

## 30.3 Responsibilities

The Financial Aid Requirement is responsible for:

- determining whether Financial Aid is required
- tracking annual workflow
- referencing the controlling Submission
- requesting replacements
- recording completion
- recording waivers

It is **not** responsible for:

- storing submitted answers
- storing historical Financial Aid information
- calculating scholarships

---

## 30.4 Core Fields

Suggested fields:

- Enrollment
- Financial Aid Required
- Referenced Submission
- Replacement Required
- Requirement Status
- Request Sent Date
- Submission Received Date
- Reviewed By
- Review Date
- Review Status
- Review Notes
- Requested Clarifications
- Recommended Scholarship
- Approved Scholarship
- Decision Reason
- Approved By
- Approval Date
- Review Completed Date
- Waived
- Waived By
- Waived Date
- Waiver Reason
- Internal Notes

The approved scholarship amount is not the authoritative financial amount.

The actual scholarship affecting the Student’s obligation must be recorded as a line in the controlling Tuition Package.

---

## 30.5 Requirement Status

Suggested workflow:

- Not Applicable
- Not Requested
- Requested
- Sent
- Received
- Under Review
- Completed
- Waived

These statuses represent workflow only.

They never modify the underlying Submission.

---

## 30.6 Carry Forward

When a new Enrollment is created:

The system checks the Student Financial Aid Profile.

If:

- Current Submission exists
- Replacement is not required

then the Enrollment references the existing Submission.

No duplicate Submission is created.

---

## 30.7 Replacement Required

If staff require updated Financial Aid:

The Requirement becomes incomplete.

Example:

```text
Financial Aid

↓

Replacement Required

↓

Waiting for Submission
```

The previous Submission remains part of the Student history until replaced.

---

## 30.8 Waiver

Authorized staff may waive the annual Financial Aid requirement.

Examples:

- Full Tuition
- Administrative Exception
- Other approved reason

Waiver records:

- User
- Date
- Reason
- Previous Status

Every waiver creates an Audit Event.

---

## 30.9 Completion

Financial Aid is considered complete when one of the following is true:

- Requirement is Not Applicable
- Existing Submission satisfies the requirement
- New Submission has been received and accepted
- Requirement has been waived

Enrollment Completion uses this result.

---

## 30.10 Review Outcome

The annual Financial Aid review belongs to the Enrollment Financial Aid Requirement.

It may record:

- Reviewer
- Review date
- Clarifications requested
- Recommended scholarship
- Approved scholarship
- Decision reason
- Approval metadata
- Internal notes

The review explains the Financial Aid decision.

The controlling Tuition Package records the actual financial effect.

If the approved scholarship later changes:

1. The Financial Aid Requirement review is updated with full audit history.
2. A new Tuition Package version is created.
3. The new scholarship amount is recorded as a Tuition Package Line.
4. Admire Status is reset if the net tuition amount changes.
5. A new Tuition Contract may be required.

## 30.11 Relationship to Enrollment

The Enrollment Dashboard should summarize Financial Aid using:

- Current Status
- Referenced Submission Date
- Replacement Required
- Outstanding Actions

Staff should not need to open the Student Financial Aid Profile simply to determine annual progress.

---

## 30.12 Business Rules

An Enrollment Financial Aid Requirement:

- belongs to one Enrollment
- references one controlling Submission
- may require a replacement
- may be waived
- may not modify Submission history
- participates in Enrollment Completion calculations

---

## 30.13 Design Decision

> The Enrollment owns the workflow.
>
> The Student owns the Financial Aid history.
>
> This separation prevents duplicated records while preserving accurate annual workflow tracking.

---

## 30.14 Future Integrations

Future integrations include:

- Parent Portal
- Workflow Engine
- Email Notifications
- Scholarship Recommendation Engine
- Secure Document Upload
- Automated Reminder System

These integrations should operate through the Requirement rather than directly manipulating the Student Financial Aid Profile.

---

## 30.15 AI Implementation Notes

Aggregate:

```
Enrollment
│
└── FinancialAidRequirement
```

Reference only:

```
Student
│
└── FinancialAidProfile
```

Never:

- duplicate submissions
- copy Financial Aid answers into Enrollment
- update Submission contents

Always:

- reference the controlling Submission
- calculate workflow completion
- audit waivers
- preserve historical references
# 31. Financial Aid Review Outcome

## 31.1 Purpose

The Financial Aid Review Outcome records the administrative evaluation of the Financial Aid information referenced by one Enrollment.

It explains:

- Who reviewed the information
- When it was reviewed
- Whether clarification was requested
- What scholarship was recommended
- What scholarship was approved
- Why the decision was made

The Review Outcome belongs to the Enrollment Financial Aid Requirement.

It is not a separate aggregate and does not own the Financial Aid Submission.

---

## 31.2 Ownership

The Review Outcome is part of one Enrollment Financial Aid Requirement.

Relationship:

```text
Enrollment

↓

Financial Aid Requirement

↓

Review Outcome
## 31.3 Suggested Fields

- Reviewed By
- Review Date
- Review Status
- Review Notes
- Clarification Requested
- Clarification Requested Date
- Clarification Received Date
- Recommended Scholarship
- Approved Scholarship
- Decision Reason
- Approved By
- Approval Date
- Replacement Review Required
- Internal Notes

---

## 31.4 Review Status

Suggested statuses:

- Not Started
- In Review
- Clarification Requested
- Ready for Decision
- Approved
- Declined
- Completed
- Superseded

These statuses describe the review process only.

They do not modify the submitted Financial Aid application.

---

## 31.5 Scholarship Recommendation

The Review Outcome may record:

- Recommended Scholarship
- Approved Scholarship

These values explain the administrative decision.

They are **not** the authoritative financial record.

The authoritative financial effect is recorded as a Tuition Package Item in the active Tuition Package Version.

---

## 31.6 Changes to the Decision

If the approved scholarship changes:

1. Record the new decision.
2. Preserve the prior decision in Audit History.
3. Create a new Tuition Package Version.
4. Add or modify the Scholarship Tuition Package Item.
5. Recalculate the package total.
6. Reset Admire Status if the net tuition changes.
7. Generate a replacement Tuition Contract if required.

---

## 31.7 Clarification Requests

Staff may request clarification without modifying the original Financial Aid Submission.

Clarifications may be resolved by:

- Staff Notes
- Communications
- Additional Attachments
- A replacement Financial Aid Submission

The original submission always remains unchanged.

---

## 31.8 Business Rules

A Review Outcome:

- belongs to one Enrollment Financial Aid Requirement
- references one Financial Aid Submission
- may recommend a scholarship
- may approve a scholarship
- never changes submitted answers
- never directly changes tuition
- always preserves review history

---

## 31.9 Design Decision

> Financial Aid Review explains **why** a scholarship was approved.
>
> The Tuition Package records **what** financial obligation the Student ultimately has.

This keeps business reasoning separate from financial accounting.

---

## 31.10 AI Implementation Notes

Implement Review Outcome as a child object of the Enrollment Financial Aid Requirement.

Do not implement it as a separate aggregate.

Never:

- edit submitted Financial Aid answers
- calculate balances from the Review
- duplicate scholarship amounts elsewhere

Always:

- preserve review history
- audit decision changes
- create a new Tuition Package Version when an approved scholarship changes