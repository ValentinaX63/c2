---
id: "007"
title: "entrevista-campaign: RAG pipeline — S3 upload, chunking, Pinecone upsert"
milestone: M1
priority: P0
estimate: 5h
blockedBy: ["006"]
blocks: ["008"]
parent: null
---

## Summary

Implement the document ingestion pipeline: upload to S3, text extraction, chunking, embedding (via Anthropic), and upsert to Pinecone. Also implement the RAG search endpoint used by conversation-lambda.

## Scope

**In scope:**
- `src/db/s3.py` — S3 presigned upload, get object
- `src/db/pinecone.py` — Pinecone client wrapper (upsert, query)
- `src/knowledge_base.py` — KnowledgeBaseManager: process_document() pipeline, delete_document()
- `src/rag.py` — RAGService: search(query, campaign_id, top_k=5)
- Integration tests with mocked S3 and Pinecone clients

## Acceptance Criteria

- [ ] `KnowledgeBaseManager.process_document(file_bytes, filename, campaign_id)` uploads to S3, chunks text (≤512 tokens), embeds with Anthropic, upserts to Pinecone with `campaign_id` metadata.
- [ ] `RAGService.search(query, campaign_id)` returns top-k chunks filtered by campaign_id.
- [ ] S3 and Pinecone clients are injected (testable via mocks).
- [ ] `make test` passes with mocked external services.

## Test Plan

```bash
make install && make test
python -m pytest tests/integration/test_rag_pipeline.py -v
```

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-21, US-22 |
| `AGENTS.md` | Pinecone + S3 + Anthropic embeddings in tech stack |

## Definition of Ready

- [ ] Task 006 complete.
- [ ] Pinecone index name and dimension confirmed (Anthropic embed model output dimension).
- [ ] S3 bucket name in `.env.example`.
