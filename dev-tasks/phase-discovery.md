# Security Operations, Compliance, And Regulatory Phase Discovery

## App Identity

| Field | Value |
| --- | --- |
| Suite | Enterprise Platform, Data, And Governance |
| App | Security Operations, Compliance, And Regulatory |
| App slug | `security-operations-compliance-regulatory` |
| Implementation repo | `ts-epdg-security-operations-compliance-regulatory` |
| Database | `ts_enterprise_platform_governance` |
| Schema | `security_compliance_regulatory` |
| APIs | TMF720, TMF672, TMF696, TMF621, TMF644, TMF667, TMF707, TMF710 |
| Generated date | 2026-06-17 |
| Phase/task signature | 4 phases / P01=14, P02=7, P03=7, P04=5 |

Phase count decision: 4 phases are evidence-derived from the current app-repo state, P01 runtime bootstrap requirements, and 8 build-ready feature files grouped by lifecycle, UI/API/data/event ownership, integration risk, and release gates.

Repeated skeleton audit: Evidence-derived and accepted for this app. Even when another app shares a phase/task-count signature, this discovery file cites this app's feature files, phase files, current repo state, and split/merge decisions; regenerate and split or merge phases if those inputs change.

## Input Evidence Inventory

| Evidence | Link | Status |
| --- | --- | --- |
| App implementation usage | [../implementation-file-usage.md](../implementation-file-usage.md) | Present |
| App README | [../README.md](../README.md) | Present |
| Modules and features | [../modules-and-features.md](../modules-and-features.md) | Present |
| Personas and journeys | [../personas-and-user-journeys.md](../personas-and-user-journeys.md) | Present |
| Suite data model | [../../data-model.md](../../data-model.md) | Present |
| Suite tech/UI guidance | [../../tech-and-ui-guidance.md](../../tech-and-ui-guidance.md) | Present |
| Suite implementation guide | [../../implementation-file-usage-guide.md](../../implementation-file-usage-guide.md) | Present |
| Repository strategy | [../../../../repository-strategy.md](../../../../repository-strategy.md) | Present |
| Feature: Business Continuity And Operational Resilience | [../features/business-continuity-and-operational-resilience.md](../features/business-continuity-and-operational-resilience.md) | Present |
| Feature: Compliance Control | [../features/compliance-control.md](../features/compliance-control.md) | Present |
| Feature: Data Retention And Legal Hold | [../features/data-retention-and-legal-hold.md](../features/data-retention-and-legal-hold.md) | Present |
| Feature: Export Control Audit Evidence And Resilience Response | [../features/export-control-audit-evidence-and-resilience-response.md](../features/export-control-audit-evidence-and-resilience-response.md) | Present |
| Feature: Privacy Breach Lawful Intercept And Legal Requests | [../features/privacy-breach-lawful-intercept-and-legal-requests.md](../features/privacy-breach-lawful-intercept-and-legal-requests.md) | Present |
| Feature: Regulatory Operations | [../features/regulatory-operations.md](../features/regulatory-operations.md) | Present |
| Feature: Security Monitoring And Incident | [../features/security-monitoring-and-incident.md](../features/security-monitoring-and-incident.md) | Present |
| Feature: Vulnerability Patch Vendor Risk And Fraud Coordination | [../features/vulnerability-patch-vendor-risk-and-fraud-coordination.md](../features/vulnerability-patch-vendor-risk-and-fraud-coordination.md) | Present |

## App Repository Current State Inventory

| Marker | Value |
| --- | --- |
| Repo exists | Yes |
| Runnable frontend: | No |
| Runnable backend: | No |
| App-specific migrations: | Yes |
| OpenAPI contract | Yes |
| Event contracts | Yes |
| Deployment skeleton | Yes |
| CI workflow | No |
| Current implementation conclusion: | Keep the zero-to-one foundation explicit until runnable frontend, backend, migrations, contracts, CI, deployment, and proof-slice evidence are all present in `ts-epdg-security-operations-compliance-regulatory`. |

