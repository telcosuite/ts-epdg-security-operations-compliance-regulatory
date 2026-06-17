# Security Operations, Compliance, And Regulatory P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold Development Tasks

Suite: Enterprise Platform, Data, And Governance

App: Security Operations, Compliance, And Regulatory

App slug: `security-operations-compliance-regulatory`

Implementation repository: `ts-epdg-security-operations-compliance-regulatory`

Phase: P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold

Phase file: `P02-business-continuity-and-operational-resilience-and-compliance-control-and-data.md`

Phase rationale: Build the Business Continuity And Operational Resilience, Compliance Control, Data Retention And Legal Hold capability cluster for Security Operations, Compliance, And Regulatory, carrying source workflows, APIs, events, tables, controls, and tests from the feature files into implementable work.

Phase exit gate: Security Operations, Compliance, And Regulatory can execute the Business Continuity And Operational Resilience, Compliance Control, Data Retention And Legal Hold workflows through UI, API, `security_compliance_regulatory` persistence, outbox events, audit evidence, and release tests.

Out of scope for this phase: Runtime bootstrap is in P01; unrelated feature clusters and post-launch operations remain in their own phases.

Source tracker: [development-task-tracker.md](development-task-tracker.md)

Repository strategy: [TelcoSuite Repository Strategy](../../../../repository-strategy.md)

## Phase Coverage

- [Business Continuity And Operational Resilience](../features/business-continuity-and-operational-resilience.md)
- [Compliance Control](../features/compliance-control.md)
- [Data Retention And Legal Hold](../features/data-retention-and-legal-hold.md)

## Phase Tasks

### DT-06-security-operations-compliance-regulatory-P02-T001: Build Business Continuity And Operational Resilience API, data model, workflow, and event spine

| Field | Value |
| --- | --- |
| Phase | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| Priority | P0 |
| Source evidence | [Business Continuity And Operational Resilience](../features/business-continuity-and-operational-resilience.md), [Implementation usage](../implementation-file-usage.md), [App README](../README.md), [App overview](../../security-operations-compliance-regulatory.md), [Modules and features](../modules-and-features.md), [Personas and journeys](../personas-and-user-journeys.md), [Suite tech/UI guidance](../../tech-and-ui-guidance.md), [Suite data model](../../data-model.md), [Suite implementation guide](../../implementation-file-usage-guide.md), [Repository strategy](../../../../repository-strategy.md) |
| Feature or module | Business Continuity And Operational Resilience |
| Build area | API/Data/Event/Workflow/Security/Test |
| Target artifact | `backend/src/main/java/com/telcosuite/enterpriseplatformdatagovernance/securityoperationscomplianceregulatory/BusinessContinuityAndOperationalResilienceController.java`, `security_compliance_regulatory.business_continuity_and_operational_resilience`, `contracts/events/BusinessContinuityAndOperationalResilienceStateChangedEvent.json`, and `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/business-continuity-and-operational-resilience` |
| Dependencies | DT-06-security-operations-compliance-regulatory-P01-T013 |
| Outputs | `BusinessContinuityAndOperationalResilienceController`, `BusinessContinuityAndOperationalResilienceService`, `security_compliance_regulatory.business_continuity_and_operational_resilience` migration, `BusinessContinuityAndOperationalResilienceStateChangedEvent` outbox schema, OpenAPI operations, unit/contract/migration/event replay tests |
| Missing evidence | No |

#### Implementation Notes

- Implement command and query APIs for `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/business-continuity-and-operational-resilience` using TMF655, TMF707, TMF724, with create, update, search, detail, lifecycle transition, timeline, evidence, and exception endpoints where the feature lifecycle requires them.
- Persist `Business Continuity And Operational Resilience` state in `security_compliance_regulatory.business_continuity_and_operational_resilience` with tenant, brand/market, lifecycle state, source authority, idempotency key, correlation ID, actor, reason code, audit fields, and `tmf_payload` JSONB.
- Publish `BusinessContinuityAndOperationalResilienceStateChangedEvent` through the transactional outbox with changed fields, replay metadata, consumer acknowledgement state, and reconciliation status for workflows: Resilience assessment, Continuity exercise, Failover validation, Resilience remediation.
- Carry source details into code and tests for personas SOC analyst, Compliance officer, Regulatory reporting owner and objects resilience plan, critical dependency, RTO/RPO objective, exercise plan; keep cross-app references read-only unless they arrive through governed APIs/events/projections.

#### Acceptance Criteria

