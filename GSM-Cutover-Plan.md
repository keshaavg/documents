# GSM Cutover Activity List – Master

## Document Intent

This document defines the minimum required activities to support GSM cutover readiness.

The purpose of this document is to:
- Act as a live cutover readiness and governance artifact
- Support dry runs of deployment, enablement, and rollback
- Provide a structured source for raising PBIs and execution tasks
- Capture all non-development activities required for cutover

This document is intentionally not a task-level runbook.  
Detailed execution steps are expected to be tracked and delivered via PBIs raised against the activities defined here.

This document is expected to evolve as cutover approaches.

---

## Change Log

| Version | Date       | Type        | Summary |
|--------|------------|-------------|---------|
| v1.0   | 2026-01-XX | Initial     | Initial cutover framework created |
| v1.1   | 2026-01-XX | Structural  | Added service enablement strategy and post-cutover hypercare |
| v1.2   | 2026-01-XX | Governance  | Added SOM handover and operational readiness |

---

## PRE Cutover Checks

### GSM-1 Access & Identity

#### GSM-1.1 Kafka Client and Identity
- Request Kafka Client ID and Identity Pool ID via DP ServiceNow
- Update application configuration with Client ID and Identity Pool ID

#### GSM-1.2 Kafka Topic Access
- Obtain access manifest for GSM managed identity for topics:
  - TS.SE.DEAL.INTERNAL-(env)
  - TS.SE.OPERATIONAL.EVENT.ETRM.EVENT-(env)
- Register consumer group ID:
  - gsm.volume.acknowledgement-prod
- Confirm producer publish permission
- Confirm consumer read and offset commit permission

#### GSM-1.3 Supporting Platform Access
- Confirm Service Discovery and API access
- Confirm Schema Registry OAuth access and Avro schema availability

---

### GSM-2 Kafka Topics and Messaging
- Confirm Kafka topics exist in target environment (PROD):
  - TS.SE.OPERATIONAL.EVENT.ETRM.EVENT-(env)
  - TS.SE.DEAL.INTERNAL-(env)
- Validate producer publish smoke test
- Validate consumer ACK consumption
- Validate offset commits
- Validate end-to-end acknowledgement path

---

### GSM-3 Application Configuration and Secrets

#### GSM-3.1 Kafka Configuration (to be populated)

**Kafka OAuth**
- kafkaOAuthAppOptions.clientId
- kafkaOAuthAppOptions.identityPoolId

**Service Discovery**
- serviceDiscoveryOptions.apiUrl

**Schema Registry**
- schemaRegistryAuthenticationOptions.oauthEnabled
- schemaRegistryAuthenticationOptions.clientId
- schemaRegistryAuthenticationOptions.identityPoolId

**OpenTelemetry**
- openTelemetryExporterOptions.apiUrl
- openTelemetryExporterOptions.endpoint
- openTelemetryExporterOptions.enabled
- openTelemetryExporterOptions.serviceEnvironment
- openTelemetryExporterOptions.applicationId
- openTelemetryExporterOptions.apiVersion
- OpenTelemetry API key (if used)

**Consumer**
- consumerOptions.topic
- consumerOptions.groupId

**Producer**
- producerOptions.topicName
- producerOptions.messageIdentifier
- producerOptions.dealId

#### GSM-3.2 Databricks Configuration (to be populated)

**Connection**
- DatabricksConn.BaseUrl
- DatabricksConn.WarehouseId
- DatabricksConn.WarehouseIdForFrequentData

**Authentication**
- DatabricksConn.ManagedIdentityClientId
- Databricks PAT token (if applicable)
- Databricks service principal secret (if applicable)

**Catalog and Schemas**
- DatabricksConn.CatalogName
- DatabricksConn.CuratedGasStorageSchema
- DatabricksConn.EnrichedGasDigitalPlatformSchema

**Runtime and Retry**
- DatabricksConn.RetryCount
- DatabricksConn.RetryStatementExecutionDelay

**Environment**
- DatabricksConn.Env

**Note on configuration sourcing**
- Configuration values that differ between environments (DEV, QAT, PROD) will be injected via zero-day deployment pipelines and infrastructure.
- Configuration values that are secrets will be stored and injected from Azure Key Vault via the deployment infrastructure pipeline.
- Configuration values that are non-secret and do not vary between environments will be populated directly in appsettings.json.

---

### GSM-4 Databricks (Views and Access)

#### GSM-4.1 Databricks Catalogs
- ts_shell_energy_eun-unitycatalog-dev
- ts_shell_energy_eun-unitycatalog-uat
- ts_shell_energy_eun-unitycatalog-prd
- ts_shell_energy_eun-unitycatalog-tst

#### GSM-4.2 TRM Base Views
- Catalog: ts_shell_energy_eun-unitycatalog-<env>
- Views:
  - enriched_deal.vw_base_deal
  - enriched_gas_physical_position.vw_base
  - enriched_gas_physical_position.vw_bucketed_base
  - enriched_etrm_reference.vw_eod_max_completed_portfolio_job
  - enriched_etrm_reference.vw_eod_undisc_max_completed_portfolio_job

**Activities**
- Confirm views exist in target environment
- Confirm data populated
- Request and validate read access for:
  - GSM API managed identity
  - GSM Azure Functions managed identity
  - GSM developers (read-only)

