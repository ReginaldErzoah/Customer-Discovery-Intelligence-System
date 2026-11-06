# Customer Discovery Intelligence System (CDIS)
## API Contract Specification

**Document:** `api_contract.md`  
**Project:** Customer Discovery Intelligence System (CDIS)  
**Version:** 1.0  
**Date:** November 2026  
**Status:** Initial API specification

---

# 1. Introduction

## 1.1 Purpose

This document defines the REST API contract for the Customer Discovery Intelligence System (CDIS).

It specifies how external clients, particularly the Streamlit frontend, interact with the FastAPI backend.

The document defines:

- API architecture.
- API conventions.
- Resource definitions.
- Endpoint specifications.
- Request and response structures.
- HTTP status codes.
- Error handling.
- Validation rules.
- File upload behaviour.
- Processing status management.
- API versioning.
- Security considerations.

## 1.2 API Objectives

The CDIS API should allow authorised clients to:

1. Create and manage research projects.
2. Upload customer interview documents.
3. Retrieve document information.
4. Initiate document processing.
5. Monitor processing status.
6. Retrieve extracted customer findings.
7. Inspect supporting evidence.
8. Review and approve findings.
9. Retrieve cross-interview research themes.
10. Generate research reports.
11. Download generated reports.

## 1.3 API Technology

| Component | Technology |
|---|---|
| Framework | FastAPI |
| Language | Python |
| Request validation | Pydantic |
| Database | PostgreSQL |
| ORM | SQLAlchemy |
| Database migrations | Alembic |
| HTTP client | HTTPX |
| Testing | pytest and FastAPI TestClient |

---

# 2. API Architecture

## 2.1 Architectural Style

CDIS will expose a RESTful API.

The API will organise functionality around resources such as:

- Projects.
- Documents.
- Processing runs.
- Findings.
- Evidence.
- Themes.
- Reports.

HTTP methods will represent the operations performed on these resources.

## 2.2 Base URL

During local development:

```text
http://localhost:8000
```

API version 1:

```text
http://localhost:8000/api/v1
```

The version prefix allows the API to evolve while providing a predictable interface to clients.

## 2.3 API Documentation

FastAPI will automatically expose interactive API documentation.

Development endpoints:

```text
/docs
/redoc
/openapi.json
```

These provide:

- Interactive endpoint testing.
- Request schema inspection.
- Response schema inspection.
- API contract generation.

The generated OpenAPI specification should remain consistent with this document.

---

# 3. API Design Conventions

## 3.1 HTTP Methods

| Method | Purpose | Example |
|---|---|---|
| GET | Retrieve resources | Retrieve a project |
| POST | Create a resource or initiate an operation | Create a project |
| PATCH | Partially update a resource | Update project description |
| PUT | Replace a resource representation | Replace a complete configuration |
| DELETE | Delete a resource | Delete an eligible document |

The API should use methods consistently.

## 3.2 Resource Naming

Use plural resource names.

Examples:

```text
/projects
/documents
/findings
/reports
```

Use nested paths where the relationship is meaningful.

Example:

```text
/projects/{project_id}/documents
```

Avoid exposing implementation-specific names in public endpoints.

## 3.3 Identifiers

Resource identifiers will use UUIDs.

Example:

```text
/projects/550e8400-e29b-41d4-a716-446655440000
```

Clients should treat these identifiers as opaque strings rather than assuming a particular UUID generation strategy.

## 3.4 Request and Response Format

JSON will be the default request and response format.

File uploads will use `multipart/form-data`.

Example JSON request:

```json
{
  "name": "Customer Payment Experience",
  "description": "Research into customer payment challenges",
  "research_objective": "Identify barriers affecting payment completion"
}
```

## 3.5 Date and Time Format

Dates should use ISO 8601.

Example date:

```text
2026-11-06
```

Timestamps should include timezone information.

Example:

```text
2026-11-06T15:30:00Z
```

The backend should store timestamps consistently, preferably in UTC.

