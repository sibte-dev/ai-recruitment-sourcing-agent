# AI Recruitment Sourcing Agent

Multi-agent n8n workflow that automates candidate sourcing, screening, and compliance checks using Retrieval-Augmented Generation (RAG) for semantic candidate-to-role matching.

> **Note:** This repository contains a sanitized demo architecture built with dummy data. It mirrors the design patterns used in real production recruitment workflows without exposing any confidential client data, credentials, or proprietary business logic.

## Problem

Traditional keyword-based ATS search misses qualified candidates whose resumes don't literally contain the exact terms in a job description. Manual screening across EMEA/APAC/Americas time zones is slow and inconsistent across recruiters.

## Architecture

```
Webhook Trigger (new job req)
        │
        ▼
┌─────────────────────┐
│  Sourcing Agent      │  → Boolean search + LinkedIn/ATS pull
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Vector DB (RAG)      │  → embeds job requirements + candidate profiles
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Screening Agent      │  → semantic match scoring + fit summary
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Compliance Agent     │  → jurisdictional/labor-law flag check
└─────────────────────┘
        │
        ▼
   ATS Update + Slack Notification
```

## Why this design

- **RAG over keyword search** — semantic matching surfaces qualified candidates that Boolean search misses, especially across non-native-English resumes in international pipelines.
- **Separate agents per stage** — isolating sourcing, screening, and compliance into distinct n8n sub-workflows makes each stage independently testable and easier to debug when a single step fails, instead of one monolithic workflow.
- **n8n over Zapier/Make** — self-hosted via Docker gives full control over data residency, which matters for EU AI Act / GDPR compliance in cross-border hiring.

## Tech stack

- n8n (self-hosted, Docker Compose)
- LangChain (agent orchestration)
- Vector database (candidate/job embeddings)
- IBM watsonx / Claude (agent reasoning)
- PostgreSQL (pipeline state + audit log)

## Repository structure

```
/workflows     → sanitized n8n workflow JSON exports
/docs          → architecture notes & technical decisions
README.md
```

## Status

Demo/portfolio version — actively maintained as a reference architecture.
