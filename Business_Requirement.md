# Business Requirement

## Project Title
**Fidelity FFIO Pricing Platform Outbound Feed Modernization**

---

# Business Overview

Fidelity's FFIO business unit is modernizing its Pricing Platform by transitioning from a legacy technology stack to a cloud-based architecture. As part of this modernization initiative, EY will support the migration of **41 outbound pricing data feeds** from the legacy platform to the target-state environment.

The primary objective of this initiative is to ensure all outbound feeds continue to provide accurate, timely, and reliable pricing data while maintaining uninterrupted business operations. The migration must preserve existing file formats, delivery schedules, transmission mechanisms, and downstream consumer integrations, enabling a seamless and zero-disruption transition from the legacy platform to the modernized cloud platform.

The target-state architecture leverages centralized data management, API-driven integrations, Kafka-based event streaming, and existing feed generation and scheduling frameworks. The modernization effort is aligned with Fidelity's broader Pricing Platform transformation roadmap, with cloud platform completion targeted for Q1 2027.
---

# Business Goal

Successfully migrate all 41 in-scope outbound pricing data feeds from the legacy pricing platform to the modernized cloud-based platform while ensuring:

- No disruption to downstream business processes.
- No changes to existing feed contracts.
- Preservation of file layouts and delivery schedules.
- Preservation of existing downstream endpoints.
- Accurate and reliable delivery of pricing data.
- Validation of target-state outputs against legacy outputs.
- Adoption of Fidelity-approved integration and modernization standards.

---

# Business Objectives

1. Analyze and understand all 41 outbound pricing data feeds currently operating on the legacy platform.
2. Perform detailed data mapping and gap analysis between legacy and target-state platforms.
3. Create an approved migration approach and implementation strategy for all in-scope feeds.
4. Utilize Fidelity-approved APIs and Kafka streams for data ingestion, processing, and transmission.
5. Load required and missing attributes into centralized target-state databases.
6. Reuse existing feed generation, scheduling, and transmission frameworks wherever possible.
7. Maintain backward compatibility throughout the migration process.
8. Implement required transformation and calculation logic for applicable feeds.
9. Validate migrated feeds through automated legacy-versus-target comparison processes.
10. Support Fidelity-led Regression Testing, User Acceptance Testing (UAT), and Parallel Runs.
11. Obtain final business validation and approval prior to production adoption.

---

# Scope of Work

## 1. Requirements Analysis and Feed Discovery

Perform a comprehensive review of all 41 in-scope outbound pricing feeds to establish a complete understanding of the current-state environment.

### Activities

- Analyze feed layouts and structures.
- Review feed schedules and transmission patterns.
- Identify downstream consumers and integrations.
- Understand current business processing logic.
- Review existing transformation and calculation requirements.
- Document current-state feed behavior and dependencies.

### Deliverables

- Feed Analysis Documentation
- Current-State Feed Inventory
- Requirements Documentation

---

## 2. Data Mapping and Gap Analysis

Perform detailed field-level mapping between source systems and target-state databases.

### Activities

- Identify required source attributes.
- Map legacy data elements to target-state structures.
- Identify missing attributes and data gaps.
- Document transformation requirements.
- Review mappings with Fidelity SMEs.
- Produce approved source-to-target mapping documentation.

### Deliverables

- Gap Analysis Document
- Source-to-Target Mapping Documentation
- Approved Client Specifications

---

## 3. Migration Planning and Solution Design

Define the migration strategy and implementation approach for all 41 outbound feeds.

### Activities

- Define data acquisition strategy.
- Identify API integration requirements.
- Identify Kafka integration requirements.
- Define feed migration sequencing.
- Define scheduling and transmission reuse approach.
- Document risks, dependencies, and mitigation plans.
- Produce project delivery roadmap.

### Deliverables

- Migration Approach Plan
- Project Plan
- Implementation Roadmap

---

## 4. Data Integration and Platform Onboarding

Load required data into the target-state platform using Fidelity-approved integration mechanisms.

### Activities

- Ingest data using APIs.
- Ingest data using Kafka event streams.
- Populate centralized target-state databases.
- Enrich data where required.
- Support data movement into Centralized Data Platform (CDP).
- Ensure data availability for downstream feed generation processes.

### Deliverables

- API Integration Components
- Kafka Integration Components
- CDP Data Population Processes

---

## 5. Feed Migration and Development

Implement the migrated outbound feeds on the modernized platform.

### Activities

For each of the 41 outbound feeds:

