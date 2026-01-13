# GSM Cutover – Azure DevOps Modules and PBIs

This document defines Azure DevOps Modules and Product Backlog Items (PBIs) derived from the GSM Cutover Activity List.
It aligns with existing GSM ADA module-based backlog structure.

---

## Module 16 – GSM Pre-Cutover Readiness

### [GSM][ADA] Module 16.1 – Pre-Cutover Environment and Configuration Readiness

**Description**  
Verify PROD environment readiness for GSM cutover by confirming environment variables, Azure DevOps Library variable groups, service connections, and Key Vault references are correctly set up and resolvable. Internal application configuration is validated via standard pipelines and is not itemised.

**Acceptance Criteria**
- Required environment variables for GSM are present in PROD
- Azure DevOps Library variable groups are correctly linked
- Key Vault references resolve successfully
- Pipeline service connections and permissions authorised
- Evidence captured for readiness confirmation

**Tasks**
- Verify environment variables for GSM components
- Verify Library variable group linkage for PROD pipelines
- Validate Key Vault references resolve successfully
- Validate pipeline permissions and service connections
- Capture readiness evidence
- Raise and track ServiceNow requests for gaps

---

### [GSM][ADA] Module 16.2 – Kafka Client Identity, Topic Access, and Permissions Readiness

**Description**  
Ensure Kafka identity and permissions required by GSM are established in PROD. No business messages are published during verification.

**Acceptance Criteria**
- Kafka Client ID and Identity Pool ID available
- Access granted to required Kafka topics
- Consumer group `gsm.volume.acknowledgement-prod` registered
- Producer and consumer permissions confirmed

**Tasks**
- Raise DP ServiceNow request for Kafka Client ID and Identity Pool ID
- Obtain access manifest for GSM managed identity
- Confirm producer publish permission
- Confirm consumer read and offset commit permission
- Validate consumer group registration
- Capture evidence

---

### [GSM][ADA] Module 16.3 – Databricks SQL Warehouse and Managed Identity Access Readiness

**Description**  
Create GSM-owned Databricks SQL Warehouses and ensure GSM managed identity is created and granted Unity Catalog access to query TRM / PRM views in PROD.

**Acceptance Criteria**
- GSM managed identity exists in PROD
- Unity Catalog access granted for required schemas and views
- Databricks SQL Warehouses created (standard and frequent)
- Warehouse IDs captured for configuration
- Managed identity can query required base views

**Tasks**
- Raise ServiceNow request to create/confirm GSM managed identity
- Raise ServiceNow request to grant Unity Catalog access
- Track ServiceNow approvals
- Create Databricks SQL Warehouse for standard workloads
- Create Databricks SQL Warehouse for frequent/intraday workloads
- Capture Warehouse IDs
- Validate managed identity connectivity
- Execute validation queries and capture evidence

---

### [GSM][ADA] Module 16.4 – TRM / PRM Base Views and Data Availability Confirmation

**Description**  
Confirm required TRM / PRM base views exist and are populated in PROD.

**Acceptance Criteria**
- Required base views exist and are queryable
- Data populated by PRM processes
- Read access validated for GSM identities
- Evidence captured

**Tasks**
- Coordinate with TRM / PRM team
- Verify base view existence
- Validate sample data presence
- Validate read access
- Capture evidence

---

### [GSM][ADA] Module 16.5 – GSM Curated View Readiness

**Description**  
Ensure GSM curated views exist and are populated based on PRM data.

**Acceptance Criteria**
- Curated schema exists
- All GSM curated views populated
- Read access validated

**Tasks**
- Verify curated schema
- Verify curated views
- Validate data population
- Capture evidence

---

### [GSM][ADA] Module 16.6 – Portfolio Strategy View Readiness

**Description**  
Confirm portfolio strategy view availability, data freshness, and access.

**Acceptance Criteria**
- portfolio_strategy view exists in PROD
- Latest strategy data available
- Job schedule confirmed (07:00 UK)
- Read access validated

**Tasks**
- Verify view existence
- Validate data freshness
- Confirm job scheduling and variables
- Capture evidence

---

### [GSM][ADA] Module 16.7 – SAP MTS Interface Readiness

**Description**  
Confirm SAP MTS files required for GSM are available via SFTP.

**Acceptance Criteria**
- SFTP path exists
- Expected files present
- File naming and structure confirmed

**Tasks**
- Confirm SFTP details
- Verify file availability
- Validate naming and structure
- Capture evidence

