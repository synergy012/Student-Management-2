# Legacy System Lessons

The previous repository is a source of requirements and lessons, not the foundation for the new codebase.

## Preserve conceptually

- Applications and admissions
- Academic-year selection
- Enrollment dashboard
- Bulk actions
- Tuition components by division and year
- Proration and student-specific overrides
- Kollel monthly stipend concepts
- Email templates and communication tracking
- Roles, permissions, and activity logging
- Academic-year transition
- Reporting and exports
- Admire integration as a future workstream

## Rebuild differently

- Do not overload Student with annual fields.
- Do not duplicate entire application records into Student.
- Do not model workflow as many independent Boolean fields.
- Do not hard-code YZA, YOH, KOLLEL, or payment-component names in domain logic.
- Do not combine technical secrets with business configuration.
- Do not write `.env` files from the administrator UI.
- Do not maintain separate report-state copies.
- Do not mix current and historical data.
- Do not preserve generated PDF or Dropbox Sign architecture.

## Remove entirely

- Fillable PDF logic
- AcroForm logic
- PDF templates
- PDF contract generation
- PDF field mapping
- Document-based signature workflows
- Dropbox Sign integration tied to generated documents
- PDF path fields as contract authority

## Replace with

- Native web forms
- Immutable submission snapshots
- Versioned contract terms
- Structured financial lines
- Workflow-state history
- Web acknowledgments and signatures
- Read-only web rendering and print-friendly HTML
