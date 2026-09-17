# Architecture

## Logical workflows

### Workflow 1: analyst feedback -> RAG
A 60-second schedule queries the IRIS PostgreSQL database for customer alerts resolved as `False Positive`. New alert IDs are embedded with Qwen3-Embedding-4B and stored in Qdrant with analyst justification and source log context.

### Workflow 2: ingestion -> queue
An n8n webhook accepts JSON and publishes it to RabbitMQ. This separates event arrival rate from AI processing latency.

### Workflow 3: queue -> triage -> escalation
The RabbitMQ consumer parses the message, normalizes volatile fields, hashes the stable event representation, and checks Redis. New alerts are embedded and compared with known FP vectors in Qdrant. Non-matches are analyzed by Gemma. Parsed results above the configured TP-score gate are forwarded to DFIR-IRIS and Telegram.

## Trust boundaries
Treat the webhook, alert content, retrieved RAG text, and LLM output as untrusted data. Credentials should live in n8n's credential store or an external secret manager, not in workflow JSON. Restrict Qdrant, Redis, RabbitMQ, the model endpoint, and IRIS to trusted networks and authenticated clients.