---

### [GSM][ADA] Module 16.8 – Deployment Governance and SOM Approval Gates

**Description**  
Ensure quality gates and approval gates are configured for PROD deployments.

**Acceptance Criteria**
- Quality gates configured
- Approval gates configured for Infra, API, UI
- Approver groups validated
- SOM sign-off obtained

**Tasks**
- Coordinate with SOM
- Configure quality and approval gates
- Validate approvals
- Capture evidence

---

### [GSM][ADA] Module 16.9 – Pre-Cutover Deployment (Services Disabled)

**Description**  
Deploy GSM components to PROD without enabling services.

**Acceptance Criteria**
- Infrastructure, API, UI deployed successfully
- Functions and WebJobs deployed
- Services remain disabled

**Tasks**
- Deploy Infrastructure pipeline
- Deploy API pipeline
- Deploy UI pipeline
- Validate deployments
- Confirm services remain disabled

---

### [GSM][ADA] Module 16.10 – Contract Setup in GSM

**Description**  
Create and configure GSM contracts aligned with ETRM migration.

**Acceptance Criteria**
- All in-scope contracts created
- Contract configuration validated
- Alignment with migrated ETRM data confirmed

**Tasks**
- Identify contracts in scope
- Create contracts in GSM
- Configure flowpoints, capacities, WGV, decline curves, deal mappings
- Validate configuration
- Capture sign-off

---

### [GSM][ADA] Module 16.11 – User Migration and Role / Permission Setup

**Description**  
Migrate users and assign roles and permissions in GSM.

**Acceptance Criteria**
- Users created in GSM
- Roles and permissions assigned
- Access enforcement validated

**Tasks**
- Identify legacy users
- Create users in GSM
- Assign roles and permissions
- Validate access behaviour
- Document access process

---

### [GSM][ADA] Module 16.12 – Application Access Enablement (Shell Access Management)

**Description**  
Enable GSM access via Shell access management tooling.

**Acceptance Criteria**
- GSM onboarded into access management
- Access policies configured
- User access validated

**Tasks**
- Raise onboarding request
- Configure access policies
- Validate authorised and unauthorised access
- Capture evidence

---

### [GSM][ADA] Module 16.13 – WAF Enablement (Pre-Go-Live)

**Description**  
Confirm and enable WAF for GSM UI if required.

**Acceptance Criteria**
- WAF requirement confirmed
- WAF enabled if mandatory
- No valid traffic blocked

**Tasks**
- Confirm requirement
- Request WAF enablement
- Validate traffic and logs

---

### [GSM][ADA] Module 16.14 – Operational Readiness and SOM Handover

**Description**  
Complete operational handover to SOM prior to cutover.

**Acceptance Criteria**
- Knowledge transfer completed
- Runbooks shared
- Escalation paths agreed
- SOM readiness confirmed

**Tasks**
- Deliver KT sessions
- Share runbooks
- Agree escalation paths
- Capture readiness confirmation

---

## Module 17 – GSM Cutover Execution

### [GSM][ADA] Module 17.1 – Service Enablement During Cutover Window

**Description**  
Enable GSM services during the agreed cutover window.

**Acceptance Criteria**
- Functions, WebJobs, Kafka enabled
- Enablement performed only during cutover

**Tasks**
- Enable Functions
- Enable WebJobs
- Enable Kafka processing
- Capture evidence

---

### [GSM][ADA] Module 17.2 – Post-Enablement Smoke Testing

**Description**  
Validate GSM stability post-enablement.

**Acceptance Criteria**
- UI available
- Services running without critical errors
- Kafka stable

**Tasks**
- UI smoke tests
- Validate Functions and WebJobs
- Validate Kafka producer and consumer
- Review logs

---

### [GSM][ADA] Module 17.3 – Cutover Go / No-Go Confirmation

**Description**  
Confirm cutover outcome and rollback decision.

**Acceptance Criteria**
- End-to-end validation complete
- Go / No-Go decision recorded

**Tasks**
- Review validation results
- Record decision
- Execute rollback if required

---

## Module 18 – GSM Post-Cutover Hypercare

### [GSM][ADA] Module 18.1 – Hypercare and User Communication

**Description**  
Provide hypercare support and communicate GSM go-live.

**Acceptance Criteria**
- Hypercare active
- Users informed
- SOM providing operational support

**Tasks**
- Monitor services
- Support SOM
- Send go-live communication