#### GSM-4.3 GSM Curated Views
- Schema: curated_gas_storage_management
- Views:
  - gsm_vdm_endur_intraday_daily_vols_vw
  - gsm_vdm_endur_intraday_hourly_vols_vw
  - gsm_vdm_endur_eod_daily_vols_vw
  - gsm_vdm_endur_eod_hourly_vols_vw
  - gsm_vdm_endur_unrestricted_daily_vols_vw
  - gsm_vdm_endur_unrestricted_hourly_vols_vw

**Activities**
- Confirm views exist
- Confirm data populated
- Request and validate read access for GSM managed identities and developers

#### GSM-4.4 Portfolio Strategy View
- Catalog: ts_shell_energy_eun-unitycatalog-<env>
- Schema: enriched_gas_digital_platform
- View: portfolio_strategy

**Activities**
- Confirm view exists in target environment
- Confirm latest portfolio / strategy changes available
- Request and validate read access for GSM managed identities and developers
- Confirm Portfolio job runs at (07:00 UK) and setup via library variables

---

### GSM-5 Interfaces and External Integrations
- Confirm MTS files for Endur are populated in SFTP
- Validate transfer of actual and confirmed values
- Question - SFTP details remain as for current process

---

### GSM-6 System Identities and Azure Resources
- Create GSM API system-assigned managed identity (if not existing)
- Create GSM Function App system-assigned managed identity (if not existing)
- Assign Databricks Unity Catalog access
- Validate managed identity connectivity to Databricks

---

## GSM-7 Business Readiness, Migration Setup, and Security Enablement

**Scope**  
This section covers GSM-specific setup required due to ETRM migration, including contract setup, user migration, access control, and security enablement prior to go-live. Data migration is owned by another team; GSM activities focus on configuration, controls, and readiness.

### GSM-7.1 Contract Setup in GSM (ETRM Migration Readiness)
- Identify all existing storage contracts in scope for migration
- Set up contracts in GSM, including:
  - Single-location contracts
  - Multi-location contracts
- Validate contract configuration:
  - Flowpoints, capacities, WGV, Decline Curves, Deal mappings etc
- Validate contracts align with migrated data from the new ETRM
- Confirm contract setup is complete before enabling live processing

**Dependencies**
- ETRM data migration completed by upstream team
- Contract definitions confirmed by business

### GSM-7.2 User Migration and Access Setup
- Identify existing users from the legacy application
- Create users in GSM V3
- Assign roles and permissions based on:
  - Current application access
  - GSM Read / Write access model
  - Enable ETRM uploads for eligible users
- Define and agree a mechanism for assigning access in the new application
- Validate user access

### GSM-7.3 Application Access Enablement (Shell Access Management)
- Request application onboarding into the access management tool
- Configure access policies for GSM UI
- Validate users can access GSM through the approved Shell access mechanism

### GSM-7.4 Web Application Firewall (WAF) Enablement (Pre-Go-Live)
- Confirm whether WAF is mandatory for GSM in PROD
- Request WAF enablement for GSM UI
- Validate WAF configuration does not block valid application traffic

---

## GSM-8 Deployment Governance and Controls

### GSM-8.1 Quality Gates and Approval Gates (SOM Process)
- Configure quality gates and approvals for PROD pipelines
- Validate approver groups

### GSM-8.2 Deployment Sequencing
1. Deploy Infrastructure pipeline
2. Deploy GSM API pipeline
3. Deploy GSM UI pipeline

### GSM-8.3 Deployment Validation
- Validate successful execution of each pipeline stage
- Confirm no failed or skipped stages

### GSM-8.4 Final Readiness Confirmation
- Confirm PROD pipeline configuration
- Confirm QAT deployments completed successfully
- Confirm SOM sign-off received

### GSM-8.5 Service Enablement Strategy (Pre-Cutover Constraint)
- Applications may be deployed pre-cutover
- Services must remain disabled until cutover weekend
- Enablement is a controlled cutover-only activity

### GSM-8.6 Operational Readiness and SOM Handover (Pre-Cutover)
- Engage SOM team prior to cutover
- Complete knowledge transfer and documentation handover
- Confirm SOM readiness for primary operational support post-cutover

---

## GSM-9 Connectivity and Smoke Testing (POST-DEPLOYMENT)

### GSM-9.1 UI Front-End Smoke Checks
- Validate GSM UI availability and access

### GSM-9.2 Azure Functions and WebJobs Startup Validation
- Confirm all Functions and WebJobs are running

### GSM-9.3 Kafka Producer Smoke Validation
- Validate producer startup without publishing business data

### GSM-9.4 Kafka Consumer and Acknowledgement Validation
- Validate consumer readiness and stability

---

## GSM-10 Rollback Readiness and Parallel-Run Strategy

### GSM-10.1 Parallel Deployment Model
- New GSM runs alongside existing application

### GSM-10.2 Rollback Principle
- Revert access and stop GSM services if required

### GSM-10.3 Rollback Options
- Disable access
- Stop GSM services

### GSM-10.4 Automation / Scripted Rollback
- Evaluate need for rollback automation

---

## GSM-11 Post-Cutover Hypercare and User Communication

### GSM-11.1 Post-Cutover Monitoring and Hypercare Period
- Monitor GSM during defined hypercare window
- Support SOM as required

### GSM-11.2 User Communication and Go-Live Confirmation
- Confirm successful cutover to users
- Communicate support channels
