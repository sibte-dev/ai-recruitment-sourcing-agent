# Architecture Notes

## Design Decisions

This document expands on the high-level architecture in the main README, covering specific technical tradeoffs made while building this system.

### Why RAG instead of Boolean-only search
Boolean search relies on exact keyword matches. Candidate resumes are highly variable in phrasing, especially across non-native English speakers and international qualification formats. RAG allows semantic similarity matching — comparing meaning, not just literal text.

### Why isolate agents into separate sub-workflows
Each stage (sourcing, screening, compliance) has different failure modes and different rate-limit constraints. Isolating them makes it possible to retry or debug one stage without re-running the entire pipeline.

### Why self-hosted n8n
Data residency requirements under GDPR / EU AI Act make cloud-hosted automation tools riskier for candidate PII. Self-hosting via Docker keeps all data processing within controlled infrastructure.
