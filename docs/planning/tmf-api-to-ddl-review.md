# Security Operations, Compliance, And Regulatory TMF API To DDL Review

Reviewed: 2026-06-14

Status: Complete for baseline app implementation. Endpoint-specific contract tests and final story-level field promotion still happen during build.

## Scope

This review covers `security_compliance_regulatory` in suite database `ts_enterprise_platform_governance`. It uses the local TMF Open API reference set, the suite data model, the API-to-DDL traceability matrix, and the V001 starter DDL.

The review confirms that the app can move into implementation with a V002 typed DDL baseline while preserving full TMF payload compatibility through validated `tmf_payload`, typed common TMF columns, and normalized support tables.

## TMF API Baseline Selection

| TMF API | Local baseline spec | Resources/path roots reviewed | V001 table groups |
| --- | --- | --- | --- |
| TMF720 | `references/tmforum-open-apis/openapi-specs/TMF720_DigitalIdentity/TMF720_Digital_Identity_Management_API_v4.0.0_swagger.json` | `digitalIdentity` | platform_user; customer_identity_link; developer_organization; developer_application |
| TMF672 | `references/tmforum-open-apis/openapi-specs/TMF672_UserRolesPermissions/TMF672-User_Role_Permission_Management_API-v5.1.0.oas.yaml` | `checkPermission`, `permissionSet`, `permissionSpecification`, `permissionSpecificationSet` | tenant; environment; platform_user; platform_role; platform_permission; developer_application references |
| TMF696 | `references/tmforum-open-apis/openapi-specs/TMF696_RiskManagement/TMF696_Risk_Management_API_v4.0.0_swagger.json` | `partyRoleProductOfferingRiskAssessment`, `partyRoleRiskAssessment`, `productOfferingRiskAssessment`, `productOrderRiskAssessment`, `shoppingCartRiskAssessment` | rule_definition; decision_definition; credit_decision; risk_exposure; compliance_control |
| TMF621 | `references/tmforum-open-apis/openapi-specs/TMF621_TroubleTicket/TMF621-Trouble_Ticket-v5.0.1.oas.yaml` | `troubleTicket`, `troubleTicketSpecification` | trouble_ticket; service_problem; operational_incident; care_case references in BSS |
| TMF644 | `references/tmforum-open-apis/openapi-specs/TMF644_Privacy/TMF644-Privacy-v5.0.0.oas.yaml` | `partyPrivacyAgreement`, `partyPrivacyProfile`, `partyPrivacyProfileSpecification` | consent; privacy_preference; retention_policy references in platform |
| TMF667 | `references/tmforum-open-apis/openapi-specs/TMF667_Document/TMF667_Document_Management_API_v4.0.0_swagger.json` | `document`, `documentSpecification` | customer_document_metadata; control_evidence; audit_evidence_package |
| TMF707 | `references/tmforum-open-apis/openapi-specs/TMF707_TestResult/TMF707_Test_Result_Management_API_v4.0.0_swagger.json` | `nonFunctionalTestResult`, `testCaseResult`, `testSuiteResult` | test_result; control_evidence references |
| TMF710 | `references/tmforum-open-apis/openapi-specs/TMF710_GeneralTestArtifact/TMF710_General_Test_Artifact_Management_API_v4.0.0_swagger.json` | `generalTestArtifact` | api_contract; api_contract_version; test_artifact; conformance_evidence |
| TMF681 | `references/tmforum-open-apis/openapi-specs/TMF681_Communication/TMF681_Communication_Management_API_v4.0.0_swagger.json` | `communicationMessage` | communication_record; notification_template; notification_delivery_attempt references |
| TMF735 | `references/tmforum-open-apis/openapi-specs/TMF735_CDRTransactionManagement/TMF735-CDR_Transaction_Management-v5.0.0.oas.yaml` | `cdrTransaction` | cdr_transaction; usage_ingestion_batch; revenue_assurance_case |
| TMF724 | `references/tmforum-open-apis/openapi-specs/TMF724_IncidentManagement/TMF724_Incident_Management_API_v4.0.0_swagger.json` | `diagnoseIncident`, `incident`, `resolveIncident` | operational_incident; operational_command_center_session references |
| TMF655 | `references/tmforum-open-apis/openapi-specs/TMF655_ChangeManagement/TMF655_Change_Management_API_v4.0.0_swagger.json` | `changeRequest` | change_record; maintenance_window; migration_decommissioning_state references |

## Current DDL Coverage

Current starter DDL is in `database/postgres/suites/ts_enterprise_platform_governance/V001__create_app_schemas_and_starter_tables.sql` under schema `security_compliance_regulatory`.

