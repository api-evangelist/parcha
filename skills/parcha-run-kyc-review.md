---
name: Run a KYC (individual) compliance review
description: Start a Know Your Customer agent job for an individual, then poll for and read the check results.
api: openapi/parcha-openapi-original.json
operations:
  - start_kyc_agent_job_api_v1_startKYCAgentJob_post
  - get_job_by_id_api_v1_getJobById_get
  - get_check_results_api_v1_getCheckResults_get
---

# Run a KYC (individual) compliance review

Use Parcha's KYC agent to screen an individual (sanctions, PEP, adverse media,
proof-of-address and identity document verification).

## Auth
- `Authorization: Bearer <API_KEY>`; include your `agent_key` in the body.
- Base URL: `https://api.parcha.ai/api/v1` (sandbox: `https://demo.parcha.ai/api/v1`).

## Steps
1. **Start the review** — `POST /api/v1/startKYCAgentJob` (`start_kyc_agent_job_api_v1_startKYCAgentJob_post`).
   Body: `agent_key` and `kyc_schema` with the individual's `self_attested_data`
   (name, address, documents). Optionally set `webhook_url` / `tool_webhook_url`. Capture the job id.
2. **Poll status** — `GET /api/v1/getJobById?job_id=<id>` (`get_job_by_id_api_v1_getJobById_get`).
3. **Read results** — `GET /api/v1/getCheckResults` (`get_check_results_api_v1_getCheckResults_get`).

## Conventions & errors
- HMAC-verify webhook callbacks; no idempotency-key — correlate by your own case `id`.
- Errors: 401 / 402 / 404 / 422 as documented in errors/parcha-problem-types.yml.
