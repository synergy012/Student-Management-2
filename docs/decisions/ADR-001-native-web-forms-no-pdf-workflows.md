# ADR-001: Native Web Forms and No PDF Workflows

## Status

Accepted

## Context

The legacy system invested substantial effort in generating, filling, storing, routing, and signing PDFs. This created duplicate data, brittle field mappings, external integration complexity, and poor workflow visibility.

The new system requires applications and tuition contracts in early releases and must treat their data as structured, queryable, versioned system records.

## Decision

All operational forms and agreements will be native web workflows.

This applies to:

- Applications
- Requests for additional information
- Financial-aid applications
- Tuition contracts
- Acknowledgments
- Signatures
- Corrections
- Amendments

The system will not generate, fill, route, or depend on PDFs.

## Supporting attachments

Users may upload supporting files, including PDFs, when evidence is required. An uploaded PDF is an attachment only and is not the authoritative workflow record.

## Contract preservation

An accepted contract must preserve:

- Exact contract-template version
- Exact displayed language
- Exact financial values
- All acknowledgments
- Signer identity and capacity
- Date and time
- Acceptance method
- Relevant request metadata
- Immutable snapshot
- Supersession/amendment links

## Consequences

Positive:

- Structured searchable data
- Clear workflow tracking
- Easier validation
- Easier annual configuration
- Reduced external dependency
- Better auditability
- Better mobile experience

Tradeoffs:

- Web signature and evidence standards must be designed carefully.
- Print-friendly HTML may be needed.
- Immutable snapshots and versioning are mandatory.
- Supporting-file security remains necessary.