1. Given an authorized persona submits `POST /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/business-continuity-and-operational-resilience`, when required fields and policy checks pass, then the API returns `201` with `$.state`, persists `security_compliance_regulatory.business_continuity_and_operational_resilience.id`, and appends `BusinessContinuityAndOperationalResilienceStateChangedEvent` to `security_compliance_regulatory.event_outbox`.
2. Given a stale, duplicate, or out-of-order request hits `PATCH /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/business-continuity-and-operational-resilience/{id}`, when optimistic locking or idempotency validation fails, then the API returns `409` with `$.error.code='stale-or-duplicate-command'` and no second event is emitted.
3. Given another app needs `Business Continuity And Operational Resilience` state, when it requests data, then it receives TMF-aligned API/event/projection output and no direct database access to `security_compliance_regulatory.business_continuity_and_operational_resilience` is required.

#### Definition Of Done

- `BusinessContinuityAndOperationalResilienceController`, service, repository, DTOs, validation, error model, and migration for `security_compliance_regulatory.business_continuity_and_operational_resilience` are committed under `ts-epdg-security-operations-compliance-regulatory`.
- OpenAPI contract tests, unit tests, Flyway migration tests, event schema tests, and event replay tests cover `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/business-continuity-and-operational-resilience`, `security_compliance_regulatory.business_continuity_and_operational_resilience`, and `BusinessContinuityAndOperationalResilienceStateChangedEvent`.
- `development-task-tracker.md` records command output, source feature link, PR/evidence links, and any blocked downstream consumer.

#### Negative Scenarios

- Unauthorized, cross-tenant, or wrong-purpose requests to `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/business-continuity-and-operational-resilience` return `403` and write a denial audit row instead of exposing `Business Continuity And Operational Resilience` data.
- Missing source authority, stale dependency state, invalid lifecycle transition, or failed policy decision keeps `security_compliance_regulatory.business_continuity_and_operational_resilience` in blocked/exception state with owner and due date.
- Downstream outage or consumer rejection queues retry/replay for `BusinessContinuityAndOperationalResilienceStateChangedEvent` and prevents silent completion.

#### Edge Cases

- Bulk or project-scale updates to `Business Continuity And Operational Resilience` use preview, partial-failure reporting, idempotency keys, rollback/repair notes, and async export where needed.
- Historical correction preserves previous `security_compliance_regulatory.business_continuity_and_operational_resilience` values, audit reason, source timestamp, actor, and downstream recalculation/replay instructions.
- Multi-tenant, market, residency, localization, and high-volume queue cases include pagination, back-pressure, circuit breaker, and replay controls.

#### Test Expectations

- `mvn test` covers `BusinessContinuityAndOperationalResilienceService`, validation, authorization, idempotency, and lifecycle transition rules.
- OpenAPI contract tests call `POST/GET/PATCH /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/business-continuity-and-operational-resilience` and verify `$.state`, `$.id`, error payloads, and pagination/filter behavior.
- Flyway migration tests verify `security_compliance_regulatory.business_continuity_and_operational_resilience` columns and indexes; event replay tests validate `contracts/events/BusinessContinuityAndOperationalResilienceStateChangedEvent.json` and `security_compliance_regulatory.event_outbox` ordering.

### DT-06-security-operations-compliance-regulatory-P02-T002: Build Business Continuity And Operational Resilience workbench, controls, observability, and release tests

| Field | Value |
| --- | --- |
| Phase | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| Priority | P1 |
| Source evidence | [Business Continuity And Operational Resilience](../features/business-continuity-and-operational-resilience.md), [Implementation usage](../implementation-file-usage.md), [App README](../README.md), [App overview](../../security-operations-compliance-regulatory.md), [Modules and features](../modules-and-features.md), [Personas and journeys](../personas-and-user-journeys.md), [Suite tech/UI guidance](../../tech-and-ui-guidance.md), [Suite data model](../../data-model.md), [Suite implementation guide](../../implementation-file-usage-guide.md), [Repository strategy](../../../../repository-strategy.md) |
| Feature or module | Business Continuity And Operational Resilience |
| Build area | UI/Security/Ops/Test |
| Target artifact | `frontend/src/app/pages/business-continuity-and-operational-resilience/`, `tests/e2e/business-continuity-and-operational-resilience.spec.ts`, Grafana panel `business-continuity-and-operational-resilience`, and `docs/operations-runbook.md#business-continuity-and-operational-resilience` |
| Dependencies | DT-06-security-operations-compliance-regulatory-P02-T001 |
| Outputs | Angular workbench, queue/detail/timeline/evidence panels, role-aware guards, accessibility states, E2E tests, dashboard JSON, alert rules, runbook section |
| Missing evidence | No |

