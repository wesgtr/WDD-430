# Feature Specification: ReloadLog Web Application (Reloading Log + Test Progress)

**Feature Branch**: `001-reloadlog-webapp`  
**Created**: 2026-01-20  
**Status**: Draft  
**Input**: User description: "Create a detailed project specification for a web application that allows users to create the reloading ammo recipes and progress with tests updates and report the improvement.. The application should include user authentication, recipe creation and reports of progress and test as in the mockups"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Sign up / Sign in and protect data (Priority: P1)

As a user, I can create an account and sign in so that my projects, recipes, and test history are private and persist across devices.

**Why this priority**: No meaningful value if users cannot securely save and return to their data.

**Independent Test**: A tester can create an account, sign in/out, and verify protected pages require authentication.

**Acceptance Scenarios**:

1. **Given** I am not signed in, **When** I visit a protected route (e.g., dashboard), **Then** I am redirected to sign-in.
2. **Given** I have a verified account, **When** I sign in with correct credentials, **Then** I reach the dashboard.
3. **Given** I am signed in, **When** I sign out, **Then** my session ends and protected pages require sign-in again.

---

### User Story 2 - View dashboard and projects (Priority: P1)

As a user, I can see a dashboard that summarizes my recent projects and key metrics so I can quickly understand where I am in my load development.

**Why this priority**: The mockups emphasize a dashboard-first workflow; it’s the main entry point.

**Independent Test**: Create a project and verify it appears on the dashboard; verify dashboard cards render with correct aggregate values.

**Acceptance Scenarios**:

1. **Given** I have zero projects, **When** I open the dashboard, **Then** I see an empty-state prompt to create a project.
2. **Given** I have projects, **When** I open the dashboard, **Then** I see a recent projects list and summary stats.
3. **Given** I search for a project/recipe, **When** I type into search, **Then** results filter within 300ms for up to 1,000 items.

---

### User Story 3 - Create a project and manage its load recipes (Priority: P1)

As a user, I can create a “project” (caliber + platform notes) and add multiple load recipes within it so that I can iterate and compare outcomes.

**Why this priority**: Projects + recipe lists are core to the mockups and to organizing progress.

**Independent Test**: Create a project, add two recipes, and confirm they appear in the project details “Load Recipes” tab.

**Acceptance Scenarios**:

1. **Given** I am signed in, **When** I create a project with required fields, **Then** it appears in my project list.
2. **Given** a project exists, **When** I create a new load recipe, **Then** it is listed with a status (e.g., Draft/Development/Verified).
3. **Given** a recipe exists, **When** I edit it, **Then** changes are saved and the “Last Modified” timestamp updates.

---

### User Story 4 - Enter recipe details with validation and safety guardrails (Priority: P1)

As a user, I can record recipe details (components + measurements) and run a “safety check” workflow that helps me confirm I entered the values I intended.

**Why this priority**: Data quality matters; the mockups include a dedicated recipe entry form and a validation sidebar.

**Independent Test**: Create a recipe, enter values, attempt save with missing required fields, and confirm validation behavior.

**Acceptance Scenarios**:

1. **Given** I am creating a recipe, **When** I omit required fields, **Then** I see inline, actionable validation messages and the recipe cannot be saved.
2. **Given** I have entered numeric measurement fields, **When** I enter out-of-range values (project-defined or user-defined ranges), **Then** I receive a warning before save.
3. **Given** I click “Validate Load”, **When** reference bounds are available (user-provided), **Then** the UI shows the comparison status (e.g., Pending/Checked/Warning).

---

### User Story 5 - Log range sessions and test results (Priority: P1)

As a user, I can log a range session for a specific recipe and enter shot data and accuracy metrics so I can measure progress over time.

**Why this priority**: “Progress with tests updates and report the improvement” requires structured test capture.

**Independent Test**: Create a session, add 10 shot rows, and verify computed aggregates (avg, SD, ES) update.

**Acceptance Scenarios**:

1. **Given** a recipe exists, **When** I start a range session, **Then** it is linked to that recipe and shows an “Active” status.
2. **Given** I enter shot velocities, **When** I add/edit/delete shot rows, **Then** the session aggregates update immediately.
3. **Given** I finalize a session, **When** I click “Finalize Session”, **Then** edits are locked (or require an explicit “Reopen”), and the session is included in analysis.

---

### User Story 6 - View progress and improvement reports (Priority: P2)

As a user, I can view reports that show whether my outcomes are improving (e.g., smaller groups, lower SD/ES, more consistent results) across sessions and recipe iterations.

**Why this priority**: This is the primary “value add” beyond simple note taking.

**Independent Test**: Create two sessions for the same recipe and verify that the report shows trend direction and deltas.

**Acceptance Scenarios**:

1. **Given** multiple sessions exist for a recipe, **When** I open analysis, **Then** I see trend summaries (e.g., last 90 days) and key deltas vs baseline.
2. **Given** a user selects a project and time range, **When** filters change, **Then** graphs and insights refresh within 1 second.
3. **Given** insufficient data, **When** I open analysis, **Then** I see an empty state explaining what data is needed.

---

### User Story 7 - Export and share a report (Priority: P2)

As a user, I can export a report (PDF/CSV) so I can save, print, or share my results.

