# Customer Discovery Intelligence System (CDIS)
## Development Roadmap

**Document:** `development_roadmap.md`  
**Project:** Customer Discovery Intelligence System (CDIS)  
**Version:** 1.0  
**Date:** November 2026  
**Status:** Initial development plan

---

# 1. Introduction

## 1.1 Purpose

This document defines the implementation roadmap for the Customer Discovery Intelligence System (CDIS).

It translates the project's requirements, use cases and architecture into a sequence of practical development milestones.

Each milestone defines:

- Development objectives.
- Features to implement.
- Technical concepts to understand.
- Technologies to introduce.
- Practical implementation tasks.
- Expected deliverables.
- Completion criteria.

The roadmap is designed around progressive implementation. Each phase builds on capabilities established in earlier phases.

## 1.2 Development Philosophy

CDIS will be developed using the following principles:

1. Learn before implementing: Understand the underlying engineering concepts before introducing new technologies.
2. Build incrementally: Implement small, testable components rather than developing the entire application simultaneously.
3. Understand architectural decisions: Know why a particular technology or design pattern is being used.
4. Test continuously: Introduce automated tests alongside functionality.
5. Preserve data integrity: Never sacrifice source data or evidence traceability for convenience.
6. Measure AI quality: Evaluate generated intelligence against manually annotated reference data.
7. Introduce complexity deliberately: Avoid adopting advanced infrastructure before the application needs it.
8. Document engineering decisions: Maintain an understandable record of architectural choices.

## 1.3 Development Environment

The development environment will use:

| Component | Technology |
|---|---|
| Operating system | Windows + WSL2 Ubuntu |
| Terminal | Git Bash / Ubuntu terminal |
| IDE | Visual Studio Code |
| Programming language | Python 3.12+ |
| Dependency management | uv |
| Version control | Git |
| Repository hosting | GitHub |
| Local AI runtime | Ollama |
| Primary database | PostgreSQL |
| Containerisation | Docker |

---

# 2. Development Overview

CDIS will be implemented through ten major milestones.

| Milestone | Name | Main outcome |
|---|---|---|
| 1 | Engineering Foundation | Configured repository and development environment |
| 2 | Document Intelligence | Reliable document ingestion and transformation pipeline |
| 3 | Persistent Backend | FastAPI application with PostgreSQL persistence |
| 4 | AI Extraction | Structured customer insight extraction using local LLMs |
| 5 | Contextual Intelligence | RAG and cross-interview analysis |
| 6 | Intelligent Workflows | Orchestrated research processing and human approval |
| 7 | Report Automation | Evidence-backed DOCX research reports |
| 8 | Complete Application | Streamlit frontend integrated with FastAPI |
| 9 | MLOps and Evaluation | Measurable AI quality and observability |
| 10 | Production Deployment | Containerised and deployed production application |

The milestones are sequential at the architectural level, although some activities may overlap.

---

# 3. Detailed Development Milestones

# MILESTONE 1 — ENGINEERING FOUNDATION

**Objective:** Establish a professional development environment and create the foundational structure of the CDIS codebase.

## 3.1.1 Learning Objectives

Understand:

- Git repositories and branching.
- Python project structure.
- Python modules and packages.
- Virtual environments.
- Dependency management.
- Environment variables.
- Application configuration.
- Logging fundamentals.
- Automated testing.
- Code quality and formatting.

## 3.1.2 Technologies

| Technology | Purpose |
|---|---|
| Python | Application development |
| uv | Environment and dependency management |
| Git | Version control |
| GitHub | Remote repository |
| pytest | Automated testing |
| Ruff | Linting and formatting |
| Pydantic Settings | Configuration management |

## 3.1.3 Implementation Tasks

### Task 1: Create GitHub Repository

- Create a repository named `customer-discovery-intelligence`.
- Initialise Git.
- Create the initial README.
- Establish the main branch.
- Configure `.gitignore`.
- Create the initial documentation directory.

### Task 2: Configure Python Environment

- Initialise the Python project using uv.
- Configure the Python version.
- Create the virtual environment.
- Understand `pyproject.toml`.
- Understand `uv.lock`.
- Install initial development dependencies.

### Task 3: Establish Project Structure

Create the initial application directories.

```text
customer-discovery-intelligence/
│
├── backend/
│   └── app/
│
├── frontend/
│
├── tests/
│
├── docs/
│
├── data/
│
├── scripts/
│
├── .github/
│
├── pyproject.toml
├── uv.lock
├── .gitignore
└── README.md
```

### Task 4: Configure Code Quality

- Configure Ruff.
- Configure pytest.
- Establish basic testing conventions.
- Create an initial automated test.
- Understand test discovery and assertions.

### Task 5: Implement Configuration and Logging

- Create environment-based configuration.
- Introduce Pydantic Settings.
- Create a central logging configuration.
- Understand logging levels.
- Understand why secrets should not be committed.

### Task 6: Establish Git Workflow