## 3.6 Pagination

List endpoints should support pagination.

Initial query parameters:

| Parameter | Type | Default | Description |
|---|---|---|---|
| limit | Integer | 20 | Maximum records returned |
| offset | Integer | 0 | Number of records skipped |

Example:

```text
GET /api/v1/projects?limit=20&offset=0
```

The API should enforce a reasonable maximum limit.

## 3.7 Filtering

List endpoints should support appropriate filtering.

Examples:

```text
GET /api/v1/findings?finding_type=customer_problem
```

```text
GET /api/v1/documents?status=completed
```

Only documented filter parameters should be accepted.

---

# 4. Standard Response Structures

## 4.1 Successful Resource Response

Successful requests should return structured JSON.

Example:

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Customer Payment Experience",
  "description": "Research into customer payment challenges",
  "research_objective": "Identify barriers affecting payment completion",
  "status": "active",
  "created_at": "2026-11-06T15:30:00Z",
  "updated_at": "2026-11-06T15:30:00Z"
}
```

## 4.2 Paginated Response

List endpoints should return a consistent pagination structure.

Example:

```json
{
  "items": [],
  "total": 45,
  "limit": 20,
  "offset": 0
}
```

Where:

- `items` contains the returned records.
- `total` is the total number of matching records.
- `limit` is the requested page size.
- `offset` is the starting position.

## 4.3 Error Response

Errors should follow a consistent structure.

Example:

```json
{
  "error": {
    "code": "DOCUMENT_NOT_FOUND",
    "message": "The requested document could not be found.",
    "details": null,
    "request_id": "a8d5f5f1-8d1c-4bb8-8db0-5f54f3e0e95a"
  }
}
```

Error fields:

| Field | Purpose |
|---|---|
| code | Machine-readable error identifier |
| message | Human-readable explanation |
| details | Optional additional validation information |
| request_id | Identifier for correlating logs |

Validation errors may include field-level details.

Example:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request contains invalid data.",
    "details": [
      {
        "field": "name",
        "message": "This field is required."
      }
    ],
    "request_id": "a8d5f5f1-8d1c-4bb8-8db0-5f54f3e0e95a"
  }
}
```

---

# 5. Resource 1: Research Projects

Projects organise related customer research activities.

## 5.1 Create Project

**Endpoint**

```http
POST /api/v1/projects
```

**Purpose**

Create a new research project.

**Request body**

```json
{
  "name": "Customer Payment Experience",
  "description": "Research into customer payment challenges",
  "research_objective": "Identify barriers affecting payment completion"
}
```

