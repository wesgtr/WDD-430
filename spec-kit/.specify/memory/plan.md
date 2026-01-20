# Implementation Plan: ReloadLog Web Application

**Branch**: `001-reloadlog-webapp` | **Date**: 2026-01-20 | **Spec**: `spec-kit/.specify/memory/spec.md`
**Input**: Feature specification from `/spec-kit/.specify/memory/spec.md`

## Summary

Build a full-stack web application for tracking user-owned reloading projects, load recipes, and range test sessions, and generate analysis/reporting of improvement over time.

**Approach (AnalogJS + PostgreSQL)**

- Use **AnalogJS** (Angular-based) for the UI and server runtime (server routes) so the app can be deployed as a single web service.
- Use **PostgreSQL** for persistence.
- Use **Prisma** for schema/migrations/typed DB access.
- Implement **cookie-based session authentication** (httpOnly, sameSite, secure in production) with **argon2id** password hashing.
- Implement analysis as:
  - persisted aggregates (computed at session finalization)
  - on-demand trend queries for charts.

## Technical Context

**Language/Version**: TypeScript 5.x + Node.js 20 LTS  
**Primary Dependencies**: AnalogJS, Angular, Prisma, Zod, argon2  
**Storage**: PostgreSQL 15+  
**Testing**: Vitest (unit), Playwright (E2E)  
**Target Platform**: Web + Node server runtime  
**Project Type**: web  
**Performance Goals**: p95 API latency ≤ 300ms; analysis ≤ 2s for 10k shot rows  
**Constraints**: no generated load recommendations; safety disclaimers; deterministic tests

## Constitution Check

- **Code Quality Standards**: lint/format/typecheck gates; typed contracts
- **Testing Standards**: unit + integration + E2E; fix flakes fast
- **User Experience Consistency**: match mockups; consistent states; accessibility
- **Performance Requirements**: indexes + persisted aggregates; budgeted dashboards
- **Security and Reliability**: session cookies; CSRF strategy; strict authorization

## Project Structure

```text
specs/001-reloadlog-webapp/          # canonical plan/spec set

spec-kit/.specify/memory/            # working copies for Spec-Kit commands
├── spec.md
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
└── contracts/
    └── openapi.yaml
```

## Phase Plan

- Phase 0: Research & decisions (ORM/auth/testing/export strategy)
- Phase 1: Data model + API contracts + route map to mockups
- Phase 2: MVP build (auth, project/recipe CRUD, sessions+shots, aggregates)
- Phase 3: Analysis + export (CSV first, PDF optional)
- Phase 4: Optional inventory
