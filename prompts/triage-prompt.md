# Triage prompt design

The production workflow asks the model to act as a senior SOC/DFIR analyst, analyze Wazuh rule description/level/log data, extract real IoCs, return TP/FP scores totaling 100, keep the summary short, and produce response guidance only above the configured TP-score threshold. It requires JSON-only output.

For a public implementation, keep environment-specific allowlists and trusted IPs outside the prompt and inject them from controlled configuration/context. Prefer schema-constrained structured output when supported by the model/API.