| Current table | TMF purpose | V002 decision |
| --- | --- | --- |
| `security_compliance_regulatory.security_incident` | Starter table for Security Operations, Compliance, And Regulatory; V002 promotes common TMF fields and keeps full validated payload support. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.compliance_control` | Starter table for Security Operations, Compliance, And Regulatory; V002 promotes common TMF fields and keeps full validated payload support. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.control_evidence` | Starter table for Security Operations, Compliance, And Regulatory; V002 promotes common TMF fields and keeps full validated payload support. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.regulatory_obligation` | Starter table for Security Operations, Compliance, And Regulatory; V002 promotes common TMF fields and keeps full validated payload support. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.regulatory_submission` | Starter table for Security Operations, Compliance, And Regulatory; V002 promotes common TMF fields and keeps full validated payload support. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.retention_policy` | Governance/control table for app data handling. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.legal_hold` | Starter table for Security Operations, Compliance, And Regulatory; V002 promotes common TMF fields and keeps full validated payload support. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.audit_evidence_package` | Starter table for Security Operations, Compliance, And Regulatory; V002 promotes common TMF fields and keeps full validated payload support. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.business_continuity_plan` | Starter table for Security Operations, Compliance, And Regulatory; V002 promotes common TMF fields and keeps full validated payload support. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| `security_compliance_regulatory.event_outbox` | App outbox for domain and TMF notification events. | Keep and refine through `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |

## Resource To Table Decisions

| TMF API/resource | Master or anchor table | Path coverage | Promoted field candidates | Field handling strategy |
| --- | --- | --- | --- | --- |
| TMF720 `digitalIdentity` | `security_compliance_regulatory.security_incident` | `/digitalIdentity`, `/digitalIdentity/{id}` | `id`, `href`, `creationDate`, `lastUpdate`, `nickname`, `status`, `attachment`, `contactMedium` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF672 `checkPermission` | `security_compliance_regulatory.security_incident` | `/checkPermission`, `/checkPermission/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF672 `permissionSet` | `security_compliance_regulatory.security_incident` | `/permissionSet`, `/permissionSet/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF672 `permissionSpecification` | `security_compliance_regulatory.security_incident` | `/permissionSpecification`, `/permissionSpecification/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF672 `permissionSpecificationSet` | `security_compliance_regulatory.security_incident` | `/permissionSpecificationSet`, `/permissionSpecificationSet/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF696 `partyRoleProductOfferingRiskAssessment` | `security_compliance_regulatory.compliance_control` | `/partyRoleProductOfferingRiskAssessment`, `/partyRoleProductOfferingRiskAssessment/{id}` | `id`, `href`, `status`, `characteristic`, `partyRole`, `place`, `productOffering`, `riskAssessmentResult` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF696 `partyRoleRiskAssessment` | `security_compliance_regulatory.compliance_control` | `/partyRoleRiskAssessment`, `/partyRoleRiskAssessment/{id}` | `id`, `href`, `status`, `characteristic`, `place`, `riskAssessmentResult`, `@baseType`, `@schemaLocation` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF696 `productOfferingRiskAssessment` | `security_compliance_regulatory.compliance_control` | `/productOfferingRiskAssessment`, `/productOfferingRiskAssessment/{id}` | `id`, `href`, `status`, `characteristic`, `partyRole`, `place`, `productOffering`, `riskAssessmentResult` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF696 `productOrderRiskAssessment` | `security_compliance_regulatory.compliance_control` | `/productOrderRiskAssessment`, `/productOrderRiskAssessment/{id}` | `id`, `href`, `status`, `characteristic`, `place`, `productOrder`, `riskAssessmentResult`, `@baseType` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF696 `shoppingCartRiskAssessment` | `security_compliance_regulatory.compliance_control` | `/shoppingCartRiskAssessment`, `/shoppingCartRiskAssessment/{id}` | `id`, `href`, `status`, `characteristic`, `place`, `riskAssessmentResult`, `shoppingCart`, `@baseType` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF621 `troubleTicket` | `security_compliance_regulatory.security_incident` | `/troubleTicket`, `/troubleTicket/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF621 `troubleTicketSpecification` | `security_compliance_regulatory.security_incident` | `/troubleTicketSpecification`, `/troubleTicketSpecification/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF644 `partyPrivacyAgreement` | `security_compliance_regulatory.retention_policy` | `/partyPrivacyAgreement`, `/partyPrivacyAgreement/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF644 `partyPrivacyProfile` | `security_compliance_regulatory.retention_policy` | `/partyPrivacyProfile`, `/partyPrivacyProfile/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF644 `partyPrivacyProfileSpecification` | `security_compliance_regulatory.retention_policy` | `/partyPrivacyProfileSpecification`, `/partyPrivacyProfileSpecification/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF667 `document` | `security_compliance_regulatory.control_evidence` | `/document`, `/document/{id}` | `id`, `href`, `creationDate`, `description`, `documentType`, `lastUpdate`, `name`, `version` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF667 `documentSpecification` | `security_compliance_regulatory.control_evidence` | `/documentSpecification`, `/documentSpecification/{id}` | `id`, `href`, `description`, `isBundle`, `lastUpdate`, `name`, `version`, `attachment` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF707 `nonFunctionalTestResult` | `security_compliance_regulatory.control_evidence` | `/nonFunctionalTestResult`, `/nonFunctionalTestResult/{id}` | `id`, `href`, `nonFunctionalTestResultDefinition`, `testExecution`, `@baseType`, `@schemaLocation`, `@type` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF707 `testCaseResult` | `security_compliance_regulatory.control_evidence` | `/testCaseResult`, `/testCaseResult/{id}` | `id`, `href`, `testCaseResultDefinition`, `testExecution`, `@baseType`, `@schemaLocation`, `@type` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF707 `testSuiteResult` | `security_compliance_regulatory.control_evidence` | `/testSuiteResult`, `/testSuiteResult/{id}` | `id`, `href`, `testExecution`, `testSuiteResultDefinition`, `@baseType`, `@schemaLocation`, `@type` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF710 `generalTestArtifact` | `security_compliance_regulatory.security_incident` | `/generalTestArtifact`, `/generalTestArtifact/{id}` | `id`, `href`, `description`, `version`, `versionDescription`, `agreement`, `attribute`, `generalArtifactDefinition` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF681 `communicationMessage` | `security_compliance_regulatory.security_incident` | `/communicationMessage`, `/communicationMessage/{id}` | `id`, `href`, `content`, `description`, `logFlag`, `messageType`, `priority`, `scheduledSendTime` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF735 `cdrTransaction` | `security_compliance_regulatory.security_incident` | `/cdrTransaction`, `/cdrTransaction/{id}` | Common TMF metadata plus payload validation | Promote common TMF metadata; store resource-specific fields in tmf_payload until query patterns justify additional typed columns. |
| TMF724 `diagnoseIncident` | `security_compliance_regulatory.security_incident` | `/diagnoseIncident`, `/diagnoseIncident/{id}` | `id`, `href`, `ackTime`, `category`, `clearTime`, `domain`, `incidentDetail`, `incidentResolutionSuggestion` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF724 `incident` | `security_compliance_regulatory.security_incident` | `/incident`, `/incident/{id}` | `id`, `href`, `ackTime`, `category`, `clearTime`, `domain`, `incidentDetail`, `incidentResolutionSuggestion` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF724 `resolveIncident` | `security_compliance_regulatory.security_incident` | `/resolveIncident`, `/resolveIncident/{id}` | `id`, `href`, `ackTime`, `category`, `clearTime`, `domain`, `incidentDetail`, `incidentResolutionSuggestion` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |
| TMF655 `changeRequest` | `security_compliance_regulatory.security_incident` | `/changeRequest`, `/changeRequest/{id}` | `id`, `href`, `actualEndTime`, `actualStartTime`, `channel`, `completionDate`, `description`, `impact` | Promote common TMF lifecycle/reference fields; store remaining validated resource fields in tmf_payload and characteristics tables. |

