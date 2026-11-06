# Customer Discovery Intelligence System (CDIS)
## Use Case Specification

**Document:** `use_cases.md`  
**Project:** Customer Discovery Intelligence System (CDIS)  
**Version:** 1.0  
**Date:** November 2026  
**Status:** Initial specification  
**Related Document:** `requirements.md`

---

# 1. Introduction

## 1.1 Purpose

This document defines the primary use cases of the Customer Discovery Intelligence System (CDIS).

It describes how users interact with the system to create research projects, upload customer interviews, process research documents, extract customer intelligence, review findings and generate research reports.

The document provides a behavioural specification that will guide application development, API design, workflow implementation and testing.

## 1.2 Scope

The use cases cover three product maturity levels:

- MVP: Basic document ingestion and AI-powered insight extraction.
- Advanced: Persistent research management, contextual retrieval and cross-interview analysis.
- Production: Human approval, background workflows, monitoring and multi-user functionality.

Each use case includes its purpose, actors, preconditions, main flow, alternative flows and expected outcome where applicable.

---

# 2. Actors

An actor is a person, external system or service that interacts with CDIS.

## 2.1 Primary Actors

| Actor | Description | Main responsibilities |
|---|---|---|
| Researcher | Primary application user | Creates research projects, uploads documents and analyses findings |
| Reviewer | Person responsible for validating research outputs | Reviews, edits, approves or rejects findings |
| Administrator | Application administrator | Manages system configuration and access |
| Product Manager | Consumer of customer intelligence | Uses research findings to inform product decisions |

A single person may perform multiple roles.

## 2.2 Supporting Actors and External Systems

| Actor/System | Responsibility |
|---|---|
| Streamlit | Provides the application interface |
| FastAPI | Exposes application functionality through HTTP APIs |
| Ollama | Provides local LLM inference |
| PostgreSQL | Stores persistent application data |
| pgvector | Stores and retrieves vector embeddings |
| Sentence Transformers | Generates document embeddings |
| LangGraph | Coordinates advanced research workflows |
| MLflow | Supports experiment tracking and AI evaluation |
| Langfuse | Supports LLM observability |
| Object Storage | Stores original documents and generated reports |

Not all supporting systems will be introduced in the MVP.

---

# 3. Use Case Overview

## 3.1 Primary Use Cases

| ID | Use case | Primary actor | Product phase |
|---|---|---|---|
| UC-001 | Create Research Project | Researcher | MVP |
| UC-002 | View Research Project | Researcher | MVP |
| UC-003 | Upload Interview Document | Researcher | MVP |
| UC-004 | Validate Uploaded Document | System | MVP |
| UC-005 | Extract Document Content | System | MVP |
| UC-006 | Transform and Validate Extracted Data | System | MVP |
| UC-007 | Extract Customer Insights | System | MVP |
| UC-008 | Validate AI-Generated Findings | System | MVP |
| UC-009 | Export Structured Findings | Researcher | MVP |
| UC-010 | Process Multiple Interviews | Researcher | Advanced |
| UC-011 | Identify Recurring Customer Themes | System | Advanced |
| UC-012 | Identify Contradictory Findings | System | Advanced |
| UC-013 | Retrieve Historical Research | Researcher | Advanced |
| UC-014 | Compare Customer Segments | Researcher | Advanced |
| UC-015 | Review and Edit Findings | Reviewer | Advanced |
| UC-016 | Approve or Reject Findings | Reviewer | Advanced |
| UC-017 | Generate Research Report | Researcher | Advanced |
| UC-018 | Download Research Report | Researcher | Advanced |
| UC-019 | Monitor Processing Workflows | Administrator | Production |
| UC-020 | Manage User Access | Administrator | Production |
| UC-021 | Inspect AI Performance | Administrator | Production |
| UC-022 | Reprocess a Document | Researcher/Administrator | Production |

---

# 4. Use Case Specifications

# 4.1 Research Project Management

## UC-001: Create Research Project

**Primary actor:** Researcher  
**Priority:** High  
**Phase:** MVP

### Description

Allows a researcher to create a customer research project that will contain related interviews, documents and findings.

### Preconditions

- The application is accessible.
- The researcher can access the project creation functionality.

### Main Flow