#### Implementation Notes

- Create `frontend/src/app/pages/business-continuity-and-operational-resilience/` with search/intake, detail, lifecycle timeline, exception queue, evidence drawer, dependency freshness panel, and allowed-next-action controls for personas SOC analyst, Compliance officer, Regulatory reporting owner.
- Wire route guards, tenant/brand/market context, masking, no-permission states, keyboard navigation, PrimeNG table/form patterns, and saved filters using `ts-shared-ui-design-system`.
- Add dashboard metrics and runbook steps for workflows Resilience assessment, Continuity exercise, Failover validation, Resilience remediation, event replay backlog, queue aging, policy denials, consumer lag, and completion quality.

#### Acceptance Criteria

1. Given an authorized persona opens `/app/security-operations-compliance-regulatory/business-continuity-and-operational-resilience`, when records exist, then the workbench returns `$.uiState='ready'` and renders `Business Continuity And Operational Resilience` rows with lifecycle state, owner, freshness, SLA/OLA timer, and action menu.
2. Given the persona lacks permission, when the same route loads, then the UI shows a no-permission state and the backend returns `403` with `$.error.code='access-denied'`.
3. Given replay backlog or queue aging exceeds threshold, when Grafana dashboard `business-continuity-and-operational-resilience` refreshes, then it shows the metric and links to `docs/operations-runbook.md#business-continuity-and-operational-resilience`.

#### Definition Of Done

- `frontend/src/app/pages/business-continuity-and-operational-resilience/` includes route, component, service, state, fixtures, empty/loading/error/no-permission states, and accessibility labels.
- `tests/e2e/business-continuity-and-operational-resilience.spec.ts`, accessibility checks, security tests, dashboard checks, and runbook review pass and are linked from the tracker.
- `development-task-tracker.md` captures screenshots, command output, PR links, dashboard/runbook links, and unresolved blockers.

#### Negative Scenarios

- Do not render `Business Continuity And Operational Resilience` details across tenant/residency boundaries; masked values stay masked in table, detail, export, timeline, and dashboard paths.
- Do not close UI actions when backend validation, event publication, reconciliation, or required evidence is incomplete.
- Do not hide downstream outage, stale source data, policy denial, or manual override behind a generic success toast.

#### Edge Cases

- Mobile or constrained layouts for `Business Continuity And Operational Resilience` collapse tables into accessible cards without losing lifecycle, owner, SLA/OLA, or evidence fields.
- Bulk/replay actions require preview, explicit confirmation, partial-failure details, rollback/repair notes, and operator evidence.
- High-volume dashboard and queue views use pagination, saved filters, async export, trace IDs, and back-pressure indicators.

#### Test Expectations

- `npm run lint`, `npm test`, and `tests/e2e/business-continuity-and-operational-resilience.spec.ts` validate route, forms, guards, workbench states, and API integration.
- Accessibility tests cover keyboard navigation, focus order, screen-reader labels, color contrast, density, and responsive layout.
- Operational-readiness tests validate Grafana dashboard JSON, alert rules, event replay panel, runbook links, and release evidence.

### DT-06-security-operations-compliance-regulatory-P02-T003: Build Compliance Control API, data model, workflow, and event spine

| Field | Value |
| --- | --- |
| Phase | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| Priority | P0 |
| Source evidence | [Compliance Control](../features/compliance-control.md), [Implementation usage](../implementation-file-usage.md), [App README](../README.md), [App overview](../../security-operations-compliance-regulatory.md), [Modules and features](../modules-and-features.md), [Personas and journeys](../personas-and-user-journeys.md), [Suite tech/UI guidance](../../tech-and-ui-guidance.md), [Suite data model](../../data-model.md), [Suite implementation guide](../../implementation-file-usage-guide.md), [Repository strategy](../../../../repository-strategy.md) |
| Feature or module | Compliance Control |
| Build area | API/Data/Event/Workflow/Security/Test |
| Target artifact | `backend/src/main/java/com/telcosuite/enterpriseplatformdatagovernance/securityoperationscomplianceregulatory/ComplianceControlController.java`, `security_compliance_regulatory.compliance_control`, `contracts/events/ComplianceControlStateChangedEvent.json`, and `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/compliance-control` |
| Dependencies | DT-06-security-operations-compliance-regulatory-P02-T001 |
| Outputs | `ComplianceControlController`, `ComplianceControlService`, `security_compliance_regulatory.compliance_control` migration, `ComplianceControlStateChangedEvent` outbox schema, OpenAPI operations, unit/contract/migration/event replay tests |
| Missing evidence | No |

#### Implementation Notes