**Response — 201 Created**

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Customer Payment Experience",
  "description": "Research into customer payment challenges",
  "research_objective": "Identify barriers affecting payment completion",
  "status": "active",
  "created_at": "2026-11-06T15:30:00Z",
  "updated_at": "2026-11-06T15:30:00Z"
}
```

**Possible status codes**

- `201`: Project created.
- `422`: Invalid request data.

## 5.2 List Projects

**Endpoint**

```http
GET /api/v1/projects
```

**Purpose**

Retrieve research projects.

**Query parameters**

| Parameter | Type | Description |
|---|---|---|
| limit | Integer | Maximum records |
| offset | Integer | Pagination offset |
| status | String | Optional project status |

**Response — 200 OK**

```json
{
  "items": [],
  "total": 0,
  "limit": 20,
  "offset": 0
}
```

## 5.3 Retrieve Project

**Endpoint**

```http
GET /api/v1/projects/{project_id}
```

**Purpose**

Retrieve one project.

**Possible status codes**

- `200`: Project retrieved.
- `404`: Project not found.

## 5.4 Update Project

**Endpoint**

```http
PATCH /api/v1/projects/{project_id}
```

**Purpose**

Update selected project fields.

**Request body**

```json
{
  "description": "Updated research description",
  "status": "active"
}
```

Only supplied fields should be updated.

**Possible status codes**

- `200`: Project updated.
- `404`: Project not found.
- `422`: Invalid request data.

## 5.5 Delete Project

**Endpoint**

```http
DELETE /api/v1/projects/{project_id}
```

**Purpose**

Delete or archive a project according to the application's retention policy.

**Design consideration**

Projects containing documents, evidence and approved findings should not be permanently deleted automatically.

Archival is preferred for the initial implementation.

**Possible status codes**

- `204`: Operation completed.
- `404`: Project not found.
- `409`: Project cannot be deleted because of dependent records or policy restrictions.

---

# 6. Resource 2: Documents

Documents represent uploaded research materials.

## 6.1 Upload Document

**Endpoint**

```http
POST /api/v1/projects/{project_id}/documents
```

**Purpose**

Upload a customer interview document to a research project.

**Content type**

```text
multipart/form-data
```

**Request fields**

| Field | Type | Required | Description |
|---|---|---|---|
| file | File | Yes | Interview document |
| research_date | Date | No | Date of the research interview |
| document_description | String | No | Additional document description |

Supported initial formats:

- DOCX.
- TXT.

PDF can be enabled after its extraction and validation pipeline is implemented.

**Response — 201 Created**

```json
{
  "id": "650e8400-e29b-41d4-a716-446655440000",
  "project_id": "550e8400-e29b-41d4-a716-446655440000",
  "filename": "customer_interview_001.docx",
  "file_type": "docx",
  "file_size": 24576,
  "status": "uploaded",
  "uploaded_at": "2026-11-06T15:30:00Z"
}
```

**Possible status codes**

- `201`: Document uploaded.
- `404`: Project not found.
- `413`: File too large.
- `415`: Unsupported file type.
- `422`: Invalid document.
- `409`: Duplicate document according to the deduplication policy.

## 6.2 List Project Documents

**Endpoint**

```http
GET /api/v1/projects/{project_id}/documents
```

**Purpose**

Retrieve all documents associated with a project.

**Query parameters**

| Parameter | Type | Description |
|---|---|---|
| limit | Integer | Maximum records |
| offset | Integer | Pagination offset |
| status | String | Optional processing status |

**Response — 200 OK**

```json
{
  "items": [],
  "total": 0,
  "limit": 20,
  "offset": 0
}
```

## 6.3 Retrieve Document

**Endpoint**

```http
GET /api/v1/documents/{document_id}
```

**Purpose**

Retrieve document metadata and current processing status.

**Response — 200 OK**

```json
{
  "id": "650e8400-e29b-41d4-a716-446655440000",
  "project_id": "550e8400-e29b-41d4-a716-446655440000",
  "filename": "customer_interview_001.docx",
  "file_type": "docx",
  "file_size": 24576,
  "status": "completed",
  "uploaded_at": "2026-11-06T15:30:00Z"
}
```

## 6.4 Delete Document

**Endpoint**

```http
DELETE /api/v1/documents/{document_id}
```

**Purpose**

Delete or archive a document according to data retention policies.

**Possible status codes**

- `204`: Operation completed.
- `404`: Document not found.
- `409`: Document cannot be deleted because of dependent records or retention restrictions.

---

# 7. Resource 3: Document Processing

Document processing transforms uploaded research material into structured information.

## 7.1 Initiate Processing

**Endpoint**

```http
POST /api/v1/documents/{document_id}/process
```

**Purpose**

Start document processing.

Processing includes:

1. Document validation.
2. Content extraction.
3. Text normalisation.
4. Data quality validation.
5. Chunking.
6. AI extraction when enabled.
7. Evidence verification.
8. Finding persistence.

**Request body**

```json
{
  "enable_ai_extraction": true,
  "enable_chunking": true
}
```

These options represent initial processing controls and may be refined during implementation.

**Response — 202 Accepted**

```json
{
  "processing_run_id": "750e8400-e29b-41d4-a716-446655440000",
  "document_id": "650e8400-e29b-41d4-a716-446655440000",
  "status": "pending",
  "message": "Document processing has been accepted."
}
```

**Why 202 instead of 200?**

The request has been accepted, but the complete processing operation may not have finished.

This is particularly important once background processing is introduced.

## 7.2 Retrieve Processing Status

**Endpoint**

```http
GET /api/v1/processing-runs/{processing_run_id}
```

**Purpose**

Retrieve the current processing state.

**Response — 200 OK**

```json
{
  "id": "750e8400-e29b-41d4-a716-446655440000",
  "document_id": "650e8400-e29b-41d4-a716-446655440000",
  "status": "processing",
  "started_at": "2026-11-06T15:30:00Z",
  "completed_at": null,
  "error_message": null
}
```

## 7.3 List Processing Runs

**Endpoint**

```http
GET /api/v1/documents/{document_id}/processing-runs
```

**Purpose**

Retrieve the processing history for a document.

This endpoint is important for debugging, reproducibility and model comparisons.

---

# 8. Resource 4: Extracted Findings

Findings represent customer problems, needs and feature requests.

## 8.1 List Findings

**Endpoint**

```http
GET /api/v1/projects/{project_id}/findings
```

**Purpose**

Retrieve findings associated with a research project.

**Query parameters**

| Parameter | Type | Description |
|---|---|---|
| finding_type | String | Filter by finding category |
| review_status | String | Filter by review status |
| document_id | UUID | Filter by source document |
| limit | Integer | Maximum records |
| offset | Integer | Pagination offset |

**Response — 200 OK**

```json
{
  "items": [
    {
      "id": "850e8400-e29b-41d4-a716-446655440000",
      "project_id": "550e8400-e29b-41d4-a716-446655440000",
      "finding_type": "customer_problem",
      "title": "Difficulty tracking order progress",
      "description": "Customers struggle to determine when their orders will arrive.",
      "interpretation": "Customers may lack sufficient order-status visibility.",
      "confidence": "medium",
      "review_status": "pending_review",
      "created_at": "2026-11-06T15:30:00Z"
    }
  ],
  "total": 1,
  "limit": 20,
  "offset": 0
}
```

## 8.2 Retrieve Finding

**Endpoint**

```http
GET /api/v1/findings/{finding_id}
```

**Purpose**

Retrieve a finding and its associated metadata.

**Response — 200 OK**

```json
{
  "id": "850e8400-e29b-41d4-a716-446655440000",
  "finding_type": "customer_problem",
  "title": "Difficulty tracking order progress",
  "description": "Customers struggle to determine when their orders will arrive.",
  "interpretation": "Customers may lack sufficient order-status visibility.",
  "confidence": "medium",
  "review_status": "pending_review",
  "created_at": "2026-11-06T15:30:00Z"
}
```

## 8.3 Retrieve Finding Evidence

**Endpoint**

```http
GET /api/v1/findings/{finding_id}/evidence
```

**Purpose**

Retrieve all supporting quotations for a finding.

**Response — 200 OK**

```json
{
  "items": [
    {
      "id": "950e8400-e29b-41d4-a716-446655440000",
      "finding_id": "850e8400-e29b-41d4-a716-446655440000",
      "document_id": "650e8400-e29b-41d4-a716-446655440000",
      "quote": "I never know when my order will arrive.",
      "source_location": {
        "paragraph": 18
      },
      "verification_status": "verified"
    }
  ],
  "total": 1
}
```

## 8.4 Update Finding

**Endpoint**

```http
PATCH /api/v1/findings/{finding_id}
```

**Purpose**

Allow an authorised reviewer to update a finding.

**Request body**

```json
{
  "title": "Limited visibility into order progress",
  "description": "Customers cannot reliably determine when their orders will arrive.",
  "interpretation": "The existing order tracking experience may not provide sufficient information."
}
```

Updates should preserve finding history.

The application should record who made the change and why.

**Possible status codes**

- `200`: Finding updated.
- `404`: Finding not found.
- `422`: Invalid update.
- `403`: User is not authorised.

---

# 9. Resource 5: Human Review

## 9.1 Submit Review Decision

**Endpoint**

```http
POST /api/v1/findings/{finding_id}/reviews
```

**Purpose**

Record a human review decision.

**Request body**

```json
{
  "decision": "approved",
  "comments": "The finding is supported by the customer quotation."
}
```

Possible decisions:

- `approved`
- `rejected`
- `needs_revision`

**Response — 201 Created**

```json
{
  "id": "a50e8400-e29b-41d4-a716-446655440000",
  "finding_id": "850e8400-e29b-41d4-a716-446655440000",
  "decision": "approved",
  "comments": "The finding is supported by the customer quotation.",
  "reviewed_at": "2026-11-06T16:00:00Z"
}
```

## 9.2 Retrieve Review History

**Endpoint**

```http
GET /api/v1/findings/{finding_id}/reviews
```

**Purpose**

Retrieve the complete review history for a finding.

Previous review records must not be overwritten by subsequent decisions.

---

# 10. Resource 6: Research Themes

Themes represent patterns identified across multiple customer interviews.

## 10.1 List Research Themes

**Endpoint**

```http
GET /api/v1/projects/{project_id}/themes
```

**Purpose**

Retrieve themes associated with a project.

**Response — 200 OK**

```json
{
  "items": [
    {
      "id": "b50e8400-e29b-41d4-a716-446655440000",
      "name": "Payment friction",
      "description": "Recurring difficulties customers experience during payment.",
      "status": "active"
    }
  ],
  "total": 1
}
```

## 10.2 Retrieve Theme

**Endpoint**

```http
GET /api/v1/themes/{theme_id}
```

**Purpose**

Retrieve a theme and its associated findings.

## 10.3 Initiate Cross-Interview Analysis

**Endpoint**

```http
POST /api/v1/projects/{project_id}/analyze
```

**Purpose**

Initiate project-level research analysis.

Potential operations:

- Recurring theme identification.
- Unique need identification.
- Contradiction detection.
- Segment comparison.

**Request body**

```json
{
  "analysis_types": [
    "recurring_themes",
    "unique_needs",
    "contradictions"
  ]
}
```

**Response — 202 Accepted**

```json
{
  "analysis_run_id": "c50e8400-e29b-41d4-a716-446655440000",
  "project_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "pending",
  "message": "Research analysis has been accepted."
}
```

Analysis should only use appropriate findings and source material.

The system should distinguish evidence-backed patterns from weak or speculative patterns.

---

# 11. Resource 7: Research Reports

## 11.1 Generate Report

**Endpoint**

```http
POST /api/v1/projects/{project_id}/reports
```

**Purpose**

Generate a Word research report.

**Request body**

```json
{
  "report_name": "Customer Payment Research Report",
  "include_executive_summary": true,
  "include_customer_problems": true,
  "include_customer_needs": true,
  "include_feature_requests": true,
  "include_themes": true,
  "include_evidence_appendix": true
}
```

Report generation should use appropriately reviewed findings.

The system should clearly identify any sections containing unapproved or uncertain findings.

**Response — 202 Accepted**

```json
{
  "report_id": "d50e8400-e29b-41d4-a716-446655440000",
  "project_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "generating",
  "message": "Report generation has been accepted."
}
```

## 11.2 Retrieve Report

**Endpoint**

```http
GET /api/v1/reports/{report_id}
```

**Purpose**

Retrieve report metadata and generation status.

## 11.3 Download Report

**Endpoint**

```http
GET /api/v1/reports/{report_id}/download
```

**Purpose**

Download the generated DOCX report.

**Successful response**

```http
200 OK
Content-Type: application/vnd.openxmlformats-officedocument.wordprocessingml.document
Content-Disposition: attachment
```

The actual response body contains the document file, not JSON.

**Possible status codes**

- `200`: Download successful.
- `404`: Report not found.
- `409`: Report generation is incomplete.
- `410`: Report is no longer available under the retention policy.

---

# 12. Endpoint Summary

The following table consolidates the initial API surface.

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/projects` | Create project |
| GET | `/projects` | List projects |
| GET | `/projects/{project_id}` | Retrieve project |
| PATCH | `/projects/{project_id}` | Update project |
| DELETE | `/projects/{project_id}` | Delete or archive project |
| POST | `/projects/{project_id}/documents` | Upload document |
| GET | `/projects/{project_id}/documents` | List project documents |
| GET | `/documents/{document_id}` | Retrieve document |
| DELETE | `/documents/{document_id}` | Delete or archive document |
| POST | `/documents/{document_id}/process` | Start processing |
| GET | `/processing-runs/{processing_run_id}` | Retrieve processing status |
| GET | `/documents/{document_id}/processing-runs` | List processing history |
| GET | `/projects/{project_id}/findings` | List project findings |
| GET | `/findings/{finding_id}` | Retrieve finding |
| GET | `/findings/{finding_id}/evidence` | Retrieve evidence |
| PATCH | `/findings/{finding_id}` | Update finding |
| POST | `/findings/{finding_id}/reviews` | Submit review |
| GET | `/findings/{finding_id}/reviews` | Retrieve review history |
| GET | `/projects/{project_id}/themes` | List themes |
| GET | `/themes/{theme_id}` | Retrieve theme |
| POST | `/projects/{project_id}/analyze` | Start research analysis |
| POST | `/projects/{project_id}/reports` | Generate report |
| GET | `/reports/{report_id}` | Retrieve report |
| GET | `/reports/{report_id}/download` | Download report |

