# Deployment Guide

## Prerequisites
- n8n with the nodes used by the exported workflow
- RabbitMQ
- Redis
- Qdrant
- PostgreSQL access to DFIR-IRIS if using analyst-feedback ingestion
- OpenAI-compatible endpoint for Qwen3 embeddings
- OpenAI-compatible chat endpoint serving the configured Gemma model
- Optional Telegram and VirusTotal credentials

## Import
Import `workflows/autosoc-sanitized.json` into n8n. Re-create all credentials inside n8n; the public export intentionally contains placeholder credential IDs. Replace `example.local`, `REPLACE_*`, `DEMO_CUSTOMER_ID`, and demo collection names.

## Qdrant
Create the collection using the vector dimensionality produced by your deployed Qwen3-Embedding-4B service and choose the distance metric used during your validation. Do not copy an assumed vector size from this repository; the source workflow does not define collection creation.

## Validation sequence
Test webhook ingestion, queue delivery/acknowledgement, Redis duplicate behavior, embedding output shape, Qdrant writes/searches, LLM JSON parsing, and finally IRIS/Telegram routing. Use synthetic events first. Add retries/dead-letter handling and monitoring before production.