- Recreate feed generation capability on the target platform.
- Preserve existing feed structures and formats.
- Preserve downstream integration requirements.
- Reuse approved feed generation frameworks.
- Reuse approved scheduling and transmission frameworks.

For applicable feeds:

- Implement transformation logic.
- Implement calculation logic.
- Apply feed-specific processing rules.

### Deliverables

- Migrated Outbound Feeds
- Transformation Components
- Feed Generation Components

---

## 6. Feed Generation and Distribution

Generate and distribute outbound pricing feeds from the modernized platform.

### Activities

- Generate outbound feed files.
- Schedule feed execution.
- Transmit feeds using approved mechanisms.
- Ensure compatibility with downstream systems.
- Support existing delivery patterns and timing requirements.

### Deliverables

- Production-Ready Feed Generation Processes
- Scheduling Configurations
- Transmission Configurations

---

## 7. Validation and Reconciliation

Validate migrated feeds against legacy platform outputs.

### Activities

- Execute automated file-to-file comparisons.
- Compare legacy and target-state outputs.
- Document variances and discrepancies.
- Perform reconciliation analysis.
- Identify and resolve defects.
- Produce validation reports.

### Deliverables

- File Comparison Reports
- Validation Reports
- Defect Logs
- Reconciliation Reports

---

## 8. Testing Support

Support Fidelity's testing and production readiness activities.

### Activities

- Support unit testing of migrated feeds.
- Support Regression Testing.
- Support User Acceptance Testing (UAT).
- Support Production Parallel Runs.
- Resolve identified issues and defects.
- Validate conformance to Approved Client Specifications.

### Deliverables

- Unit Test Results
- Testing Support Documentation
- Defect Resolution Reports

---

## 9. Governance and Project Management

Provide oversight and reporting throughout project execution.

### Activities

- Weekly status reporting.
- Risk and issue management.
- Mitigation tracking.
- Resource onboarding updates.
- Stakeholder communication.
- Delivery progress tracking.

### Deliverables

- Weekly Status Reports
- Risk and Mitigation Reports
- Project Tracking Reports

---

## 10. Final Validation and Business Sign-Off

Obtain formal approval confirming migration readiness and business acceptance.

### Activities

- Complete final validation activities.
- Review migration outcomes.
- Validate compliance with Approved Client Specifications.
- Obtain business approval from Fidelity stakeholders.
- Prepare final migration acceptance documentation.

### Deliverables

- Final Validation Report
- Client Sign-Off Report

---

# Key Business Requirements

| Requirement ID | Requirement |
|---------------|-------------|
| BR-01 | Migrate all 41 outbound pricing data feeds to the modernized cloud platform |
| BR-02 | Preserve existing file formats |
| BR-03 | Preserve existing delivery schedules |
| BR-04 | Preserve downstream system endpoints |
| BR-05 | Perform source-to-target data mapping for all feeds |
| BR-06 | Identify and remediate data gaps |
| BR-07 | Utilize APIs and Kafka for data ingestion and transmission |
| BR-08 | Load required attributes into centralized target-state databases |
| BR-09 | Apply transformation and calculation logic for applicable feeds |
| BR-10 | Generate feeds using approved feed generation framework |
| BR-11 | Validate target-state outputs against legacy outputs |
| BR-12 | Produce comparison and reconciliation reports |
| BR-13 | Support Regression Testing, UAT, and Parallel Runs |
| BR-14 | Resolve identified validation defects |
| BR-15 | Obtain Fidelity business approval and final sign-off |

---

# Success Criteria

The project will be considered successful when:

- All 41 outbound feeds are migrated successfully.
- Existing feed contracts remain unchanged.
- Existing file formats are preserved.
- Existing delivery schedules are preserved.
- Existing downstream endpoints are preserved.
- Required data is available within the target-state platform.
- Applicable transformation logic is implemented successfully.
- File comparison results meet approved acceptance thresholds.
- All critical defects are resolved.
- Fidelity completes Regression Testing, UAT, and Parallel Runs successfully.
- Final business validation and sign-off are obtained.
- Downstream consumers experience no disruption during or after migration.

---

# Expected Business Outcome

By completing this initiative, Fidelity will successfully modernize its outbound pricing feed ecosystem, reduce dependency on legacy technology, leverage modern cloud-based integration patterns, improve data governance and maintain uninterrupted delivery of critical pricing data to downstream consumers while supporting the broader FFIO Pricing Platform modernization strategy.