# PRD --- Financial Advisory Compliance RAG Assistant

## 1. Product Overview

**Sprint:** Sprint 2 --- AI Application Development with RAG

### Problem Statement

A financial advisory firm maintains market research, fund factsheets,
and regulatory disclosures, but advisors risk giving non-compliant
guidance because they cannot verify answers against the latest approved
documents.

### Product Vision

Build a Retrieval-Augmented Generation (RAG) application that lets
financial advisors ask questions about approved documents and receive
grounded answers with source citations. When sufficient evidence cannot
be retrieved, the system should avoid inventing an answer.

## 2. Target Users

### Primary --- Financial Advisors

Need to quickly find and verify information from approved documents
before communicating guidance to clients.

### Secondary --- Knowledge/Compliance Team

Maintains approved documents and the knowledge base.

## 3. Goals

1.  Upload approved documents.
2.  Extract, clean, and chunk document text.
3.  Preserve source and position metadata.
4.  Generate and store embeddings.
5.  Retrieve relevant chunks using semantic similarity.
6.  Generate grounded answers from retrieved context.
7.  Display source citations.
8.  Refuse or qualify answers when evidence is insufficient.
9.  Support conversational follow-ups.
10. Provide a backend RAG API and chat interface.
11. Evaluate retrieval and answer quality.
12. Deploy and document a reproducible application.

## 4. Non-Goals

-   Autonomous financial advice.
-   Trade or investment execution.
-   Replacing compliance professionals.
-   Guaranteeing legal/regulatory compliance.
-   Training a foundation model from scratch.
-   Generating answers from unsupported external information.

## 5. Core User Journey

1.  Advisor uploads an approved document.
2.  System extracts and cleans text.
3.  Text is split into chunks with source metadata.
4.  Chunks are embedded and stored in a vector database.
5.  Advisor asks a question.
6.  The query is embedded.
7.  Relevant chunks are retrieved using top-K similarity search.
8.  Metadata filtering/re-ranking may refine results.
9.  Retrieved context is assembled within model limits.
10. The LLM generates a grounded answer.
11. The UI displays the answer and citations.
12. If evidence is insufficient, the system safely refuses or asks for
    clarification.

## 6. Functional Requirements

### Document and Ingestion

-   Support approved source documents.
-   Extract text while preserving source identity.
-   Clean boilerplate, whitespace, and encoding.
-   Apply a defined chunking strategy.
-   Attach document ID, source name, document type, chunk ID, and
    page/position where available.

### Embeddings and Retrieval

-   Generate embeddings for chunks.
-   Store vectors, text, and metadata.
-   Convert queries into embeddings.
-   Perform top-K similarity search.
-   Support metadata filtering.
-   Support retrieval tuning and optional re-ranking.
-   Maintain a labelled test set for retrieval evaluation.

### RAG and Generation

-   Assemble retrieved context within token/context limits.
-   Instruct the LLM to answer using retrieved evidence.
-   Provide source citations.
-   Avoid unsupported claims.
-   Return a safe refusal/clarification when evidence is insufficient.
-   Support follow-up questions with conversational context.

### Application

-   Expose the RAG pipeline through an API.
-   Provide document upload/indexing functionality.
-   Provide a chat UI.
-   Support streaming responses where practical.
-   Log requests, retrieval information, errors, and usage without
    exposing secrets.

## 7. Architecture

``` text
Approved Documents
      ↓
Loading → Extraction → Cleaning → Chunking + Metadata
      ↓
Embeddings
      ↓
Vector Database
      ↑
Query → Query Embedding → Similarity Search
      ↓
Metadata Filtering / Re-ranking
      ↓
Context Assembly
      ↓
LLM + Grounded Prompt
      ↓
Answer + Source Citations
      ↓
Chat UI / API
```

## 8. Security

-   Never commit API keys.
-   Store secrets in environment variables.
-   Exclude `.env` through `.gitignore`.
-   Provide `.env.example` without real credentials.
-   Do not expose secrets in logs.

## 9. Non-Functional Requirements

-   **Reliability:** handle ingestion, embedding, retrieval, and LLM
    failures gracefully.
-   **Reproducibility:** provide dependency and setup instructions.
-   **Traceability:** grounded answers must map to source chunks.
-   **Performance:** retrieve a practical top-K set instead of the
    entire corpus.
-   **Maintainability:** keep ingestion, retrieval, generation, API, and
    UI modular.

## 10. Success Criteria

Sprint 2 is successful when: 1. Documents can be ingested. 2. Text is
cleaned and chunked. 3. Chunks have source metadata. 4. Embeddings are
generated and stored. 5. Relevant chunks are retrieved. 6. Answers use
retrieved context. 7. Answers show citations. 8. Weak retrieval triggers
safe refusal/clarification. 9. Users can interact through a chat UI. 10.
The RAG service is available through an API. 11. Retrieval and answer
quality can be evaluated. 12. The application can be deployed and
reproduced from the documentation.

## 11. Curriculum Alignment

  Module       Outcome
  ------------ ------------------------------------------------------
  3.1          RAG architecture and milestones
  3.2          Secure LLM workspace
  3.3--3.4     Document processing and embeddings
  3.5          Vector database and retrieval
  3.6          Grounded RAG pipeline
  3.7          Web application and delivery
  3.8--3.9     PRD and Mock UX
  3.10--3.11   Workspace and GitHub workflow
  3.19--3.24   Document ingestion
  3.25--3.29   Embedding generation and quality
  3.30--3.36   Vector search and retrieval evaluation
  3.37--3.43   RAG, citations, guardrails, conversation, evaluation
  3.44--3.48   API, upload, UI, streaming, monitoring
  3.49--3.50   Deployment, documentation and final delivery

## 12. Risks and Mitigation

  Risk                  Mitigation
  --------------------- -------------------------------------------------
  Outdated documents    Track source identity/version where available
  Hallucinations        Ground responses and implement refusal handling
  Poor retrieval        Evaluate queries, tune top-K, use re-ranking
  Large context         Token-aware chunking and context limits
  API failures          Error handling and appropriate retries
  Exposed keys          Environment variables and `.gitignore`
  Duplicate documents   Track source/document identifiers
  Untraceable answers   Preserve source and position metadata

## 13. Future Enhancements

-   Document version management.
-   Advanced hybrid retrieval.
-   Larger evaluation datasets.
-   Role-based access control.
-   Compliance audit trails.
-   Advanced monitoring.