- Create feature branches.
- Practise Conventional Commits.
- Push branches to GitHub.
- Create pull requests.
- Merge validated changes.

## 3.1.4 Expected Deliverables

- GitHub repository.
- Initial project structure.
- Configured Python environment.
- Dependency lockfile.
- Working pytest configuration.
- Ruff configuration.
- Application configuration module.
- Logging module.
- Initial README.

## 3.1.5 Completion Criteria

Milestone 1 is complete when:

- The project runs locally.
- Dependencies can be installed reproducibly.
- Tests execute successfully.
- Ruff checks pass.
- Configuration loads from environment variables.
- Logging works.
- GitHub contains the initial project structure.

---

# MILESTONE 2 — DOCUMENT INTELLIGENCE

**Objective:** Build a standalone document-processing pipeline that transforms raw interview documents into validated and structured text.

This is the first major functional component of CDIS.

## 3.2.1 Learning Objectives

Understand:

- File handling in Python.
- Binary versus text files.
- DOCX document structure.
- Paragraphs and tables.
- Text extraction.
- Data normalisation.
- Regular expressions.
- Data validation.
- Data quality dimensions.
- Document metadata.
- Hashing and duplicate detection.
- Text segmentation and chunking.
- Unit testing and integration testing.

## 3.2.2 Technologies

| Technology | Purpose |
|---|---|
| python-docx | DOCX extraction |
| pypdf | PDF extraction |
| Python standard library | Text processing and hashing |
| Pydantic | Structured data validation |
| pytest | Pipeline testing |

## 3.2.3 Implementation Tasks

### Task 1: Understand Document Formats

Study:

- DOCX structure.
- Paragraphs.
- Headings.
- Tables.
- Runs and text formatting.
- PDF text extraction.
- Plain-text files.

Understand why different file formats require different parsers.

### Task 2: Implement Document Validation

Create a document validator.

It should check:

- Supported file extensions.
- Actual file format.
- Maximum file size.
- File readability.
- Empty documents.
- Duplicate file hashes.

### Task 3: Implement DOCX Extraction

Build an extractor capable of:

- Reading DOCX files.
- Extracting paragraphs.
- Extracting headings.
- Extracting tables.
- Preserving paragraph order.
- Preserving identifiable speaker labels.
- Recording source locations where possible.

### Task 4: Implement Text Normalisation

Develop transformation functions for:

- Whitespace normalisation.
- Unnecessary line-break removal.
- Formatting character handling.
- Speaker label normalisation.
- Empty section detection.

Preserve the original extracted content separately from the normalised version.

### Task 5: Implement Data Quality Validation

Introduce checks for:

| Dimension | Implementation |
|---|---|
| Completeness | Detect empty or incomplete transcripts |
| Validity | Check supported document structures |
| Consistency | Check speaker label conventions |
| Uniqueness | Detect duplicate document hashes |
| Integrity | Validate source references |
| Timeliness | Preserve document dates where available |

### Task 6: Implement Document Chunking

Learn:

- Why long documents require segmentation.
- Fixed-size chunking.
- Paragraph-based chunking.
- Overlapping chunks.
- Chunk metadata.
- Source reference preservation.

Implement an initial paragraph-based chunker.

Do not introduce embeddings yet.

### Task 7: Create Synthetic Interview Dataset

Create 10–15 realistic customer interview transcripts.

Each transcript should contain:

- Interview identifier.
- Research objective.
- Participant information.
- Interviewer questions.
- Customer responses.
- Problems.
- Needs.
- Feature requests.
- Different perspectives.
- Some ambiguous statements.

Use these documents to test the pipeline.

## 3.2.4 Expected Deliverables

- Document validation module.
- DOCX extraction module.
- PDF extraction module.
- Text normalisation module.
- Data quality validation module.
- Document chunking module.
- Synthetic interview dataset.
- Automated tests.
- Document-processing examples.

## 3.2.5 Completion Criteria

Milestone 2 is complete when:

- Supported documents can be read.
- Paragraphs and tables are extracted.
- Speaker information is preserved where available.
- Text is normalised without losing source information.
- Invalid documents are identified.
- Duplicate documents can be detected.
- Chunks retain source references.
- Automated tests cover the important processing behaviour.

---

# MILESTONE 3 — PERSISTENT BACKEND

**Objective:** Introduce FastAPI and PostgreSQL to transform the standalone processing pipeline into a persistent backend application.

## 3.3.1 Learning Objectives

Understand:

- HTTP fundamentals.
- REST API architecture.
- HTTP methods and status codes.
- Request and response schemas.
- Pydantic models.
- SQL fundamentals.
- Relational database design.
- Primary and foreign keys.
- Database normalisation.
- SQLAlchemy ORM.
- Database sessions.
- Transactions.
- Database migrations.
- Repository and service patterns.
- Dependency injection.

## 3.3.2 Technologies