- Implement command and query APIs for `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/compliance-control` using TMF644, TMF667, TMF696, TMF707, TMF710, with create, update, search, detail, lifecycle transition, timeline, evidence, and exception endpoints where the feature lifecycle requires them.
- Persist `Compliance Control` state in `security_compliance_regulatory.compliance_control` with tenant, brand/market, lifecycle state, source authority, idempotency key, correlation ID, actor, reason code, audit fields, and `tmf_payload` JSONB.
- Publish `ComplianceControlStateChangedEvent` through the transactional outbox with changed fields, replay metadata, consumer acknowledgement state, and reconciliation status for workflows: Control definition, Control testing, Attestation campaign, Remediation tracking.
- Carry source details into code and tests for personas SOC analyst, Compliance officer, Regulatory reporting owner and objects compliance control, control test, attestation, exception; keep cross-app references read-only unless they arrive through governed APIs/events/projections.

#### Acceptance Criteria

1. Given an authorized persona submits `POST /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/compliance-control`, when required fields and policy checks pass, then the API returns `201` with `$.state`, persists `security_compliance_regulatory.compliance_control.id`, and appends `ComplianceControlStateChangedEvent` to `security_compliance_regulatory.event_outbox`.
2. Given a stale, duplicate, or out-of-order request hits `PATCH /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/compliance-control/{id}`, when optimistic locking or idempotency validation fails, then the API returns `409` with `$.error.code='stale-or-duplicate-command'` and no second event is emitted.
3. Given another app needs `Compliance Control` state, when it requests data, then it receives TMF-aligned API/event/projection output and no direct database access to `security_compliance_regulatory.compliance_control` is required.

#### Definition Of Done

- `ComplianceControlController`, service, repository, DTOs, validation, error model, and migration for `security_compliance_regulatory.compliance_control` are committed under `ts-epdg-security-operations-compliance-regulatory`.
- OpenAPI contract tests, unit tests, Flyway migration tests, event schema tests, and event replay tests cover `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/compliance-control`, `security_compliance_regulatory.compliance_control`, and `ComplianceControlStateChangedEvent`.
- `development-task-tracker.md` records command output, source feature link, PR/evidence links, and any blocked downstream consumer.

#### Negative Scenarios

- Unauthorized, cross-tenant, or wrong-purpose requests to `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/compliance-control` return `403` and write a denial audit row instead of exposing `Compliance Control` data.
- Missing source authority, stale dependency state, invalid lifecycle transition, or failed policy decision keeps `security_compliance_regulatory.compliance_control` in blocked/exception state with owner and due date.
- Downstream outage or consumer rejection queues retry/replay for `ComplianceControlStateChangedEvent` and prevents silent completion.

#### Edge Cases

- Bulk or project-scale updates to `Compliance Control` use preview, partial-failure reporting, idempotency keys, rollback/repair notes, and async export where needed.
- Historical correction preserves previous `security_compliance_regulatory.compliance_control` values, audit reason, source timestamp, actor, and downstream recalculation/replay instructions.
- Multi-tenant, market, residency, localization, and high-volume queue cases include pagination, back-pressure, circuit breaker, and replay controls.

#### Test Expectations

- `mvn test` covers `ComplianceControlService`, validation, authorization, idempotency, and lifecycle transition rules.
- OpenAPI contract tests call `POST/GET/PATCH /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/compliance-control` and verify `$.state`, `$.id`, error payloads, and pagination/filter behavior.
- Flyway migration tests verify `security_compliance_regulatory.compliance_control` columns and indexes; event replay tests validate `contracts/events/ComplianceControlStateChangedEvent.json` and `security_compliance_regulatory.event_outbox` ordering.

### DT-06-security-operations-compliance-regulatory-P02-T004: Build Compliance Control workbench, controls, observability, and release tests

| Field | Value |
| --- | --- |
| Phase | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| Priority | P1 |
| Source evidence | [Compliance Control](../features/compliance-control.md), [Implementation usage](../implementation-file-usage.md), [App README](../README.md), [App overview](../../security-operations-compliance-regulatory.md), [Modules and features](../modules-and-features.md), [Personas and journeys](../personas-and-user-journeys.md), [Suite tech/UI guidance](../../tech-and-ui-guidance.md), [Suite data model](../../data-model.md), [Suite implementation guide](../../implementation-file-usage-guide.md), [Repository strategy](../../../../repository-strategy.md) |
| Feature or module | Compliance Control |
| Build area | UI/Security/Ops/Test |
| Target artifact | `frontend/src/app/pages/compliance-control/`, `tests/e2e/compliance-control.spec.ts`, Grafana panel `compliance-control`, and `docs/operations-runbook.md#compliance-control` |
| Dependencies | DT-06-security-operations-compliance-regulatory-P02-T003 |
| Outputs | Angular workbench, queue/detail/timeline/evidence panels, role-aware guards, accessibility states, E2E tests, dashboard JSON, alert rules, runbook section |
| Missing evidence | No |