1. The researcher opens the project management interface.
2. The researcher selects the option to create a new project.
3. The system presents a project creation form.
4. The researcher provides a project name.
5. The researcher provides a research objective.
6. The researcher submits the form.
7. The system validates the supplied information.
8. The system generates a unique project identifier.
9. The system creates the project.
10. The system confirms successful project creation.

### Alternative Flows

**A1: Missing project name**

1. The researcher submits the form without a project name.
2. The system detects the missing required field.
3. The system displays a validation error.
4. The researcher corrects the information.

**A2: Invalid project information**

1. The researcher submits invalid information.
2. The system rejects the request.
3. The system communicates the validation failure.

### Postconditions

- A research project exists.
- The project has a unique identifier.
- The project is available for document association.

---

## UC-002: View Research Project

**Primary actor:** Researcher  
**Priority:** High  
**Phase:** MVP

### Description

Allows a researcher to view project information and its associated research materials.

### Preconditions

- A research project exists.

### Main Flow

1. The researcher opens the project dashboard.
2. The system retrieves available projects.
3. The system displays project names and relevant metadata.
4. The researcher selects a project.
5. The system retrieves its details.
6. The system displays associated documents and available findings.

### Alternative Flows

**A1: No existing projects**

The system displays an appropriate message indicating that no research projects have been created.

**A2: Project not found**

The system displays an appropriate error message.

### Postconditions

The researcher can inspect the selected project's information.

---

# 4.2 Document Ingestion and Processing

## UC-003: Upload Interview Document

**Primary actor:** Researcher  
**Priority:** High  
**Phase:** MVP

### Description

Allows a researcher to upload a customer interview transcript for processing.

### Preconditions

- A research project exists.
- The researcher has access to the project.
- The document is in a supported format.

### Main Flow

1. The researcher opens a project.
2. The researcher navigates to the document upload interface.
3. The researcher selects a document.
4. The system receives the uploaded file.
5. The system validates the file format and size.
6. The system calculates a document hash.
7. The system checks for potential duplicates.
8. The system records document metadata.
9. The system preserves the original document.
10. The system confirms successful upload.

### Alternative Flows

**A1: Unsupported document format**

1. The researcher uploads an unsupported file.
2. The system identifies the unsupported format.
3. The system rejects the upload.
4. The system displays the accepted file formats.

**A2: File exceeds size limit**

1. The researcher uploads an oversized document.
2. The system detects the violation.
3. The system rejects the document.

**A3: Duplicate document**

1. The researcher uploads a document that has already been processed or registered.
2. The system detects a matching document hash.
3. The system identifies the potential duplicate.
4. The system informs the researcher.

**A4: Corrupted document**

1. The researcher uploads a corrupted file.
2. The system identifies that the document cannot be processed.
3. The system rejects the file with an appropriate error.

### Postconditions

- A valid document is associated with a research project.
- Original document information is preserved.
- Document metadata is recorded.

---

## UC-004: Validate Uploaded Document

**Primary actor:** System  
**Priority:** High  
**Phase:** MVP

### Description

Checks whether an uploaded document satisfies the system's ingestion requirements.

### Preconditions

- A document has been uploaded.

### Main Flow

1. The system receives the uploaded document.
2. The system checks its file extension.
3. The system validates the actual file format.
4. The system checks file size.
5. The system checks whether the document can be opened.
6. The system checks for readable content.
7. The system records validation results.
8. The system marks the document as valid or invalid.

### Alternative Flows

**A1: Empty document**

The system identifies that the document contains no usable text and marks it as invalid for further processing.

**A2: Unreadable document**

The system records an extraction or reading error and stops processing.

**A3: Unsupported content**

The system identifies content that cannot be reliably extracted and records the limitation.

### Postconditions

The document has a recorded validation status.

---

## UC-005: Extract Document Content

**Primary actor:** System  
**Priority:** High  
**Phase:** MVP

### Description

Extracts text and available structural information from customer interview documents.

### Preconditions

- The document has passed initial validation.

### Main Flow

1. The system identifies the document format.
2. The system selects the appropriate document parser.
3. The system opens the document.
4. The system extracts paragraphs.
5. The system identifies headings where available.
6. The system extracts tables.
7. The system preserves identifiable speaker labels.
8. The system records document structure.
9. The system stores extracted raw text.
10. The system records extraction status.

### Alternative Flows

**A1: Document contains no paragraphs**

The system checks whether usable content exists in tables or other supported structures.

**A2: Table extraction failure**

The system records the extraction failure and identifies the affected content.

**A3: Unsupported document structure**

