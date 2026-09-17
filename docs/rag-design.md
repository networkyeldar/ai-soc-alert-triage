# RAG Design

The RAG layer stores **analyst-confirmed false-positive history** rather than general threat intelligence. The current payload contains `alert_id`, `title`, `justification`, `full_log`, and `status=false_positive`.

For incoming alerts, the workflow embeds alert title/log context and requests the top five Qdrant matches above `0.85`. If a returned payload has `status=false_positive`, the current implementation stops further processing.

## Evaluation recommendations

Build a labeled validation set containing repeated FPs, novel FPs, true positives that resemble FPs, and unrelated alerts. Measure false suppression, FP recall, and similarity-score distributions before selecting a threshold. Consider requiring multiple supporting signals (rule ID, tenant, source type, analyst-approved scope) rather than vector similarity alone. Store embedding-model/version metadata so vectors can be re-indexed safely when the model changes.