#### Implementation Notes

- Create `frontend/src/app/pages/compliance-control/` with search/intake, detail, lifecycle timeline, exception queue, evidence drawer, dependency freshness panel, and allowed-next-action controls for personas SOC analyst, Compliance officer, Regulatory reporting owner.
- Wire route guards, tenant/brand/market context, masking, no-permission states, keyboard navigation, PrimeNG table/form patterns, and saved filters using `ts-shared-ui-design-system`.
- Add dashboard metrics and runbook steps for workflows Control definition, Control testing, Attestation campaign, Remediation tracking, event replay backlog, queue aging, policy denials, consumer lag, and completion quality.

#### Acceptance Criteria

1. Given an authorized persona opens `/app/security-operations-compliance-regulatory/compliance-control`, when records exist, then the workbench returns `$.uiState='ready'` and renders `Compliance Control` rows with lifecycle state, owner, freshness, SLA/OLA timer, and action menu.
2. Given the persona lacks permission, when the same route loads, then the UI shows a no-permission state and the backend returns `403` with `$.error.code='access-denied'`.
3. Given replay backlog or queue aging exceeds threshold, when Grafana dashboard `compliance-control` refreshes, then it shows the metric and links to `docs/operations-runbook.md#compliance-control`.

#### Definition Of Done

- `frontend/src/app/pages/compliance-control/` includes route, component, service, state, fixtures, empty/loading/error/no-permission states, and accessibility labels.
- `tests/e2e/compliance-control.spec.ts`, accessibility checks, security tests, dashboard checks, and runbook review pass and are linked from the tracker.
- `development-task-tracker.md` captures screenshots, command output, PR links, dashboard/runbook links, and unresolved blockers.

#### Negative Scenarios

- Do not render `Compliance Control` details across tenant/residency boundaries; masked values stay masked in table, detail, export, timeline, and dashboard paths.
- Do not close UI actions when backend validation, event publication, reconciliation, or required evidence is incomplete.
- Do not hide downstream outage, stale source data, policy denial, or manual override behind a generic success toast.

#### Edge Cases

- Mobile or constrained layouts for `Compliance Control` collapse tables into accessible cards without losing lifecycle, owner, SLA/OLA, or evidence fields.
- Bulk/replay actions require preview, explicit confirmation, partial-failure details, rollback/repair notes, and operator evidence.
- High-volume dashboard and queue views use pagination, saved filters, async export, trace IDs, and back-pressure indicators.

#### Test Expectations

- `npm run lint`, `npm test`, and `tests/e2e/compliance-control.spec.ts` validate route, forms, guards, workbench states, and API integration.
- Accessibility tests cover keyboard navigation, focus order, screen-reader labels, color contrast, density, and responsive layout.
- Operational-readiness tests validate Grafana dashboard JSON, alert rules, event replay panel, runbook links, and release evidence.

### DT-06-security-operations-compliance-regulatory-P02-T005: Build Data Retention And Legal Hold API, data model, workflow, and event spine

| Field | Value |
| --- | --- |
| Phase | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| Priority | P0 |
| Source evidence | [Data Retention And Legal Hold](../features/data-retention-and-legal-hold.md), [Implementation usage](../implementation-file-usage.md), [App README](../README.md), [App overview](../../security-operations-compliance-regulatory.md), [Modules and features](../modules-and-features.md), [Personas and journeys](../personas-and-user-journeys.md), [Suite tech/UI guidance](../../tech-and-ui-guidance.md), [Suite data model](../../data-model.md), [Suite implementation guide](../../implementation-file-usage-guide.md), [Repository strategy](../../../../repository-strategy.md) |
| Feature or module | Data Retention And Legal Hold |
| Build area | API/Data/Event/Workflow/Security/Test |
| Target artifact | `backend/src/main/java/com/telcosuite/enterpriseplatformdatagovernance/securityoperationscomplianceregulatory/DataRetentionAndLegalHoldController.java`, `security_compliance_regulatory.data_retention_and_legal_hold`, `contracts/events/DataRetentionAndLegalHoldStateChangedEvent.json`, and `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/data-retention-and-legal-hold` |
| Dependencies | DT-06-security-operations-compliance-regulatory-P02-T003 |
| Outputs | `DataRetentionAndLegalHoldController`, `DataRetentionAndLegalHoldService`, `security_compliance_regulatory.data_retention_and_legal_hold` migration, `DataRetentionAndLegalHoldStateChangedEvent` outbox schema, OpenAPI operations, unit/contract/migration/event replay tests |
| Missing evidence | No |