The system extracts available content and records structural limitations.

### Postconditions

- Raw extracted text is available.
- Relevant document structure is preserved.
- Source document information remains associated with extracted content.

---

## UC-006: Transform and Validate Extracted Data

**Primary actor:** System  
**Priority:** High  
**Phase:** MVP

### Description

Transforms extracted document content into a consistent representation suitable for downstream analysis.

### Preconditions

- Document extraction has completed.

### Main Flow

1. The system receives extracted raw text.
2. The system normalises whitespace.
3. The system handles unnecessary formatting characters.
4. The system normalises identifiable speaker labels where appropriate.
5. The system checks for empty sections.
6. The system validates extracted content.
7. The system preserves source references.
8. The system stores normalised text.
9. The system records transformation metadata.
10. The system marks the document as ready for AI processing.

### Alternative Flows

**A1: No usable text after normalisation**

The system marks the document as unsuitable for further processing.

**A2: Missing speaker information**

The system retains the interview content without inventing speaker identities.

**A3: Transformation error**

The system records the error and preserves the original extracted content.

### Postconditions

A validated and normalised representation of the interview is available.

---

# 4.3 AI-Powered Customer Intelligence

## UC-007: Extract Customer Insights

**Primary actor:** System  
**Supporting actor:** Ollama  
**Priority:** High  
**Phase:** MVP

### Description

Uses a locally hosted LLM to identify structured customer intelligence from interview transcripts.

### Preconditions

- A document has been successfully extracted and transformed.
- The LLM service is available.
- The document content is within supported processing limits.

### Main Flow

1. The system retrieves the normalised interview transcript.
2. The system constructs an extraction prompt.
3. The system supplies the transcript to the LLM.
4. The LLM analyses the interview.
5. The LLM identifies customer problems.
6. The LLM identifies customer needs.
7. The LLM identifies feature requests.
8. The LLM identifies supporting quotations.
9. The LLM returns structured results.
10. The system receives the response.
11. The system records model and prompt metadata.

### Alternative Flows

**A1: LLM service unavailable**

1. The system attempts to contact the LLM.
2. The service is unavailable.
3. The system records the failure.
4. The document remains available for later processing.

**A2: Invalid LLM response**

1. The LLM returns an unexpected response.
2. The system identifies the invalid structure.
3. The response is rejected.
4. The system records the validation failure.

**A3: Insufficient information**

1. The LLM analyses the transcript.
2. The transcript contains insufficient evidence for a particular insight category.
3. The system records no supported finding for that category rather than inventing one.

### Postconditions

Structured customer insight candidates are available for validation.

---

## UC-008: Validate AI-Generated Findings

**Primary actor:** System  
**Priority:** High  
**Phase:** MVP

### Description

Validates generated customer insights against predefined data structures and their original source material.

### Preconditions

- The LLM has returned extraction results.

### Main Flow

1. The system receives the generated output.
2. The system parses the response.
3. The system validates the output against Pydantic schemas.
4. The system checks required fields.
5. The system validates finding categories.
6. The system checks that supporting quotations exist in the source transcript.
7. The system checks source references where available.
8. The system identifies unsupported or invalid findings.
9. The system accepts valid findings.
10. The system records validation results.

### Alternative Flows

**A1: Schema validation failure**

The system rejects the invalid output and records the validation errors.

**A2: Quotation does not match the source**

The system flags the affected finding for rejection or correction.

**A3: Missing supporting quotation**

The system identifies the finding as lacking sufficient direct evidence.

### Postconditions

Only validated findings are eligible for subsequent storage and review.

---

## UC-009: Export Structured Findings

**Primary actor:** Researcher  
**Priority:** High  
**Phase:** MVP

### Description

Allows the researcher to save extracted customer insights in a structured JSON format.

### Preconditions

- AI extraction has completed.
- Structured outputs have passed validation.

### Main Flow

1. The researcher requests the extraction results.
2. The system retrieves validated findings.
3. The system serialises the findings into JSON.
4. The system includes relevant source references and metadata.
5. The system generates the JSON output.
6. The researcher saves the file.

### Alternative Flows

**A1: No valid findings**

The system generates an appropriate empty result or communicates that no supported findings are available.

**A2: Export failure**

The system records the error and informs the researcher.

### Postconditions

A structured JSON representation of the findings is available.

---

# 4.4 Cross-Interview Intelligence

