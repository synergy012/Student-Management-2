# System Constitution

## Student Management 2

**Version:** 1.0 (Living Document)

------------------------------------------------------------------------

# Purpose

This document defines the architectural principles governing Student
Management 2.

Every module, workflow, database design, API, and user interface must
conform to these principles. If a future implementation conflicts with
this document, this document takes precedence unless it is intentionally
revised.

------------------------------------------------------------------------

# 1. The Student Is Permanent

A Student represents a person, not a school year. A Student record
exists for the lifetime of the institution.

Students may: - Apply multiple times - Attend multiple divisions -
Withdraw and return - Graduate - Join or leave Kollel - Change
addresses - Submit multiple applications

None of these events create a new Student.

------------------------------------------------------------------------

# 2. Academic Years Drive Operations

Almost every operational workflow belongs to an Academic Year rather
than to the Student.

Examples: - Enrollment - Tuition - Financial Aid - Dormitory - Meal
Plans - Scholarships - Registration - College Program - Kollel

The Student stores history.

Academic Years store operations.

------------------------------------------------------------------------

# 3. Preserve History

Historical information should never be overwritten.

Prefer: - Versioning - Effective dates - Historical records - Timelines

History is a feature, not clutter.

------------------------------------------------------------------------

# 4. Submitted Records Are Immutable

Submitted records represent exactly what users submitted.

Examples: - Applications - Financial Aid Applications - Tuition
Contracts

The operational records created from them may change.

The submitted record never does.

------------------------------------------------------------------------

# 5. Web First

All operational workflows use native web forms.

PDFs are archival documents only.

------------------------------------------------------------------------

# 6. Configuration Before Customization

Whenever practical, behavior should be configurable rather than
hard-coded.

Examples: - Divisions - Statuses - Tuition Components - Workflow Steps -
Email Templates - Academic Years - Permissions - Payment Plans

------------------------------------------------------------------------

# 7. Build Generic Systems

Build reusable engines instead of one-off solutions.

For example, build "Division Tuition" rather than "YZA Tuition."

------------------------------------------------------------------------

# 8. Independent but Connected Workflows

Applications, Enrollment, Financial Aid, Tuition, Contracts, Kollel,
Dormitory, and College Program are independent workflows connected
through events.

------------------------------------------------------------------------

# 9. Permissions Are Part of the Data Model

Permissions are enforced by the backend.

Hidden means hidden.

------------------------------------------------------------------------

# 10. Everything Important Is Audited

Record: - Who - When - What - Previous value - New value - Reason (when
required)

------------------------------------------------------------------------

# 11. Security Before Convenience

Sensitive data includes: - SSNs - Medical information - Financial Aid -
Tuition - Scholarships - Internal Notes

These require explicit authorization.

------------------------------------------------------------------------

# 12. One Source of Truth

Every piece of information has one authoritative owner.

Examples: - Student → Current demographic information - Application →
Submitted answers - Enrollment → Year-specific participation - Dorm
Module → Dorm assignment

------------------------------------------------------------------------

# 13. Build Vertical Slices

Each milestone should produce a complete, usable workflow rather than
isolated infrastructure.

------------------------------------------------------------------------

# 14. AI Builds Incrementally

Every milestone should compile, run, and be testable before moving on.

------------------------------------------------------------------------

# 15. Everything Is Searchable

Historical institutional knowledge should remain discoverable without
cluttering daily workflows.

------------------------------------------------------------------------

# 16. The Student Record Is Both Operational and Historical

It supports today's work while preserving institutional history.

------------------------------------------------------------------------

# 17. Simple User Experience

The UI should remain simple even when the underlying model is
sophisticated.

------------------------------------------------------------------------

# 18. Integrations Are Adapters

Gravity Forms, Google Workspace, Admire, and HES integrate through
adapters.

The internal domain model must not depend on vendor-specific
implementations.

------------------------------------------------------------------------

# 19. Future-Proof First

Prefer designs that will still make sense after years of growth,
additional divisions, and new integrations.

------------------------------------------------------------------------

# Final Principle

The objective is not merely to build software.

The objective is to create the permanent institutional operating system
for the school.
