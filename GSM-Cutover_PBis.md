# GSM Cutover – Azure DevOps PBIs

This document defines the Azure DevOps PBIs and tasks derived from the GSM Cutover Activity List.
It is intended to support cutover readiness, execution, and post-cutover stabilisation.

---

## Feature 1: GSM Cutover Readiness

### PBI 1.1 – Pre-Cutover Environment and Configuration Readiness

**Description**  
Verify PROD environment readiness for GSM cutover by confirming environment variables, Azure DevOps Library variable groups, and Key Vault references are correctly set up and resolvable. Internal application configuration is validated via standard pipelines and is not itemised.

**Acceptance Criteria**
- Required environment variables for GSM are present in PROD
- Azure DevOps Library variable groups are correctly linked
- Key Vault references resolve successfully
- No unresolved or missing configuration detected

**Tasks**
- Verify environment variables for GSM components
- Verify Library variable group linkage for PROD pipelines
- Validate Key Vault references resolve successfully
- Validate pipeline permissions and service connections
- Capture readiness evidence
- Raise and track ServiceNow requests for any gaps

---

### PBI 1.2 – Kafka Client Identity, Topic Access, and Permissions Readiness

**Description**  
Ensure Kafka identity and permissions required by GSM are established in PROD. No business messages are published during verification.

**Acceptance Criteria**
- Kafka Client ID and Identity Pool ID available
- Access granted to topics:
  - TS.SE.DEAL.INTERNAL-(env)
  - TS.SE.OPERATIONAL.EVENT.ETRM.EVENT-(env)
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

### PBI 1.3 – Databricks SQL Warehouse and Managed Identity Access Readiness

**Description**  
Prepare GSM-owned Databricks SQL Warehouses and ensure GSM managed identity is created and granted required Databricks Unity Catalog access.

**Acceptance Criteria**
- GSM managed identity exists in PROD
- Unity Catalog access granted for required schemas and views
- Databricks SQL Warehouses (standard and frequent/intraday) exist
- Warehouse IDs captured:
  - DatabricksConn.WarehouseId
  - DatabricksConn.WarehouseIdForFrequentData
- Managed identity can query required base views

**Tasks**
- Raise ServiceNow request to create/confirm GSM managed identity
- Raise ServiceNow request to grant Unity Catalog access
- Track ServiceNow approvals and completion
- Create Databricks SQL Warehouse for standard workloads
- Create Databricks SQL Warehouse for frequent/intraday workloads
- Capture Warehouse IDs
- Validate managed identity connectivity
- Execute validation queries and capture evidence

---

### PBI 1.4 – TRM / PRM Base Views and Data Availability Confirmation

**Description**  
Confirm required TRM / PRM base views exist and data is populated in PROD.

**Acceptance Criteria**
- Required base views exist and are populated
- GSM managed identity has read access
- Data availability confirmed

**Tasks**
- Coordinate with TRM / PRM team
- Verify base view existence
- Validate sample data presence
- Validate read access for GSM identities
- Capture evidence

---

### PBI 1.5 – GSM Curated View Readiness

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

### PBI 1.6 – Portfolio Strategy View Readiness

**Description**  
Confirm portfolio strategy view availability and access.

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

### PBI 1.7 – SAP MTS Interface Readiness

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

### PBI 1.8 – Deployment Governance and SOM Approval Gates

**Description**  
Ensure quality gates and approval gates are configured for PROD deployments.

**Acceptance Criteria**
- Quality gates configured
- Approval gates configured for Infra, API, and UI pipelines
- Approver groups validated
- SOM sign-off obtained

**Tasks**
- Coordinate with SOM
- Configure quality and approval gates
- Validate approvals
- Capture evidence

---

### PBI 1.9 – Pre-Cutover Deployment (Services Disabled)

**Description**  
Deploy GSM components to PROD without enabling services.

**Acceptance Criteria**
- Infrastructure, API, and UI deployed successfully
- Functions and WebJobs deployed
- Services remain disabled

**Tasks**
- Deploy Infrastructure pipeline
- Deploy API pipeline
- Deploy UI pipeline
- Validate deployments
- Confirm services remain disabled

---

### PBI 1.10 – Contract Setup in GSM

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

### PBI 1.11 – User Migration and Role/Permission Setup

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

### PBI 1.12 – Application Access Enablement (Shell Access Management)

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

### PBI 1.13 – WAF Enablement (Pre-Go-Live)

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

## Feature 2: GSM Cutover Execution

### PBI 2.1 – Service Enablement During Cutover

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

### PBI 2.2 – Post-Enablement Smoke Testing

**Description**  
Validate GSM stability post-e
