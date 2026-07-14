# Security Policy

## Reporting Security Issues

If you discover a security vulnerability in this project, please **do not** open a public issue. Instead:

1. Email the project maintainers directly.
2. Include details of the vulnerability and steps to reproduce if possible.
3. Allow time for a fix before public disclosure.

## Security Scope

This project implements the itonami actor pattern for landscape architecture design support. Security considerations include:

- **Audit Trail** — all proposals, verdicts, and decisions are logged in an append-only ledger.
- **Governor Invariants** — the governor enforces hard constraints (project registration, no direct mutation, no unauthorized stamping).
- **No Stamping** — this actor never issues final stamped designs or certifies environmental compliance; those remain the licensed landscape architect's exclusive responsibility.
- **Store Integrity** — records are committed atomically and never modified in place.

## Supported Versions

Only the latest version is actively supported. Security fixes may be backported to recent versions on a case-by-case basis.