**Why this priority**: The mockups include “Export Report” and the domain expects record keeping.

**Independent Test**: Create a recipe + session and export; verify the exported file includes expected fields and is generated within a reasonable time.

**Acceptance Scenarios**:

1. **Given** I have analysis data, **When** I export a PDF, **Then** I receive a downloadable document with summary + key charts.
2. **Given** I have session data, **When** I export CSV, **Then** the file includes normalized tables (sessions, shots) with stable column names.

---

### User Story 8 - Inventory tracking for components (Priority: P3)

As a user, I can track component inventory (e.g., cases, primers, powders, bullets) so I can see what I have available when creating recipes.

**Why this priority**: Present in the navigation mockups, but not required for MVP value.

**Independent Test**: Add an inventory item and confirm it appears in recipe component selectors.

**Acceptance Scenarios**:

1. **Given** I add inventory items, **When** I create a recipe, **Then** I can select from those items.
2. **Given** a recipe is marked as “loaded” with a quantity, **When** I confirm, **Then** inventory decrements accordingly (if configured).

---

### Edge Cases

- What happens when a user enters partial shot data (e.g., 2 shots only) and attempts to finalize a session?
- How does the system handle unit mismatches (e.g., user assumes metric vs imperial) and prevent silent errors?
- What happens if a user deletes a recipe that has sessions attached?
- How does the system handle concurrent edits (same recipe edited in two tabs)?
- What happens when exports are requested for very large datasets (e.g., 50k shot rows)?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST support user authentication (sign up, sign in, sign out).
- **FR-002**: System MUST protect user data with authorization checks so users only access their own projects/recipes/sessions.
- **FR-003**: System MUST allow users to create, view, update, and delete Projects.
- **FR-004**: System MUST allow users to create, view, update, duplicate, and delete Load Recipes under a Project.
- **FR-005**: System MUST store recipe fields with high precision appropriate for measurements (avoid rounding surprises).
- **FR-006**: System MUST allow users to create Range Sessions linked to a Load Recipe.
- **FR-007**: System MUST support entry of per-shot data (at minimum: sequence number + measured value + optional note).
- **FR-008**: System MUST compute and display session aggregates (at minimum: average, extreme spread, standard deviation) from shot rows.
- **FR-009**: System MUST allow users to record accuracy results (e.g., group size) and session conditions (environmental + setup notes).
- **FR-010**: System MUST provide an Analysis/Trends view that aggregates results across sessions and recipes and displays improvement deltas.
- **FR-011**: System MUST support exporting reports in at least one format (PDF or CSV); supporting both is preferred.
- **FR-012**: System MUST provide a searchable experience across projects, recipes, and sessions.

**Safety / domain guardrails (non-negotiable)**

- **FR-013**: System MUST NOT generate or recommend reloading values; it is a tracking/logging tool for user-provided data.
- **FR-014**: Any “Safety Check” workflow MUST be limited to validating user-entered data against user-provided reference bounds and MUST require a citation/source note (e.g., manual name + page or URL) if bounds are used.
- **FR-015**: System MUST prominently display a disclaimer stating users must follow manufacturer manuals and assume all safety responsibility.

**Usability requirements (aligned with mockups)**

- **FR-016**: System MUST provide the primary screens reflected in the mockups: Dashboard, Project Details (Load Recipes tab), New Recipe Entry, Range Test Data Entry, Analysis/Trends.
- **FR-017**: System MUST implement consistent empty/loading/error states on all primary screens.

### Key Entities *(include if feature involves data)*

- **User**: Auth identity, profile, preferences (units/theme), audit metadata.
- **Project**: Container for a caliber/platform goal; includes name, notes, tags, created/updated.
- **LoadRecipe**: User-defined recipe record containing component selections and numeric measurement fields; includes status (Draft/Development/Verified), version/iteration, notes, and timestamps.
- **SafetyReference**: Optional user-provided bounds and citation metadata used by “Safety Check” (no system-provided recommendations).
- **RangeSession**: Test event linked to a recipe; includes date/time, location (optional), environment fields (optional), and finalization state.
- **Shot**: Row item under a session (sequence + numeric measurement + note).
- **AccuracyResult**: Group size measurements and related metadata (distance, group count, etc.).
- **ReportExportJob**: Tracks export requests, completion, and downloadable artifacts.
- **InventoryItem** *(optional / P3)*: Component record for selection and optional decrementing.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A new user can sign up and reach the dashboard in ≤ 2 minutes.
- **SC-002**: A signed-in user can create a project and first load recipe in ≤ 3 minutes.
- **SC-003**: A user can enter 10 shot rows and finalize a range session in ≤ 2 minutes (excluding shooting time).
- **SC-004**: Analysis view renders project-level trends for up to 100 sessions and 10,000 shot rows in ≤ 2 seconds on a typical laptop.
- **SC-005**: Export completes in ≤ 15 seconds for a project with up to 10,000 shot rows; failure states are actionable.
- **SC-006**: Authorization is enforced for all protected resources (0 known IDOR-style access bugs in review/testing).
- **SC-007**: p95 API latency ≤ 300ms for typical read queries (dashboard/project/analysis) under expected course-scale usage.