This is the planned API surface. Not every endpoint will be implemented in the first MVP.

---

# 13. HTTP Status Codes

CDIS should use HTTP status codes consistently.

| Status code | Meaning | Example |
|---|---|---|
| 200 | OK | Successfully retrieve a resource |
| 201 | Created | Successfully create a project |
| 202 | Accepted | Processing accepted |
| 204 | No Content | Successful deletion |
| 400 | Bad Request | Invalid operation |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Project does not exist |
| 409 | Conflict | Duplicate upload or invalid state |
| 413 | Payload Too Large | Upload exceeds size limit |
| 415 | Unsupported Media Type | Unsupported file format |
| 422 | Unprocessable Entity | Request validation failure |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected backend failure |
| 503 | Service Unavailable | Required service unavailable |
| 504 | Gateway Timeout | Upstream operation timed out |

The API should avoid exposing internal stack traces or sensitive implementation details in error responses.

---

# 14. Validation Rules

## 14.1 Project Validation

- Project name is required.
- Project name must not be empty.
- Project name must respect a maximum length.
- Status must belong to the supported set.

## 14.2 Document Validation

- Only supported formats are accepted.
- File size must remain within configured limits.
- File contents must be readable.
- File format must match the declared type.
- Empty documents must be rejected or flagged.
- Duplicate uploads must follow the deduplication policy.

