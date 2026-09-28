# Architecture and trust boundaries

## Processing flow

1. Wazuh detects events and remains authoritative for original alert fields and evidence.
2. n8n receives the selected alert, validates/normalizes its shape, validates the source IP, and applies a deduplication cooldown.
3. VirusTotal and AbuseIPDB receive the source IP for reputation lookups. They do not receive the full log bundle. Their results are evidence with availability, quota, freshness, and accuracy limitations.
4. For the supported Linux path, n8n retrieves bounded endpoint log context over SSH. The SSH account and command scope need to be least-privileged and isolated.
5. Ollama serves Qwen3:8B locally. One stage extracts relevant log evidence; a second drafts a structured analyst report from the alert and supporting context. Both stages are advisory.
6. n8n sends configured notifications. A notification is not an analyst approval callback.

## Trust zones

- **Endpoint:** monitored Ubuntu host, Wazuh Agent, operating-system logs, SSH, and UFW.
- **Monitoring:** Wazuh Manager/Indexer/Dashboard; original detection and event evidence live here.
- **Orchestration:** n8n and local Ollama/Qwen3:8B; workflow state, credentials, and model output require protection.
- **External services:** VirusTotal, AbuseIPDB, Discord, and optional Telegram. Only approved fields should cross these boundaries.

The flow deliberately separates observation, analysis, notification, and privileged action. The public export omits the privileged UFW action until a deterministic fail-closed gate and release evidence are complete.