## Feature/Module Cluster Analysis

| Feature | Feature ID | Source detail carried into tasks | Implementing task IDs | Phase |
| --- | --- | --- | --- | --- |
| [Business Continuity And Operational Resilience](../features/business-continuity-and-operational-resilience.md) | F-security-operations-compliance-regulatory-001 |  | DT-06-security-operations-compliance-regulatory-P02-T001, DT-06-security-operations-compliance-regulatory-P02-T002, DT-06-security-operations-compliance-regulatory-P02-T007 | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| [Compliance Control](../features/compliance-control.md) | F-security-operations-compliance-regulatory-001 |  | DT-06-security-operations-compliance-regulatory-P02-T003, DT-06-security-operations-compliance-regulatory-P02-T004, DT-06-security-operations-compliance-regulatory-P02-T007 | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| [Data Retention And Legal Hold](../features/data-retention-and-legal-hold.md) | F-security-operations-compliance-regulatory-001 |  | DT-06-security-operations-compliance-regulatory-P02-T005, DT-06-security-operations-compliance-regulatory-P02-T006, DT-06-security-operations-compliance-regulatory-P02-T007 | P02 - Business Continuity And Operational Resilience And Compliance Control And Data Retention And Legal Hold |
| [Export Control Audit Evidence And Resilience Response](../features/export-control-audit-evidence-and-resilience-response.md) | F-security-operations-compliance-regulatory-001 |  | DT-06-security-operations-compliance-regulatory-P03-T001, DT-06-security-operations-compliance-regulatory-P03-T002, DT-06-security-operations-compliance-regulatory-P03-T007 | P03 - Export Control Audit Evidence And Resilience Response And Privacy Breach Lawful Intercept And Legal Requests And Regulatory Operations |
| [Privacy Breach Lawful Intercept And Legal Requests](../features/privacy-breach-lawful-intercept-and-legal-requests.md) | F-security-operations-compliance-regulatory-001 |  | DT-06-security-operations-compliance-regulatory-P03-T003, DT-06-security-operations-compliance-regulatory-P03-T004, DT-06-security-operations-compliance-regulatory-P03-T007 | P03 - Export Control Audit Evidence And Resilience Response And Privacy Breach Lawful Intercept And Legal Requests And Regulatory Operations |
| [Regulatory Operations](../features/regulatory-operations.md) | F-security-operations-compliance-regulatory-001 |  | DT-06-security-operations-compliance-regulatory-P03-T005, DT-06-security-operations-compliance-regulatory-P03-T006, DT-06-security-operations-compliance-regulatory-P03-T007 | P03 - Export Control Audit Evidence And Resilience Response And Privacy Breach Lawful Intercept And Legal Requests And Regulatory Operations |
| [Security Monitoring And Incident](../features/security-monitoring-and-incident.md) | F-security-operations-compliance-regulatory-001 |  | DT-06-security-operations-compliance-regulatory-P04-T001, DT-06-security-operations-compliance-regulatory-P04-T002, DT-06-security-operations-compliance-regulatory-P04-T005 | P04 - Security Monitoring And Incident And Vulnerability Patch Vendor Risk And Fraud Coordination |
| [Vulnerability Patch Vendor Risk And Fraud Coordination](../features/vulnerability-patch-vendor-risk-and-fraud-coordination.md) | F-security-operations-compliance-regulatory-001 |  | DT-06-security-operations-compliance-regulatory-P04-T003, DT-06-security-operations-compliance-regulatory-P04-T004, DT-06-security-operations-compliance-regulatory-P04-T005 | P04 - Security Monitoring And Incident And Vulnerability Patch Vendor Risk And Fraud Coordination |

## Phase Decision Matrix