#### Implementation Notes

- Implement command and query APIs for `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/data-retention-and-legal-hold` using TMF644, TMF667, TMF735, with create, update, search, detail, lifecycle transition, timeline, evidence, and exception endpoints where the feature lifecycle requires them.
- Persist `Data Retention And Legal Hold` state in `security_compliance_regulatory.data_retention_and_legal_hold` with tenant, brand/market, lifecycle state, source authority, idempotency key, correlation ID, actor, reason code, audit fields, and `tmf_payload` JSONB.
- Publish `DataRetentionAndLegalHoldStateChangedEvent` through the transactional outbox with changed fields, replay metadata, consumer acknowledgement state, and reconciliation status for workflows: Retention policy definition, Legal hold placement, Deletion eligibility review, Evidence export.
- Carry source details into code and tests for personas SOC analyst, Compliance officer, Regulatory reporting owner and objects retention policy, retention schedule, legal hold, deletion eligibility decision; keep cross-app references read-only unless they arrive through governed APIs/events/projections.

#### Acceptance Criteria

1. Given an authorized persona submits `POST /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/data-retention-and-legal-hold`, when required fields and policy checks pass, then the API returns `201` with `$.state`, persists `security_compliance_regulatory.data_retention_and_legal_hold.id`, and appends `DataRetentionAndLegalHoldStateChangedEvent` to `security_compliance_regulatory.event_outbox`.
2. Given a stale, duplicate, or out-of-order request hits `PATCH /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/data-retention-and-legal-hold/{id}`, when optimistic locking or idempotency validation fails, then the API returns `409` with `$.error.code='stale-or-duplicate-command'` and no second event is emitted.
3. Given another app needs `Data Retention And Legal Hold` state, when it requests data, then it receives TMF-aligned API/event/projection output and no direct database access to `security_compliance_regulatory.data_retention_and_legal_hold` is required.

#### Definition Of Done

- `DataRetentionAndLegalHoldController`, service, repository, DTOs, validation, error model, and migration for `security_compliance_regulatory.data_retention_and_legal_hold` are committed under `ts-epdg-security-operations-compliance-regulatory`.
- OpenAPI contract tests, unit tests, Flyway migration tests, event schema tests, and event replay tests cover `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/data-retention-and-legal-hold`, `security_compliance_regulatory.data_retention_and_legal_hold`, and `DataRetentionAndLegalHoldStateChangedEvent`.
- `development-task-tracker.md` records command output, source feature link, PR/evidence links, and any blocked downstream consumer.

#### Negative Scenarios

- Unauthorized, cross-tenant, or wrong-purpose requests to `/api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/data-retention-and-legal-hold` return `403` and write a denial audit row instead of exposing `Data Retention And Legal Hold` data.
- Missing source authority, stale dependency state, invalid lifecycle transition, or failed policy decision keeps `security_compliance_regulatory.data_retention_and_legal_hold` in blocked/exception state with owner and due date.
- Downstream outage or consumer rejection queues retry/replay for `DataRetentionAndLegalHoldStateChangedEvent` and prevents silent completion.

#### Edge Cases

- Bulk or project-scale updates to `Data Retention And Legal Hold` use preview, partial-failure reporting, idempotency keys, rollback/repair notes, and async export where needed.
- Historical correction preserves previous `security_compliance_regulatory.data_retention_and_legal_hold` values, audit reason, source timestamp, actor, and downstream recalculation/replay instructions.
- Multi-tenant, market, residency, localization, and high-volume queue cases include pagination, back-pressure, circuit breaker, and replay controls.

#### Test Expectations

- `mvn test` covers `DataRetentionAndLegalHoldService`, validation, authorization, idempotency, and lifecycle transition rules.
- OpenAPI contract tests call `POST/GET/PATCH /api/06-enterprise-platform-data-governance/security-operations-compliance-regulatory/v1/data-retention-and-legal-hold` and verify `$.state`, `$.id`, error payloads, and pagination/filter behavior.
- Flyway migration tests verify `security_compliance_regulatory.data_retention_and_legal_hold` columns and indexes; event replay tests validate `contracts/events/DataRetentionAndLegalHoldStateChangedEvent.json` and `security_compliance_regulatory.event_outbox` ordering.

