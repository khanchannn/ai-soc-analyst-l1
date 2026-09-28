# Lab setup and runbook

## Prerequisites

- An isolated Ubuntu endpoint with Wazuh Agent and test logs.
- A Wazuh deployment that can forward selected alerts to an n8n webhook.
- n8n with required community/model nodes installed for the export.
- Ollama reachable only from the lab network, with `qwen3:8b` installed.
- Optional VirusTotal, AbuseIPDB, Discord, and Telegram accounts/credentials if those integrations are enabled.

## Import and configure

1. Import the redacted workflow JSON in the n8n UI. Confirm the workflow is **inactive**.
2. Recreate required credentials in n8n’s credential store; the public export intentionally has no credential bindings.
3. Configure the supported Linux endpoint, webhook authentication, IP reputation credentials, local Ollama URL/model, and approved notification channels. Keep secrets out of node text fields and exported JSON.
4. Begin with synthetic events and external API stubs where possible. Confirm rate limits, timeouts, redaction, and what gets stored in n8n execution history.
5. Confirm a false condition, missing field, invalid IP, API timeout, SSH failure, model failure, and notification failure do not execute a privileged action. The public export has no such action to execute.
6. Review [safety review](safety-review.md) before considering any response path.

## Operating assumptions

The implemented lab path is Ubuntu/Linux oriented. Alert structure, SSH log locations, API quotas, model output quality, n8n static-data persistence, and notification delivery vary by deployment and must be tested in the target lab.
