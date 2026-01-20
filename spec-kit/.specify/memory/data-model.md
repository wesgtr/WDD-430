# Data Model: ReloadLog Web Application

**Date**: 2026-01-20  
**Spec**: `spec-kit/.specify/memory/spec.md`

Canonical model is in `specs/001-reloadlog-webapp/data-model.md`. This copy exists for Spec-Kit workflow convenience.

Core entities: User, Session, Project, LoadRecipe, SafetyReference, RangeSession, Shot, AccuracyResult, ReportExportJob (+ optional InventoryItem).

Persist aggregates on session finalization: avg/SD/ES velocity.