## UC-010: Process Multiple Interviews

**Primary actor:** Researcher  
**Priority:** High  
**Phase:** Advanced

### Description

Allows a researcher to process multiple interview documents within a single research project.

### Preconditions

- A research project exists.
- Multiple valid documents are available.

### Main Flow

1. The researcher selects a research project.
2. The researcher uploads multiple interview documents.
3. The system validates each document independently.
4. The system extracts content from valid documents.
5. The system transforms and validates extracted data.
6. The system processes each interview through the intelligence pipeline.
7. The system associates findings with their respective source interviews.
8. The system consolidates processing results.
9. The system displays processing summaries.

### Alternative Flows

**A1: Some documents are invalid**

The system processes valid documents and reports failures for invalid ones.

**A2: One interview fails AI processing**

The system records the failure without automatically discarding successful results from other interviews.

### Postconditions

Multiple interviews have independent processing records and associated findings.

---

## UC-011: Identify Recurring Customer Themes

**Primary actor:** System  
**Priority:** High  
**Phase:** Advanced

### Description

Identifies patterns and recurring customer problems across multiple interviews.

### Preconditions

- Multiple interviews have been processed.
- Validated findings are available.

### Main Flow

1. The system retrieves findings across interviews.
2. The system groups semantically similar problems and needs.
3. The system identifies recurring patterns.
4. The system assigns candidate themes.
5. The system links themes to their supporting findings.
6. The system counts distinct interviews containing each theme.
7. The system records the supporting evidence.
8. The system presents recurring themes for review.

### Alternative Flows

**A1: Insufficient interviews**

The system indicates that there is insufficient research material for meaningful cross-interview analysis.

**A2: Findings are too dissimilar**

The system preserves individual findings instead of forcing them into common themes.

### Postconditions

Recurring themes are available with links to their supporting interviews and evidence.

---

## UC-012: Identify Contradictory Findings

**Primary actor:** System  
**Priority:** High  
**Phase:** Advanced

### Description

Identifies differing or conflicting customer perspectives across interviews.

### Preconditions

- Multiple interviews have been processed.
- Relevant findings are available.

### Main Flow

1. The system retrieves findings from multiple interviews.
2. The system compares related customer problems, needs and expectations.
3. The system identifies potentially conflicting statements.
4. The system retrieves their supporting quotations.
5. The system associates each perspective with its source interview.
6. The system distinguishes genuine contradictions from differences in context.
7. The system presents potential contradictions for human review.

### Alternative Flows

**A1: Insufficient contextual information**

The system flags the difference as uncertain rather than asserting a contradiction.

**A2: Statements are compatible**

The system records that no contradiction has been established.

### Postconditions

Potential contradictions are documented with supporting evidence and uncertainty information.

---

## UC-013: Retrieve Historical Research

**Primary actor:** Researcher  
**Priority:** Medium  
**Phase:** Advanced

### Description

Allows researchers to retrieve relevant information from previously processed interviews and research documents.

### Preconditions

- Historical research materials have been stored.
- Search functionality is available.

### Main Flow

1. The researcher enters a research question.
2. The system processes the query.
3. The system performs lexical and/or semantic retrieval.
4. The system ranks relevant document passages.
5. The system retrieves associated metadata.
6. The system returns relevant passages and source references.
7. The researcher inspects the retrieved information.

### Alternative Flows

**A1: No relevant material found**

The system communicates that no sufficiently relevant historical information was retrieved.

**A2: Retrieval service unavailable**

The system records the failure and communicates that retrieval is temporarily unavailable.

### Postconditions

Relevant historical research passages are available with source attribution.

---

## UC-014: Compare Customer Segments

**Primary actor:** Researcher  
**Priority:** Medium  
**Phase:** Advanced

### Description

Allows researchers to compare customer problems, needs and expectations across different customer groups.

### Preconditions

- Interview records contain usable participant or segment metadata.
- Multiple interviews are available.

### Main Flow

1. The researcher selects a research project.
2. The researcher selects available customer segments.
3. The system retrieves relevant findings.
4. The system groups findings by segment.
5. The system identifies similarities and differences.
6. The system presents comparative findings.
7. The researcher reviews the results.

### Alternative Flows

**A1: Missing segment information**

The system excludes unavailable segment classifications and communicates the limitation.

**A2: Insufficient sample size**

The system indicates that the comparison is limited by the available evidence.

### Postconditions

