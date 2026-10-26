# Project scope

## Problem
Extract selected information from documents and return validated structured data.

## Initial target fields
- `governing_law`
- `termination_notice`
- `renewal_terms`

These fields must be checked against the chosen dataset's real annotations. Update the field definitions if source labels do not map cleanly.

## Version 1 should
- Process a small, permitted sample set.
- Produce schema-valid JSON.
- Distinguish missing/ambiguous values from supported values.
- Include tests and reproducible evaluation.
- Document known limitations.

## Version 1 should not
- Provide legal advice or make legal decisions.
- Claim guaranteed accuracy.
- Process confidential documents without an approved privacy/security design.
- Include cloud deployment unless the local pipeline is reliable first.

## Acceptance criteria
See the README and PROJECT_PLAN.md. Replace assumptions with verified decisions during implementation.