| Technology | Purpose |
|---|---|
| FastAPI | Backend API |
| PostgreSQL | Persistent database |
| SQLAlchemy | Database ORM |
| Alembic | Database migrations |
| Pydantic | API validation |
| HTTPX | HTTP communication and testing |
| pytest | Automated testing |

## 3.3.3 Implementation Tasks

### Task 1: Understand Database Design

Design the initial relational model.

Core entities:

- ResearchProject.
- Document.
- ProcessingRun.
- Interview.
- Finding.
- Evidence.

Understand relationships between these entities.

### Task 2: Configure PostgreSQL

- Install or configure PostgreSQL locally.
- Create the CDIS database.
- Create a database user with appropriate permissions.
- Configure database connection settings.
- Understand connection strings.
- Practise basic SQL queries.

### Task 3: Implement SQLAlchemy Models

Create database models for the initial entities.

Understand:

- Tables.
- Columns.
- Data types.
- Relationships.
- Primary keys.
- Foreign keys.
- Constraints.

### Task 4: Introduce Alembic

- Configure database migrations.
- Create an initial migration.
- Apply migrations.
- Modify a schema.
- Generate subsequent migrations.
- Understand migration history.

### Task 5: Implement Repository Layer

Create repository functions for database operations.

Examples:

- Create project.
- Retrieve project.
- Create document record.
- Retrieve documents by project.
- Save processing results.

Understand why database access should be separated from business logic.

### Task 6: Implement Service Layer

Create application services responsible for business operations.

Examples:

- ProjectService.
- DocumentService.
- ProcessingService.

Understand how services coordinate repositories and other application components.

### Task 7: Implement FastAPI Endpoints