## 14.3 Processing Validation

- The document must exist.
- The document must belong to the expected project.
- A new run must not create duplicate processing records unintentionally.
- Processing status transitions must be valid.

## 14.4 Finding Validation

- Finding type must be supported.
- Description must not be empty.
- Evidence references must be valid.
- Review status must be supported.
- Human edits must be recorded.

## 14.5 Report Validation

- The project must exist.
- Required source data must be available.
- Evidence references must be valid.
- Report options must be supported.
- The generated file must pass validation before becoming downloadable.

---

# 15. API Security

The initial local development version may operate without full authentication.

Authentication and authorisation must be implemented before exposing sensitive customer research through a public production deployment.

## 15.1 Authentication

Future authentication options may include:

- OAuth 2.0.
- OpenID Connect.
- Secure token-based authentication.

## 15.2 Authorisation

The system should eventually support roles such as:

| Role | Responsibilities |
|---|---|
| Administrator | Manage users and system settings |
| Researcher | Create projects, upload documents and view findings |
| Reviewer | Review and approve findings |
| Viewer | View authorised research and reports |

These are initial role concepts, not a final permission matrix.

## 15.3 File Upload Security

- Enforce file size limits.
- Validate file contents.
- Use safe internal filenames.
- Prevent path traversal.
- Avoid executing uploaded content.
- Restrict upload access to authorised users.

