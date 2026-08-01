---
name: Run a single compliance check
description: Discover the available checks and run one individual check without a full KYB/KYC job.
api: openapi/parcha-openapi-original.json
operations:
  - get_available_checks_api_v1_getAvailableChecks_get
  - run_check_api_v1_runCheck_post
---

# Run a single compliance check

When you only need one check (e.g. business registration, sanctions screening, or a
document verification) rather than a full review, run it directly.

## Auth
- `Authorization: Bearer <API_KEY>`; include your `agent_key` in the body.
- Base URL: `https://api.parcha.ai/api/v1` (sandbox: `https://demo.parcha.ai/api/v1`).

## Steps
1. **Discover checks** — `GET /api/v1/getAvailableChecks` (`get_available_checks_api_v1_getAvailableChecks_get`)
   to list every available check and its id.
2. **Run the check** — `POST /api/v1/runCheck` (`run_check_api_v1_runCheck_post`) with the
   chosen check id, `agent_key`, and the input payload (business/individual data or a document).
   For a fast lightweight validation, `runFlashCheck` is also available.

## Conventions & errors
- Results carry a status and evidence; poll or use webhooks for longer-running checks.
- Errors: 401 / 402 / 404 / 422 — see errors/parcha-problem-types.yml.