Initial endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/projects` | Create project |
| GET | `/projects` | List projects |
| GET | `/projects/{project_id}` | Retrieve project |
| POST | `/projects/{project_id}/documents` | Upload document |
| GET | `/projects/{project_id}/documents` | List documents |
| GET | `/documents/{document_id}` | Retrieve document |
| POST | `/documents/{document_id}/process` | Trigger document processing |

### Task 8: Integrate Document Processing

Connect the standalone document pipeline to the FastAPI application.

The API should:

1. Receive uploaded documents.
2. Validate them.
3. Store document information.
4. Execute extraction.
5. Record processing results.
6. Return meaningful responses.

### Task 9: Test the Backend

Implement:

- API endpoint tests.
- Database integration tests.
- Repository tests.
- Validation tests.
- Error-handling tests.

## 3.3.4 Expected Deliverables

- PostgreSQL database.
- Database schema.
- SQLAlchemy models.
- Alembic migrations.
- Repository layer.
- Service layer.
- FastAPI application.
- Document upload endpoints.
- API documentation.
- Automated backend tests.

## 3.3.5 Completion Criteria

Milestone 3 is complete when:

- Projects can be created and retrieved.
- Documents can be uploaded through the API.
- Document metadata is persisted.
- Processing results can be stored.
- Database relationships work correctly.
- Migrations can reproduce the database schema.
- API tests pass.

---

# MILESTONE 4 — AI EXTRACTION

**Objective:** Introduce locally hosted LLM inference to extract structured customer intelligence from interview transcripts.

## 3.4.1 Learning Objectives

Understand:

- Large language model fundamentals.
- Transformer architecture at a conceptual level.
- Tokens and context windows.
- Model inference.
- Quantisation.
- Prompt engineering.
- Structured outputs.
- Function calling concepts.
- Pydantic output validation.
- Model limitations.
- Hallucination.
- Evidence extraction.
- Model evaluation fundamentals.

## 3.4.2 Technologies

| Technology | Purpose |
|---|---|
| Ollama | Local LLM inference |
| Compatible open-weight model | Customer insight extraction |
| HTTPX | Communication with Ollama |
| Pydantic | Structured output validation |
| pytest | Automated testing |

## 3.4.3 Implementation Tasks

### Task 1: Understand Local LLM Inference

Study:

- How language models generate responses.
- How tokens are processed.
- Context window limitations.
- Model quantisation.
- CPU and GPU inference.
- Memory requirements.

### Task 2: Install and Configure Ollama

- Install Ollama.
- Pull a small compatible model.
- Run the model locally.
- Interact with it through the terminal.
- Explore its HTTP API.
- Test structured generation.
- Record hardware usage and inference speed.

Review model licensing before selecting a model for distribution or commercial use.

### Task 3: Design the LLM Interface

Create a model-independent interface.

The application should not depend directly on Ollama throughout its business logic.

The interface should support:

- Sending prompts.
- Receiving responses.
- Configuring generation parameters.
- Handling timeouts.
- Handling inference errors.

### Task 4: Design Structured Extraction Schemas

Define Pydantic schemas for:

- CustomerProblem.
- CustomerNeed.
- FeatureRequest.
- SupportingEvidence.
- InterviewExtractionResult.

Each finding should preserve:

- Finding category.
- Description.
- Supporting quotation.
- Source reference.
- Confidence or uncertainty information.
- Processing metadata.

### Task 5: Implement Prompt Engineering

Develop prompts that instruct the model to:

- Identify customer problems.
- Identify customer needs.
- Extract feature requests.
- Preserve exact quotations.
- Avoid inventing information.
- Distinguish explicit statements from interpretation.
- Return structured output.

### Task 6: Implement Extraction Pipeline

Connect:

Document pipeline → LLM interface → Structured output validation.

### Task 7: Implement Evidence Verification

Check generated quotations against the original transcript.

Reject or flag unsupported quotations.

Do not treat a syntactically valid response as proof of factual accuracy.

### Task 8: Persist AI Results

Store validated findings and associate them with:

- Research projects.
- Source documents.
- Processing runs.
- Model versions.
- Prompt versions.

## 3.4.4 Expected Deliverables

- Ollama integration.
- Model-independent LLM interface.
- Pydantic extraction schemas.
- Prompt templates.
- Customer problem extraction.
- Customer need extraction.
- Feature request extraction.
- Evidence verification.
- Persisted AI findings.
- AI integration tests.

## 3.4.5 Completion Criteria

Milestone 4 is complete when:

- The application can communicate with a local model.
- Interviews can be analysed automatically.
- Results follow the expected schemas.
- Supporting quotations can be checked against source text.
- Failed inference requests are handled safely.
- Model and prompt versions are recorded.

---

# MILESTONE 5 — CONTEXTUAL INTELLIGENCE

**Objective:** Introduce retrieval and cross-interview analysis to transform isolated interview insights into consolidated research intelligence.

## 3.5.1 Learning Objectives

Understand:

- Embeddings.
- Vector representations.
- Semantic similarity.
- Vector databases.
- PostgreSQL indexing.
- pgvector.
- Full-text search.
- Hybrid retrieval.
- Chunking strategies.
- Retrieval ranking.
- Reranking.
- Retrieval evaluation.
- Cross-document synthesis.
- Context grounding.

## 3.5.2 Technologies

| Technology | Purpose |
|---|---|
| Sentence Transformers | Embedding generation |
| PostgreSQL | Research metadata and search |
| pgvector | Vector storage and similarity search |
| PostgreSQL full-text search | Lexical retrieval |
| Python | Analysis and synthesis |

## 3.5.3 Implementation Tasks

### Task 1: Understand Embeddings

Study how text is converted into numerical vectors.

Understand:

- Embedding dimensions.
- Similarity measures.
- Cosine similarity.
- Vector normalisation.
- Model-specific embedding behaviour.

### Task 2: Benchmark Embedding Models

Evaluate lightweight candidate models.

Compare:

- Retrieval relevance.
- Semantic similarity.
- Inference speed.
- Memory requirements.
- Model licensing.

### Task 3: Configure pgvector

- Enable the PostgreSQL extension.
- Create vector columns.
- Store embeddings.
- Implement similarity queries.
- Investigate indexing options.

### Task 4: Implement Semantic Retrieval

Build functionality to retrieve relevant interview passages using semantic similarity.

### Task 5: Implement Full-Text Retrieval

Use PostgreSQL full-text search to retrieve passages based on keyword relevance.

### Task 6: Implement Hybrid Retrieval

Combine lexical and semantic retrieval.

Compare the results against individual retrieval methods.

### Task 7: Implement Cross-Interview Analysis

Build functions for:

- Recurring problem identification.
- Customer need clustering.
- Theme identification.
- Unique finding identification.
- Contradiction detection.
- Customer segment comparison.

### Task 8: Implement Evidence Aggregation

Every consolidated theme should link back to its supporting findings and source interviews.

### Task 9: Evaluate Retrieval Quality

Create reference queries with manually identified relevant passages.

Measure:

- Precision@K.
- Recall@K.
- Mean Reciprocal Rank.
- Normalised Discounted Cumulative Gain where appropriate.

## 3.5.4 Expected Deliverables

- Embedding generation module.
- pgvector integration.
- Semantic search.
- Full-text search.
- Hybrid retrieval.
- Cross-interview analysis.
- Theme identification.
- Contradiction analysis.
- Retrieval evaluation dataset.
- Retrieval benchmark results.

## 3.5.5 Completion Criteria

Milestone 5 is complete when:

- Document chunks can be embedded.
- Embeddings can be stored and retrieved.
- Relevant passages can be searched.
- Hybrid retrieval can be evaluated.
- Cross-interview patterns can be identified.
- Consolidated findings preserve evidence traceability.

---

# MILESTONE 6 — INTELLIGENT WORKFLOWS

**Objective:** Develop reliable multi-stage research workflows with explicit state management, conditional execution, recovery and human approval.

## 3.6.1 Learning Objectives

Understand:

- Workflow orchestration.
- State machines.
- Workflow state.
- Conditional execution.
- Graph-based workflows.
- Tool calling.
- Checkpointing.
- Retry mechanisms.
- Idempotency.
- Human-in-the-loop systems.
- Workflow persistence.
- Failure recovery.

## 3.6.2 Technologies

| Technology | Purpose |
|---|---|
| Python | Initial workflow implementation |
| LangGraph | Advanced workflow orchestration |
| PostgreSQL | Persistent application state |
| pytest | Workflow testing |

## 3.6.3 Implementation Tasks

### Task 1: Build a Deterministic Python Workflow

Initially coordinate the following operations using normal Python functions:

1. Document validation.
2. Content extraction.
3. Text transformation.
4. AI extraction.
5. Evidence verification.
6. Finding persistence.
7. Research analysis.

Understand how data moves between stages.

### Task 2: Introduce Explicit Workflow State

Define workflow state containing:

- Project identifier.
- Document identifier.
- Processing status.
- Extracted content reference.
- Extraction results.
- Validation results.
- Error information.
- Execution metadata.

### Task 3: Implement Failure Handling

Introduce:

- Exception handling.
- Controlled retries.
- Timeout handling.
- Failed-stage recording.
- Safe recovery.
- Idempotent processing.

### Task 4: Introduce LangGraph

Learn:

- Graph construction.
- Nodes.
- Edges.
- Conditional edges.
- State updates.
- Graph execution.

Reimplement the appropriate research workflow using LangGraph.

### Task 5: Implement Checkpointing

Preserve workflow execution state so interrupted workflows can resume where appropriate.

### Task 6: Implement Human Approval

Introduce approval stages.

A finding should not automatically become an approved research conclusion.

### Task 7: Implement Review History

Record:

- Reviewer.
- Review decision.
- Timestamp.
- Previous finding version.
- Updated finding version.
- Review comments.

## 3.6.4 Expected Deliverables

- Deterministic research workflow.
- Workflow state schema.
- LangGraph implementation.
- Retry and failure handling.
- Persistent workflow checkpoints.
- Human approval mechanism.
- Finding review history.

## 3.6.5 Completion Criteria

Milestone 6 is complete when:

- The research pipeline executes through defined stages.
- Failures are recorded.
- Supported operations can be retried safely.
- Workflow state is preserved.
- Human approval is supported.
- Previous finding versions remain recoverable.

---

# MILESTONE 7 — REPORT AUTOMATION

**Objective:** Generate professional customer discovery research reports from validated and approved research intelligence.

## 3.7.1 Learning Objectives

Understand:

- Document generation.
- DOCX structure.
- Word document styles.
- Tables.
- Headers and footers.
- Page breaks.
- Template-driven generation.
- Report data preparation.
- Evidence referencing.
- File storage.

## 3.7.2 Technologies

| Technology | Purpose |
|---|---|
| python-docx | Word document generation |
| Pydantic | Report data validation |
| PostgreSQL | Report metadata |
| Python | Report assembly |

## 3.7.3 Implementation Tasks

### Task 1: Design Report Data Schemas

Define the structure of the complete research report.

### Task 2: Build Report Sections

Implement generators for:

- Executive summary.
- Research overview.
- Customer profiles.
- Customer problems.
- Customer needs.
- Recurring themes.
- Feature requests.
- Sentiment.
- Contradictory findings.
- Opportunities.
- Recommendations.
- Evidence appendix.

### Task 3: Implement DOCX Templates

Create a reusable Word report template.

Introduce:

- Document styles.
- Headings.
- Tables.
- Headers.
- Footers.
- Page numbering.
- Evidence formatting.

### Task 4: Implement Evidence References

Ensure quotations and findings can be traced to original interviews.

### Task 5: Implement Report Validation

Check:

- Required sections.
- Missing content.
- Evidence references.
- Output file validity.

### Task 6: Implement Report Storage

Save generated reports and their metadata.

### Task 7: Implement Download Functionality

Expose a backend endpoint for downloading generated reports.

## 3.7.4 Expected Deliverables

- Report schemas.
- DOCX templates.
- Report generator.
- Evidence appendix.
- Report validation.
- Report storage.
- Download API.

## 3.7.5 Completion Criteria

Milestone 7 is complete when:

- Reports can be generated from approved findings.
- Required sections are included.
- Evidence references are preserved.
- DOCX files open correctly.
- Reports can be retrieved and downloaded.

---

# MILESTONE 8 — COMPLETE APPLICATION

**Objective:** Build the Streamlit frontend and integrate it with the FastAPI backend.

## 3.8.1 Learning Objectives

Understand:

- Streamlit application architecture.
- Multipage applications.
- Session state.
- Forms.
- Callbacks.
- File upload components.
- API communication.
- HTTPX.
- Data tables.
- Interactive dashboards.
- Loading states.
- Error handling.
- Frontend and backend separation.

## 3.8.2 Technologies

| Technology | Purpose |
|---|---|
| Streamlit | Frontend |
| FastAPI | Backend |
| HTTPX | API communication |
| Python | Application logic |

## 3.8.3 Implementation Tasks

### Task 1: Establish Streamlit Structure

Create the main application and pages.

Initial pages:

- Dashboard.
- Research Projects.
- Documents.
- Findings.
- Reports.

### Task 2: Implement API Client

Create reusable HTTPX functions for communicating with FastAPI.

### Task 3: Build Dashboard

Display:

- Total research projects.
- Uploaded interviews.
- Processed documents.
- Extracted findings.
- Processing status.

### Task 4: Build Project Management Interface

Implement project creation and viewing.

### Task 5: Build Document Upload Interface

Allow users to upload supported research documents.

### Task 6: Build Findings Workspace

Display:

- Customer problems.
- Customer needs.
- Feature requests.
- Supporting quotations.
- Source references.
- Review status.

### Task 7: Build Research Review Interface

Allow reviewers to inspect and modify findings.

### Task 8: Build Reporting Interface

Allow users to generate and download research reports.

### Task 9: Implement Frontend Error Handling

Handle:

- Backend unavailability.
- Invalid uploads.
- Processing failures.
- Empty results.
- Failed downloads.

## 3.8.4 Expected Deliverables

- Streamlit application.
- Dashboard.
- Project management pages.
- Document upload interface.
- Findings workspace.
- Review interface.
- Reporting interface.
- Reusable API client.

## 3.8.5 Completion Criteria

Milestone 8 is complete when:

- Users can interact with CDIS through Streamlit.
- Frontend operations communicate with FastAPI.
- Projects and documents can be managed.
- Findings can be inspected.
- Reports can be generated and downloaded.
- Errors are communicated clearly.

---

# MILESTONE 9 — MLOPS AND AI EVALUATION

**Objective:** Establish a measurable framework for evaluating, comparing and monitoring CDIS intelligence components.

## 3.9.1 Learning Objectives

Understand:

- AI evaluation methodology.
- Ground-truth datasets.
- Precision and recall.
- Classification metrics.
- Extraction accuracy.
- Retrieval metrics.
- LLM evaluation.
- Faithfulness.
- Groundedness.
- Prompt versioning.
- Model versioning.
- Experiment tracking.
- LLM tracing.
- Latency and cost monitoring.
- Production feedback loops.

## 3.9.2 Technologies

| Technology | Purpose |
|---|---|
| MLflow | Experiment tracking and evaluation |
| Langfuse | LLM observability |
| pytest | Automated evaluation tests |
| Python | Evaluation framework |

## 3.9.3 Implementation Tasks

### Task 1: Build an Annotated Evaluation Dataset

Expand the synthetic interview dataset.

Target:

- 30–50 annotated interviews.
- Manually verified customer problems.
- Manually verified customer needs.
- Feature requests.
- Supporting quotations.
- Theme labels.
- Contradictory perspectives.

### Task 2: Implement Extraction Evaluation

Evaluate:

- Problem extraction precision.
- Problem extraction recall.
- Need extraction accuracy.
- Feature request extraction accuracy.
- Quotation exactness.
- Source reference accuracy.

### Task 3: Implement Retrieval Evaluation

Evaluate retrieval using reference queries and relevant passages.

### Task 4: Implement Evidence Grounding Evaluation

Measure whether generated findings are supported by their source material.

### Task 5: Introduce MLflow

Track:

- Model versions.
- Prompt versions.
- Experiment parameters.
- Evaluation datasets.
- Evaluation metrics.
- Experiment results.

### Task 6: Introduce Langfuse

Monitor:

- LLM requests.
- Prompt versions.
- Latency.
- Token usage.
- Model outputs.
- Errors.
- Trace relationships.

Avoid storing sensitive transcript content in observability systems unless explicitly permitted.

### Task 7: Compare Models and Prompts

Run controlled experiments comparing:

- Different model families.
- Different quantisation levels.
- Different prompts.
- Different extraction configurations.

### Task 8: Establish Quality Thresholds

Define minimum acceptable quality levels for important capabilities.

Do not deploy a model or prompt update solely because it produces more fluent responses.

## 3.9.4 Expected Deliverables

- Annotated evaluation dataset.
- Extraction evaluation framework.
- Retrieval evaluation framework.
- Evidence grounding evaluation.
- MLflow experiment tracking.
- Langfuse integration.
- Model comparison results.
- AI quality documentation.

## 3.9.5 Completion Criteria

Milestone 9 is complete when:

- AI outputs can be evaluated quantitatively.
- Model and prompt experiments are reproducible.
- Retrieval performance is measurable.
- Evidence accuracy is measured.
- AI execution traces can be inspected.
- Quality regressions can be identified.

---

# MILESTONE 10 — PRODUCTION DEPLOYMENT

**Objective:** Package, secure, automate and deploy CDIS using production-oriented DevOps practices.

## 3.10.1 Learning Objectives

Understand:

- Container architecture.
- Dockerfiles.
- Docker image layers.
- Container networking.
- Docker volumes.
- Docker Compose.
- Environment configuration.
- CI/CD pipelines.
- GitHub Actions.
- Container registries.
- Image security.
- Cloud deployment.
- Persistent storage.
- Monitoring.
- Infrastructure as Code.
- Application reliability.

## 3.10.2 Technologies

| Technology | Purpose |
|---|---|
| Docker | Containerisation |
| Docker Compose | Local multi-container development |
| GitHub Actions | CI/CD |
| Ruff | Code quality |
| pytest | Automated testing |
| Trivy | Security scanning |
| Google Cloud Run | Application deployment |
| Google Cloud Storage | Object storage |
| Terraform | Infrastructure as Code |

## 3.10.3 Implementation Tasks

### Task 1: Containerise the Backend

Create a production-oriented Dockerfile for FastAPI.

Understand:

- Base images.
- Build layers.
- Dependency installation.
- Environment variables.
- Container processes.
- Port exposure.

### Task 2: Containerise the Frontend

Create a Dockerfile for Streamlit.

### Task 3: Configure Docker Compose

Coordinate:

- FastAPI.
- Streamlit.
- PostgreSQL.
- Ollama where appropriate.
- Additional supporting services when required.

Configure persistent volumes and service networking.

### Task 4: Implement Health Checks

Introduce health endpoints and container health checks.

Distinguish between:

- Application liveness.
- Application readiness.
- Dependency availability.

### Task 5: Implement GitHub Actions CI

Automate:

- Dependency installation.
- Ruff checks.
- Unit tests.
- Integration tests.
- Security checks.

### Task 6: Implement Docker Build Workflow

Automate:

- Docker image building.
- Image validation.
- Image tagging.
- Container vulnerability scanning.
- Registry publishing.

### Task 7: Implement Deployment Workflow

Deploy approved versions of CDIS to Google Cloud Run.

### Task 8: Configure Persistent Storage

Introduce appropriate cloud services for:

- PostgreSQL hosting.
- Original documents.
- Generated reports.
- Application backups.

### Task 9: Implement Production Security

Introduce:

- Authentication.
- Authorisation.
- Secret management.
- File upload restrictions.
- Secure database connections.
- Appropriate logging policies.
- Threat modelling.

### Task 10: Introduce Monitoring

Monitor:

- API availability.
- Processing failures.
- Database availability.
- Request latency.
- Resource utilisation.
- LLM inference latency.
- AI evaluation metrics.

### Task 11: Document Deployment

Create deployment instructions and operational documentation.

## 3.10.4 Expected Deliverables

- Backend Dockerfile.
- Frontend Dockerfile.
- Docker Compose configuration.
- CI pipeline.
- Docker build pipeline.
- Security scanning.
- Cloud deployment.
- Persistent storage.
- Production configuration.
- Monitoring.
- Deployment documentation.

## 3.10.5 Completion Criteria

Milestone 10 is complete when:

- The application runs through Docker Compose locally.
- CI checks execute automatically.
- Docker images build reproducibly.
- Security scanning is integrated.
- Approved application versions can be deployed.
- Persistent data is protected.
- Production health and operational metrics are available.

---

# 4. Learning Strategy

The project is also a structured engineering learning programme.

Each milestone should follow the same learning cycle.

## 4.1 The Learning Cycle

1. Understand the problem.
2. Study the underlying concepts.
3. Read relevant official documentation.
4. Design the component.
5. Implement it independently.
6. Write automated tests.
7. Debug failures.
8. Review the implementation.
9. Document what was learned.
10. Integrate the component into CDIS.

## 4.2 Implementation Teaching Standards

During development, every major component should be approached using the following structure:

| Stage | What happens |
|---|---|
| Problem explanation | Understand the problem being solved |
| Conceptual foundation | Learn the relevant technical concepts |
| Architecture | Understand where the component belongs |
| Design | Identify inputs, outputs and responsibilities |
| Implementation | Build the component step by step |
| Code explanation | Understand important lines and decisions |
| Testing | Verify expected behaviour |
| Debugging | Understand common failure scenarios |
| Review | Identify weaknesses and improvements |
| Documentation | Record implementation decisions |

The objective is independent engineering ability, not merely completing the project.

---

# 5. Git and Version Control Strategy

Git will be used throughout development.

## 5.1 Branching Strategy

| Branch | Purpose |
|---|---|
| main | Stable application |
| feature/* | New functionality |
| fix/* | Bug fixes |
| docs/* | Documentation |
| refactor/* | Internal restructuring |

A separate `develop` branch will only be introduced if the integration workflow becomes complex enough to justify it.

## 5.2 Commit Convention

Use Conventional Commits where practical.

Examples:

```text
feat: implement document validation
fix: handle empty interview transcripts
test: add document extraction tests
docs: document database architecture
refactor: separate document parsing logic
ci: add automated testing workflow
```

## 5.3 Development Workflow

1. Update the local main branch.
2. Create a feature branch.
3. Implement the functionality.
4. Write tests.
5. Run Ruff.
6. Run pytest.
7. Commit changes.
8. Push the branch.
9. Create a pull request.
10. Merge after successful validation.

---

# 6. Testing Strategy

Testing will be introduced from the beginning rather than postponed until the application is complete.

## 6.1 Unit Testing

Test individual components independently.

Examples:

- File validators.
- Text normalisers.
- Document extractors.
- Chunking functions.
- Pydantic schemas.
- Evidence verification functions.

## 6.2 Integration Testing

Test interactions between components.

Examples:

- Document extraction and validation.
- FastAPI and PostgreSQL.
- API document upload and processing.
- LLM integration.
- Report generation and storage.

## 6.3 End-to-End Testing

Test complete user workflows.

Examples:

- Create project.
- Upload interview.
- Process interview.
- Extract findings.
- Review findings.
- Generate report.
- Download report.

## 6.4 AI Evaluation Testing

Evaluate AI-specific quality.

Examples:

- Extraction precision.
- Extraction recall.
- Quotation accuracy.
- Retrieval relevance.
- Evidence grounding.
- Contradiction identification quality.

## 6.5 Regression Testing

Maintain tests that detect whether new changes break previously working functionality.

---

# 7. Architecture Evolution

The architecture will evolve as the product matures.

| Stage | Architecture |
|---|---|
| Milestones 1–2 | Standalone Python processing components |
| Milestones 3–4 | Modular FastAPI backend with persistent storage |
| Milestones 5–6 | Backend with retrieval and orchestrated intelligence workflows |
| Milestones 7–8 | Complete backend and Streamlit application |
| Milestones 9–10 | Observable, containerised and deployed application |

We will retain a modular monolithic architecture unless a demonstrated operational requirement justifies a different approach.

---

# 8. Scope Control

CDIS is intentionally ambitious. The following technologies will not be introduced prematurely.

| Technology | Reason for delaying |
|---|---|
| Kubernetes | Not necessary for the initial application architecture |
| Kafka | No current requirement for distributed event streaming |
| Celery | Introduce when background processing requires a separate worker system |
| Redis | Introduce when caching or distributed task coordination is justified |
| Terraform | Introduce when cloud infrastructure needs reproducible provisioning |
| Prometheus | Introduce when dedicated metrics infrastructure is justified |
| Grafana | Introduce alongside an appropriate monitoring architecture |
| Separate vector database | PostgreSQL and pgvector should be sufficient initially |

The guiding principle is to solve real engineering problems rather than collect technologies.

---

# 9. Final Project Deliverables

At the completion of the roadmap, CDIS should contain:

| Category | Deliverable |
|---|---|
| Application | Working CDIS platform |
| Data engineering | Document ingestion pipeline |
| Data quality | Document validation framework |
| Backend | FastAPI application |
| Database | PostgreSQL schema and migrations |
| AI | Structured customer insight extraction |
| Retrieval | Hybrid RAG implementation |
| Intelligence | Cross-interview analysis |
| Workflows | LangGraph orchestration |
| Human oversight | Review and approval system |
| Reporting | DOCX report generator |
| Frontend | Streamlit application |
| Evaluation | AI benchmarking framework |
| MLOps | MLflow experiments |
| LLMOps | Langfuse observability |
| DevOps | Docker and Docker Compose |
| CI/CD | GitHub Actions workflows |
| Security | Application security controls |
| Deployment | Cloud-hosted application |
| Documentation | Technical and operational documentation |

---

# 10. Definition of Project Completion

CDIS will be considered successfully completed when it can:

1. Create and manage customer research projects.
2. Accept supported interview documents.
3. Validate and extract document content.
4. Preserve source information and document lineage.
5. Extract structured customer problems, needs and feature requests.
6. Validate generated quotations against original transcripts.
7. Store research information persistently.
8. Identify recurring themes across interviews.
9. Identify potential contradictory customer perspectives.
10. Retrieve relevant historical research.
11. Support human review and approval.
12. Generate evidence-backed DOCX research reports.
13. Expose functionality through a documented REST API.
14. Provide an interactive Streamlit interface.
15. Evaluate AI performance using an annotated dataset.
16. Track experiments and monitor AI execution.
17. Run in reproducible Docker environments.
18. Execute automated CI/CD workflows.
19. Deploy to a cloud environment.
20. Maintain appropriate security, observability and recovery mechanisms.

---

# 11. Closing Statement

CDIS is not simply an LLM-powered document summarisation application.

It is an opportunity to develop practical competence in data engineering, backend architecture, database engineering, LLM engineering, retrieval systems, AI evaluation, workflow orchestration, MLOps and production software engineering.

The roadmap deliberately progresses from fundamental Python engineering to complete intelligent application delivery.

Each milestone must produce a working component, a measurable outcome and a deeper understanding of the underlying engineering principles.

**The ultimate objective is to become capable of independently designing, implementing, evaluating, deploying and maintaining a reliable AI-powered business intelligence system.**

**End of Document.**
