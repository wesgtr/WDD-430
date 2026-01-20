# Research Notes: ReloadLog Web Application

**Date**: 2026-01-20  
**Spec**: `spec-kit/.specify/memory/spec.md`

## Open Decisions

- Prisma vs Drizzle (recommend Prisma)
- Session cookie auth details (rolling sessions, CSRF strategy)
- Export: CSV first, PDF later
- Units strategy (imperial default; optional metric preference)

## Guardrails

- No generated reloading recommendations.
- Safety check compares only against user-provided bounds with required citation.