A segment comparison is available with associated evidence and limitations.

---

# 4.5 Human Review and Approval

## UC-015: Review and Edit Findings

**Primary actor:** Reviewer  
**Priority:** High  
**Phase:** Advanced

### Description

Allows a reviewer to inspect and modify AI-generated customer findings before approval.

### Preconditions

- Findings have been generated and validated.
- The reviewer can access the research project.

### Main Flow

1. The reviewer opens the findings workspace.
2. The system displays available findings.
3. The reviewer selects a finding.
4. The system displays its interpretation and supporting evidence.
5. The reviewer examines the original quotation.
6. The reviewer edits the finding where necessary.
7. The system validates the edited information.
8. The system saves the revised finding.
9. The system records the review activity.

### Alternative Flows

**A1: Reviewer identifies unsupported interpretation**

The reviewer edits or flags the finding.

**A2: Reviewer identifies incorrect evidence**

The reviewer requests correction or rejects the finding.

**A3: Invalid edit**

The system rejects the invalid modification and communicates the validation error.

### Postconditions

The finding contains the latest saved version and associated review history.

---

## UC-016: Approve or Reject Findings

**Primary actor:** Reviewer  
**Priority:** High  
**Phase:** Advanced

### Description

Allows a reviewer to approve or reject findings based on their quality and supporting evidence.

### Preconditions

- A finding exists.
- The reviewer has permission to review it.

### Main Flow

1. The reviewer opens a finding.
2. The system displays the finding and its evidence.
3. The reviewer evaluates its accuracy and relevance.
4. The reviewer selects approve or reject.
5. The system records the decision.
6. The system updates the finding status.
7. The system records the reviewer and decision timestamp.

### Alternative Flows

**A1: Finding requires further investigation**

The reviewer leaves the finding in draft or requests revision.

**A2: Finding has insufficient evidence**

The reviewer rejects the finding or flags it as requiring additional research.

### Postconditions

The finding has an updated review status.

---

# 4.6 Reporting

## UC-017: Generate Research Report

**Primary actor:** Researcher  
**Priority:** High  
**Phase:** Advanced

### Description

Generates a structured customer discovery research report from available research findings.

### Preconditions

- A research project exists.
- Relevant findings have been generated.
- Report generation functionality is available.

### Main Flow

1. The researcher opens the reporting workspace.
2. The researcher selects a research project.
3. The researcher requests a report.
4. The system retrieves the research objectives and interview metadata.
5. The system retrieves approved findings.
6. The system retrieves relevant evidence.
7. The system prepares the report sections.
8. The system generates the report using a predefined template.
9. The system validates report structure.
10. The system saves the generated report.
11. The system informs the researcher that the report is ready.

### Alternative Flows

**A1: No approved findings**

The system communicates that there is insufficient approved material for a final report.

**A2: Incomplete research data**

The system identifies missing sections or limitations.

**A3: Report generation failure**

The system records the failure and allows a subsequent retry.

### Postconditions

A research report has been generated and associated with the project.

---

## UC-018: Download Research Report

**Primary actor:** Researcher  
**Priority:** High  
**Phase:** Advanced

### Description

Allows researchers to download generated research reports.

### Preconditions

- A report has been successfully generated.
- The report is available to the researcher.

### Main Flow

1. The researcher opens the reports interface.
2. The system displays available reports.
3. The researcher selects a report.
4. The researcher requests a download.
5. The system retrieves the report.
6. The system provides the DOCX file.

### Alternative Flows

**A1: Report unavailable**

The system informs the researcher that the report cannot be retrieved.

**A2: Download failure**

The system communicates the error and allows another attempt.

### Postconditions

The researcher obtains the generated report.

---

# 4.7 Production Operations

## UC-019: Monitor Processing Workflows

**Primary actor:** Administrator  
**Priority:** Medium  
**Phase:** Production

### Description

Allows administrators to inspect document-processing and research-analysis workflows.

### Main Flow

1. The administrator opens the monitoring interface.
2. The system retrieves workflow execution records.
3. The system displays processing status.
4. The administrator inspects failed or delayed operations.
5. The administrator examines available diagnostic information.
6. The administrator initiates an authorised recovery action where supported.

### Postconditions

Workflow execution status and relevant operational information are available.

---

## UC-020: Manage User Access

**Primary actor:** Administrator  
**Priority:** High  
**Phase:** Production

### Description