### DT-06-security-operations-compliance-regulatory-P02-T006: Build Data Retention And Legal Hold workbench, controls, observability, and release tests

| Field | Value |
| --- | --- |
| Phase | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| Priority | P1 |
| Source evidence | [Data Retention And Legal Hold](../features/data-retention-and-legal-hold.md), [Implementation usage](../implementation-file-usage.md), [App README](../README.md), [App overview](../../security-operations-compliance-regulatory.md), [Modules and features](../modules-and-features.md), [Personas and journeys](../personas-and-user-journeys.md), [Suite tech/UI guidance](../../tech-and-ui-guidance.md), [Suite data model](../../data-model.md), [Suite implementation guide](../../implementation-file-usage-guide.md), [Repository strategy](../../../../repository-strategy.md) |
| Feature or module | Data Retention And Legal Hold |
| Build area | UI/Security/Ops/Test |
| Target artifact | `frontend/src/app/pages/data-retention-and-legal-hold/`, `tests/e2e/data-retention-and-legal-hold.spec.ts`, Grafana panel `data-retention-and-legal-hold`, and `docs/operations-runbook.md#data-retention-and-legal-hold` |
| Dependencies | DT-06-security-operations-compliance-regulatory-P02-T005 |
| Outputs | Angular workbench, queue/detail/timeline/evidence panels, role-aware guards, accessibility states, E2E tests, dashboard JSON, alert rules, runbook section |
| Missing evidence | No |

#### Implementation Notes

- Create `frontend/src/app/pages/data-retention-and-legal-hold/` with search/intake, detail, lifecycle timeline, exception queue, evidence drawer, dependency freshness panel, and allowed-next-action controls for personas SOC analyst, Compliance officer, Regulatory reporting owner.
- Wire route guards, tenant/brand/market context, masking, no-permission states, keyboard navigation, PrimeNG table/form patterns, and saved filters using `ts-shared-ui-design-system`.
- Add dashboard metrics and runbook steps for workflows Retention policy definition, Legal hold placement, Deletion eligibility review, Evidence export, event replay backlog, queue aging, policy denials, consumer lag, and completion quality.

#### Acceptance Criteria

1. Given an authorized persona opens `/app/security-operations-compliance-regulatory/data-retention-and-legal-hold`, when records exist, then the workbench returns `$.uiState='ready'` and renders `Data Retention And Legal Hold` rows with lifecycle state, owner, freshness, SLA/OLA timer, and action menu.
2. Given the persona lacks permission, when the same route loads, then the UI shows a no-permission state and the backend returns `403` with `$.error.code='access-denied'`.
3. Given replay backlog or queue aging exceeds threshold, when Grafana dashboard `data-retention-and-legal-hold` refreshes, then it shows the metric and links to `docs/operations-runbook.md#data-retention-and-legal-hold`.

#### Definition Of Done

- `frontend/src/app/pages/data-retention-and-legal-hold/` includes route, component, service, state, fixtures, empty/loading/error/no-permission states, and accessibility labels.
- `tests/e2e/data-retention-and-legal-hold.spec.ts`, accessibility checks, security tests, dashboard checks, and runbook review pass and are linked from the tracker.
- `development-task-tracker.md` captures screenshots, command output, PR links, dashboard/runbook links, and unresolved blockers.

#### Negative Scenarios

- Do not render `Data Retention And Legal Hold` details across tenant/residency boundaries; masked values stay masked in table, detail, export, timeline, and dashboard paths.
- Do not close UI actions when backend validation, event publication, reconciliation, or required evidence is incomplete.
- Do not hide downstream outage, stale source data, policy denial, or manual override behind a generic success toast.

#### Edge Cases

- Mobile or constrained layouts for `Data Retention And Legal Hold` collapse tables into accessible cards without losing lifecycle, owner, SLA/OLA, or evidence fields.
- Bulk/replay actions require preview, explicit confirmation, partial-failure details, rollback/repair notes, and operator evidence.
- High-volume dashboard and queue views use pagination, saved filters, async export, trace IDs, and back-pressure indicators.

#### Test Expectations

- `npm run lint`, `npm test`, and `tests/e2e/data-retention-and-legal-hold.spec.ts` validate route, forms, guards, workbench states, and API integration.
- Accessibility tests cover keyboard navigation, focus order, screen-reader labels, color contrast, density, and responsive layout.
- Operational-readiness tests validate Grafana dashboard JSON, alert rules, event replay panel, runbook links, and release evidence.

### DT-06-security-operations-compliance-regulatory-P02-T007: Prove Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold release gate, replay, and handoff evidence