## 15.4 Sensitive Information

Customer interview material may contain commercially sensitive or personal information.

The API should:

- Enforce project-level access controls.
- Avoid logging complete interview transcripts unnecessarily.
- Protect document download endpoints.
- Avoid exposing sensitive information in errors.
- Apply appropriate retention and deletion policies.

---

# 16. Asynchronous Processing Strategy

Some CDIS operations may take several seconds or minutes.

Examples:

- LLM inference.
- Processing large interviews.
- Cross-interview analysis.
- Report generation.

These operations should eventually run asynchronously.

## 16.1 Initial Development

A controlled synchronous processing implementation may be used initially to simplify learning and testing.

The API contract should still distinguish operations that are accepted for processing from those that have completed.

## 16.2 Advanced Implementation

When background processing is introduced:

1. The API accepts the operation.
2. A processing record is created.
3. A background worker executes the task.
4. Processing status is updated.
5. The frontend checks the status endpoint.
6. Results become available when processing completes.

Potential technologies:

- Celery.
- Redis.
- Cloud Tasks.

The technology should be selected according to actual execution and infrastructure requirements.

---

# 17. API Testing Strategy

The API must be tested independently of the Streamlit frontend.

## 17.1 Unit Tests

Test:

- Request schemas.
- Response schemas.
- Validation functions.
- Business logic.
- Error handling.

