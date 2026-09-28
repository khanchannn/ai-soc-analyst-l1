# Safety review and known limitations

## Public-export safeguards

- Workflow is inactive on import.
- Credential bindings, example execution data, workflow metadata, sticky-note environment details, bot credential text, and environment-specific host addresses were removed or replaced.
- The privileged SSH node that executed a UFW deny command was removed, along with its connection edges. `BLOCK_ENABLED` is set to false in the public export.
- The source-IP enrichment flow and bounded SSH evidence-collection path remain lab-only examples and still require local review.

## Known source-workflow issue

The supplied source export connected both outputs of its `Should Block IP?` decision to the same SSH/UFW action. The configured severity and VirusTotal thresholds were not enforced at that final decision point. A basic UFW deny command was observed in a controlled lab, but the automated authorization gate was not validated end to end. The public repository therefore makes no claim of tested automatic containment.

## Before any future response implementation

- Use an explicit fail-closed policy gate that checks feature enablement, a valid non-whitelisted address, required Wazuh severity, complete and fresh enrichment, and all configured thresholds.
- Ensure only the authorized `true` output reaches a response handler; the false, unknown, malformed, and error paths must be no-action.
- Use a restricted SSH identity, an allowlisted command, explicit exit-code/state verification, audit evidence, rollback procedure, and negative/failure tests in an isolated environment.
- Keep AI-generated text out of authorization decisions. Treat log content and model output as untrusted input.

## Credential hygiene

The source JSON contained an embedded Telegram bot credential. It is not present in this repository. Rotate or revoke that credential before continuing to use the original workflow, and store any replacement in n8n’s protected credential store. Also review other provider keys and webhook URLs in the original n8n instance and its execution history.

## Data handling

Do not commit full logs, alert payloads, execution histories, the original capstone PDF, credentials, student identifiers, or private endpoint information. External reputation providers receive the selected source IP; review the applicable data-handling requirements before enabling them.