| Field | Value |
| --- | --- |
| Phase | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| Priority | P1 |
| Source evidence | [Business Continuity And Operational Resilience](../features/business-continuity-and-operational-resilience.md), [Compliance Control](../features/compliance-control.md), [Data Retention And Legal Hold](../features/data-retention-and-legal-hold.md), [Implementation usage](../implementation-file-usage.md), [App README](../README.md), [App overview](../../security-operations-compliance-regulatory.md), [Modules and features](../modules-and-features.md), [Personas and journeys](../personas-and-user-journeys.md), [Suite tech/UI guidance](../../tech-and-ui-guidance.md), [Suite data model](../../data-model.md), [Suite implementation guide](../../implementation-file-usage-guide.md), [Repository strategy](../../../../repository-strategy.md) |
| Feature or module | Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| Build area | Test/Ops/Release/Event |
| Target artifact | `tests/release/business-continuity-and-operational-resilience-and-compliance-control-and-data.spec.ts`, `docs/release-notes/business-continuity-and-operational-resilience-and-compliance-control-and-data.md`, Grafana dashboard `business-continuity-and-operational-resilience-and-compliance-control-and-data`, and replay fixtures |
| Dependencies | DT-06-security-operations-compliance-regulatory-P02-T002, DT-06-security-operations-compliance-regulatory-P02-T004, DT-06-security-operations-compliance-regulatory-P02-T006 |
| Outputs | Release-gate test, replay/reconciliation evidence, accessibility/security/performance reports, dashboard/runbook links, support handoff notes |
| Missing evidence | No |

#### Implementation Notes

- Create a release-gate checklist for `business-continuity-and-operational-resilience-and-compliance-control-and-data` covering Business Continuity And Operational Resilience, Compliance Control, Data Retention And Legal Hold, with happy path, assisted path, negative path, edge cases, event replay, data reconciliation, security, accessibility, performance, and support evidence.
- Record producer and consumer acknowledgements for phase events, reconcile `security_compliance_regulatory.event_outbox`, and link replay fixtures and correlation IDs.
- Update `docs/operations-runbook.md`, `docs/release-notes/business-continuity-and-operational-resilience-and-compliance-control-and-data.md`, and `development-task-tracker.md` with release evidence and unresolved blockers.

#### Acceptance Criteria

1. Given all tasks in `P02-business-continuity-and-operational-resilience-and-compliance-control-and-data.md` are complete, when `tests/release/business-continuity-and-operational-resilience-and-compliance-control-and-data.spec.ts` runs, then it returns exit code `0` and links evidence for UI, API, data, event, security, ops, and test gates.
2. Given a consumer rejects an event from `business-continuity-and-operational-resilience-and-compliance-control-and-data`, when replay is triggered, then the replay fixture preserves `$.correlationId`, `$.eventId`, and consumer acknowledgement state.
3. Given release notes are generated, when support reviews `docs/release-notes/business-continuity-and-operational-resilience-and-compliance-control-and-data.md`, then open blockers, rollback steps, runbook links, and ownership contacts are present.

#### Definition Of Done

- `tests/release/business-continuity-and-operational-resilience-and-compliance-control-and-data.spec.ts`, replay fixtures, dashboard/runbook links, and release notes are committed.
- Accessibility, security, contract, migration, event replay, performance, and operational-readiness evidence is linked from the tracker.
- Open blockers have owner, due date, target increment, and rollback or removal criteria.

#### Negative Scenarios

- Do not mark the phase Done if event replay, reconciliation, accessibility, security, or downstream acknowledgement evidence is missing.
- Do not release `business-continuity-and-operational-resilience-and-compliance-control-and-data` with unresolved cross-app writes, direct schema coupling, or stale source authority assumptions.
- Do not suppress failed release gates; record failures with owner, due date, and target increment.

#### Edge Cases

- Coordinated release gates may require downstream app windows; record scheduling, owner, and fallback route in release notes.
- Historical backfill, replay, bulk update, or migration repair runs must include preview, partial failure report, and rollback evidence.
- High-volume launch periods require dashboard thresholds, alert owners, queue back-pressure, and support escalation paths.

#### Test Expectations

- `tests/release/business-continuity-and-operational-resilience-and-compliance-control-and-data.spec.ts`, `mvn test`, OpenAPI/event replay tests, Flyway checks, Playwright/Cypress E2E, accessibility, security, and k6/performance gates pass.
- `docker compose config`, clean-checkout smoke, `helm lint`, Kubernetes dry-run, dashboard JSON validation, and runbook link checks pass.
- Tracker evidence links command output, PRs, screenshots, replay payloads, dashboards, release notes, and support handoff notes.