## 17.2 Integration Tests

Test:

- FastAPI and PostgreSQL.
- Repository operations.
- Document uploads.
- Processing records.
- Finding persistence.
- Report metadata.

## 17.3 Endpoint Tests

Test each endpoint for:

- Successful requests.
- Invalid inputs.
- Missing resources.
- Invalid state transitions.
- Permission failures.
- Database errors.

## 17.4 Contract Tests

Verify that actual responses conform to the documented response schemas.

The OpenAPI specification should be reviewed whenever API models change.

---

# 18. Implementation Priorities

The complete API should be implemented progressively.

## Phase 1: MVP API

Implement:

- Project creation.
- Project retrieval.
- Project listing.
- Document upload.
- Document retrieval.
- Document listing.
- Document processing.
- Processing status.
- Findings retrieval.
- Evidence retrieval.

## Phase 2: Research Management

Implement:

- Finding updates.
- Finding review.
- Review history.
- Research themes.
- Cross-interview analysis.

## Phase 3: Reporting

Implement:

- Report generation.
- Report retrieval.
- Report downloads.

## Phase 4: Production API

Introduce:

- Authentication.
- Authorisation.
- Rate limiting.
- Advanced error handling.
- Background processing.
- Improved observability.
- API performance optimisation.

---

# 19. API Contract Maintenance

This document must evolve alongside the implementation.

Whenever an endpoint changes, review:

- Endpoint path.
- HTTP method.
- Request schema.
- Response schema.
- Status codes.
- Validation rules.
- Error behaviour.
- Security requirements.

Avoid silently changing existing API behaviour.

Where breaking changes become necessary, consider a new API version.

---

# 20. Final API Objective

The CDIS API must provide a predictable and reliable interface for managing research projects, processing customer interviews, retrieving evidence-backed findings, supporting human review and generating research reports.

It should separate the frontend from internal application logic while ensuring that every important operation can be validated, tested and monitored.

**The central API design principle is that every operation should have a clear contract: defined inputs, predictable outputs, explicit failure behaviour and appropriate security controls.**

**End of Document.**