## V002 DDL Refinement

Migration: `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql`

The migration adds this implementation baseline for the app:

| Area | Decision |
| --- | --- |
| Common TMF fields | Add reusable typed columns such as `tmf_id`, `tmf_href`, `tmf_type`, `tmf_base_type`, `tmf_schema_location`, `tmf_referred_type`, `tmf_name`, `tmf_description`, `tmf_lifecycle_status`, `tmf_state`, dates, priority, and external ID to every V001 app table. |
| Full TMF compatibility | Keep the V001 `tmf_payload` column as the complete validated TMF resource snapshot for fields that are not yet promoted to typed columns. |
| Characteristics and references | Add normalized `tmf_characteristic`, `tmf_resource_reference`, `tmf_external_identifier`, `tmf_related_party`, `tmf_note`, `tmf_attachment`, and `tmf_relationship` support tables. |
| API/resource map | Add `tmf_api_resource_map` rows for the selected local TMF APIs and resource roots. |
| Event contracts | Add baseline event contract rows for create, update, state-change, and delete events per reviewed API resource. |
| Privacy and audit | Add table-level privacy, retention, legal-hold, residency, masking, and audit policy rows. |
| High-volume candidates | `security_compliance_regulatory.event_outbox` |

## Event Contract Baseline

Events are registered in `security_compliance_regulatory.event_contract` using `security_compliance_regulatory.event_outbox` as the publication basis. Consumers must be added when integrations are designed; no app should directly write another app schema.

## Privacy, Retention, And Audit Baseline

| Table | Data classification | Retention class | Audit level |
| --- | --- | --- | --- |
| `security_compliance_regulatory.security_incident` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.compliance_control` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.control_evidence` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.regulatory_obligation` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.regulatory_submission` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.retention_policy` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.legal_hold` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.audit_evidence_package` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.business_continuity_plan` | restricted | regulated_lifecycle | high |
| `security_compliance_regulatory.event_outbox` | restricted | regulated_lifecycle | high |

## Build Gate Result

| Gate item | Result |
| --- | --- |
| API/resource review | Complete for baseline implementation |
| V002 typed DDL | Complete: `database/postgres/suites/ts_enterprise_platform_governance/V004__refine_security_compliance_regulatory_tmf_core.sql` |
| Event contract register | Baseline complete |
| Privacy/retention/audit classification | Baseline complete |
| Remaining implementation control | Validate exact endpoint operations and contract tests as Angular/Spring Boot features are built |