| Phase file | Task count | Evidence basis | Exit gate |
| --- | --- | --- | --- |
| [P01-from-scratch-app-foundation-and-delivery-runtime.md](P01-from-scratch-app-foundation-and-delivery-runtime.md) | 14 | The planning pack and local repo inspection do not prove a complete runnable implementation for `ts-epdg-security-operations-compliance-regulatory`; this from-scratch foundation phase creates the app-root runtime, governance, contracts, data, CI, deployment, observability, and proof slice before feature delivery. | A clean checkout of `ts-epdg-security-operations-compliance-regulatory` can run Angular and Spring Boot, apply `security_compliance_regulatory` migrations, validate contracts/events, run Docker Compose and Helm checks, and prove one UI/API/data/event slice. |
| [P02-business-continuity-and-operational-resilience-and-compliance-control-and-data.md](P02-business-continuity-and-operational-resilience-and-compliance-control-and-data.md) | 7 | Build the Business Continuity And Operational Resilience, Compliance Control, Data Retention And Legal Hold capability cluster for Security Operations, Compliance, And Regulatory, carrying source workflows, APIs, events, tables, controls, and tests from the feature files into implementable work. | Security Operations, Compliance, And Regulatory can execute the Business Continuity And Operational Resilience, Compliance Control, Data Retention And Legal Hold workflows through UI, API, `security_compliance_regulatory` persistence, outbox events, audit evidence, and release tests. |
| [P03-export-control-audit-evidence-and-resilience-response-and-privacy-breach-lawful.md](P03-export-control-audit-evidence-and-resilience-response-and-privacy-breach-lawful.md) | 7 | Build the Export Control Audit Evidence And Resilience Response, Privacy Breach Lawful Intercept And Legal Requests, Regulatory Operations capability cluster for Security Operations, Compliance, And Regulatory, carrying source workflows, APIs, events, tables, controls, and tests from the feature files into implementable work. | Security Operations, Compliance, And Regulatory can execute the Export Control Audit Evidence And Resilience Response, Privacy Breach Lawful Intercept And Legal Requests, Regulatory Operations workflows through UI, API, `security_compliance_regulatory` persistence, outbox events, audit evidence, and release tests. |
| [P04-security-monitoring-and-incident-and-vulnerability-patch-vendor-risk-and-fraud.md](P04-security-monitoring-and-incident-and-vulnerability-patch-vendor-risk-and-fraud.md) | 5 | Build the Security Monitoring And Incident, Vulnerability Patch Vendor Risk And Fraud Coordination capability cluster for Security Operations, Compliance, And Regulatory, carrying source workflows, APIs, events, tables, controls, and tests from the feature files into implementable work. | Security Operations, Compliance, And Regulatory can execute the Security Monitoring And Incident, Vulnerability Patch Vendor Risk And Fraud Coordination workflows through UI, API, `security_compliance_regulatory` persistence, outbox events, audit evidence, and release tests. |

## Split/Merge Decisions

- P01 remains the app-runtime foundation because the local repo inspection does not prove a complete runnable implementation for `ts-epdg-security-operations-compliance-regulatory`.
- Feature phases are grouped from source `features/*.md` files by lifecycle ownership, UI workbench/API/data/event coupling, security/privacy controls, observability, and release-test needs.
- Every feature file appears in task `Source evidence`, the tracker coverage matrix, and this discovery artifact; tracker-only feature references are not accepted as coverage.
- Generic phase names from older task packs are retired by this refresh and replaced with feature-derived phase names.

## Validator and Regeneration Notes

- Run `python3 telcosuite-skills/skills/tmf-dev-task-planner/scripts/validate_dev_tasks.py --root ts-planning/planning/suite-details/06-enterprise-platform-data-governance/security-operations-compliance-regulatory --strict` after refresh.
- Re-run the mirror driver after validation so `ts-epdg-security-operations-compliance-regulatory/dev-tasks/` remains byte-identical to the planning source.
- If a source feature changes, refresh this app pack and verify phase count, feature coverage, task detail quality, and mirror parity again.
