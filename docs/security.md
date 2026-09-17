# Security Considerations

## Secrets
The original production export contained plaintext bearer credentials and environment identifiers. They are intentionally absent from this repository. Rotate any credential that has been exported or shared outside its intended secret store. Use n8n credentials or a dedicated secret manager.

## Network exposure
Keep Redis, RabbitMQ, Qdrant, PostgreSQL, IRIS, and local model APIs on private networks where possible. Put authentication/TLS in front of service APIs and validate webhook authentication.

## Prompt injection and untrusted logs
Security logs can contain attacker-controlled strings. Treat log text as data, not instructions. Use strict output schemas, delimit untrusted content, limit tool/action permissions, and do not allow model text alone to execute destructive response actions.

## Automated suppression
A vector match is not proof that a new alert is benign. Validate the RAG threshold and add deterministic guardrails (tenant, rule/source compatibility, freshness, analyst approval) before using matches for silent suppression. Consider routing uncertain matches to a review queue.

## LLM scores
`true_positive_score` and `false_positive_score` are model-generated values. They are not calibrated probabilities by default. Use them as workflow signals only after validation against labeled data.

## Auditability
For production, record the workflow version, model/version, prompt version, retrieved RAG records and similarity scores, final model output, automation decision, and subsequent analyst disposition. Avoid storing unnecessary sensitive log data in vector payloads.