Allows administrators to manage user accounts and access permissions.

### Main Flow

1. The administrator opens user management.
2. The system retrieves user accounts.
3. The administrator selects an account.
4. The administrator assigns or modifies a role.
5. The system validates the change.
6. The system saves the updated permissions.

### Alternative Flows

**A1: Unauthorised modification**

The system rejects the request.

**A2: Invalid role**

The system rejects the unsupported role assignment.

### Postconditions

The user's permissions reflect the authorised changes.

---

## UC-021: Inspect AI Performance

**Primary actor:** Administrator  
**Priority:** Medium  
**Phase:** Production

### Description

Allows administrators to inspect AI model performance, extraction quality and operational metrics.

### Main Flow

1. The administrator opens the AI monitoring interface.
2. The system retrieves evaluation and inference records.
3. The system displays relevant metrics.
4. The administrator compares model or prompt versions.
5. The administrator inspects quality regressions.
6. The administrator identifies opportunities for improvement.

### Postconditions

AI performance and operational quality information are available.

---

## UC-022: Reprocess a Document

**Primary actor:** Researcher or Administrator  
**Priority:** Medium  
**Phase:** Production

### Description

Allows authorised users to reprocess a previously uploaded document using an updated processing configuration.

### Preconditions

- A document has previously been uploaded.
- A new processing configuration or version is available.
- The user has the required permission.

### Main Flow

1. The user opens the document record.
2. The user selects the reprocessing option.
3. The system retrieves the original document.
4. The system creates a new processing run.
5. The system applies the selected processing configuration.
6. The system extracts and transforms the document.
7. The system executes the intelligence pipeline where applicable.
8. The system records the new results.
9. The system preserves the previous processing history.
10. The system reports the new processing status.

### Alternative Flows

**A1: Original document unavailable**

The system reports that reprocessing cannot proceed.

**A2: New processing run fails**

The system records the failure without deleting previous successful results.

### Postconditions

A new processing version exists, and previous processing history remains available.

---

# 5. Core End-to-End Workflows

The following workflows describe how individual use cases combine to deliver complete product functionality.

## 5.1 MVP Workflow: Single Interview to Structured Insights

**Objective:** Convert one customer interview document into validated structured insights.

### Workflow

1. Researcher creates a research project.
2. Researcher uploads a DOCX interview.
3. System validates the uploaded document.
4. System extracts paragraphs, headings and tables.
5. System preserves speaker labels where identifiable.
6. System normalises extracted content.
7. System validates the transformed data.
8. System sends the transcript to Ollama.
9. LLM identifies customer problems, needs and feature requests.
10. LLM returns supporting quotations.
11. System validates the structured output using Pydantic.
12. System checks quotations against the source transcript.
13. System exports validated findings to JSON.

### Expected Output

A structured JSON file containing:

- Interview metadata.
- Customer problems.
- Customer needs.
- Feature requests.
- Supporting quotations.
- Source references.
- Extraction metadata.
- Validation status.

---

## 5.2 Advanced Workflow: Multiple Interviews to Research Intelligence

**Objective:** Consolidate findings across multiple customer interviews.

### Workflow

1. Researcher creates a project.
2. Researcher uploads multiple interviews.
3. System validates all documents.
4. System extracts and transforms each document.
5. System processes each interview independently.
6. System generates structured findings.
7. System validates the findings.
8. System stores findings in PostgreSQL.
9. System generates embeddings.
10. System retrieves relevant contextual information.
11. System identifies recurring themes.
12. System identifies contradictory perspectives.
13. System associates every finding with source evidence.
14. Reviewer examines the consolidated findings.

### Expected Output

A consolidated research intelligence dataset containing:

- Individual interview findings.
- Recurring themes.
- Unique customer problems.
- Customer needs.
- Contradictory perspectives.
- Supporting evidence.
- Source interview references.

---

## 5.3 Reporting Workflow: Research Intelligence to Business Report

**Objective:** Generate an evidence-backed customer discovery report.

### Workflow

1. Researcher opens a completed project.
2. Researcher requests a research report.
3. System retrieves relevant findings.
4. System filters according to review status.
5. System retrieves supporting evidence.
6. System prepares the report sections.
7. System generates a DOCX report.
8. System validates the report.
9. System saves the report.
10. Researcher downloads the report.

### Expected Output

A professionally formatted Customer Discovery Research Report containing:

