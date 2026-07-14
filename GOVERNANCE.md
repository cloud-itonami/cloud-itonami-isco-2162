# Governance

## Decision-making

This project operates under a consensus-driven model with clear escalation paths:

- **Minor changes** (typos, documentation, small tests) may be merged by any maintainer.
- **Feature additions and architectural changes** require review and approval from the project lead.
- **Governance changes** are decided by the core team via discussion in the issue tracker.

## Roles

- **Project Lead** — owns the vision, approves major changes.
- **Maintainers** — review PRs, merge-ready code, respond to issues.
- **Contributors** — submit changes via pull requests.

## Standards

All code must:

1. Pass the full test suite (`clojure -M:test`).
2. Maintain the itonami actor pattern (governor-gated decisions, append-only audit trail).
3. Be documented with clear docstrings.
4. Follow the existing code style and conventions.

## Escalation

Any dispute about scope, direction, or design is escalated to the project lead for final decision.