1. Executive Summary
2. Research Overview
3. Customer Profiles
4. Customer Problems
5. Customer Needs
6. Recurring Themes
7. Feature Requests
8. Customer Sentiment
9. Contradictory Findings
10. Opportunities
11. Recommendations
12. Evidence Appendix

---

# 6. Use Case Relationships

Some use cases depend on other use cases.

The following relationships describe the logical dependencies.

| Use case | Depends on |
|---|---|
| UC-003 Upload Interview Document | UC-001 Create Research Project |
| UC-004 Validate Uploaded Document | UC-003 Upload Interview Document |
| UC-005 Extract Document Content | UC-004 Validate Uploaded Document |
| UC-006 Transform and Validate Extracted Data | UC-005 Extract Document Content |
| UC-007 Extract Customer Insights | UC-006 Transform and Validate Extracted Data |
| UC-008 Validate AI-Generated Findings | UC-007 Extract Customer Insights |
| UC-009 Export Structured Findings | UC-008 Validate AI-Generated Findings |
| UC-011 Identify Recurring Customer Themes | UC-010 Process Multiple Interviews |
| UC-012 Identify Contradictory Findings | UC-010 Process Multiple Interviews |
| UC-013 Retrieve Historical Research | Stored research documents and indexed content |
| UC-015 Review and Edit Findings | Generated findings |
| UC-016 Approve or Reject Findings | UC-015 Review and Edit Findings |
| UC-017 Generate Research Report | Approved findings and project metadata |
| UC-018 Download Research Report | UC-017 Generate Research Report |

---

# 7. Exception and Failure Scenarios

CDIS must handle expected failures without compromising source data or previously validated results.

| Scenario | Expected system behaviour |
|---|---|
| Unsupported document | Reject upload and explain the issue |
| Corrupted DOCX | Record extraction failure |
| Empty document | Mark as unsuitable for processing |
| Duplicate document | Identify potential duplicate |
| Missing speaker information | Preserve content without inventing identities |
| Ollama unavailable | Record inference failure |
| LLM returns invalid JSON | Reject and record validation failure |
| Missing required fields | Reject invalid structured output |
| Incorrect quotation | Flag the finding |
| Insufficient evidence | Preserve uncertainty |
| Database unavailable | Fail safely and preserve recoverable processing state where supported |
| Report generation failure | Record failure and allow retry |
| Retrieval returns no results | Communicate insufficient retrieved context |
| Workflow interruption | Preserve execution history and support recovery in production |

---

# 8. Security Considerations in Use Cases

All document-processing and AI-related use cases must follow these principles:

1. Uploaded documents are untrusted input.
2. Transcript content must never override system instructions.
3. Documents must be validated before processing.
4. Confidential interview information must not be unnecessarily exposed in logs.
5. Users must only access authorised projects and documents.
6. Generated findings must be validated before persistence.
7. Evidence references must be checked against source content.
8. Reports must respect document and project access permissions.
9. Sensitive information must be handled according to defined retention policies.
10. Reprocessing must not silently overwrite previous successful results.

---

# 9. MVP Use Case Prioritisation

The initial implementation will focus on the following use cases.

| Priority | ID | Use case | Implementation milestone |
|---|---|---|---|
| 1 | UC-001 | Create Research Project | Engineering foundation |
| 2 | UC-003 | Upload Interview Document | Document intelligence |
| 3 | UC-004 | Validate Uploaded Document | Document intelligence |
| 4 | UC-005 | Extract Document Content | Document intelligence |
| 5 | UC-006 | Transform and Validate Extracted Data | Document intelligence |
| 6 | UC-007 | Extract Customer Insights | AI extraction |
| 7 | UC-008 | Validate AI-Generated Findings | AI extraction |
| 8 | UC-009 | Export Structured Findings | AI extraction |

The remaining use cases will be implemented progressively as the architecture develops.

---

# 10. Conclusion

The use cases defined in this document describe the intended behaviour of CDIS across its development lifecycle.

The MVP establishes the fundamental document-to-insights pipeline.

The advanced version expands this capability into contextual research intelligence, cross-interview analysis and human-reviewed findings.

The production version introduces operational reliability, access control, workflow recovery, monitoring and deployment capabilities.

The central objective remains consistent throughout all three maturity levels:

**Transform unstructured customer research into reliable, traceable and actionable intelligence while preserving the relationship between every important finding and its original evidence.**

**End of Document.**
