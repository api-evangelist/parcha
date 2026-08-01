---
name: Run a KYB (business) compliance review
description: Start a Know Your Business agent job for a company, then poll for and read the check results.
api: openapi/parcha-openapi-original.json
operations:
  - start_kyb_agent_job_api_v1_startKYBAgentJob_post
  - get_job_by_id_api_v1_getJobById_get
  - get_check_results_api_v1_getCheckResults_get
---

# Run a KYB (business) compliance review

Use Parcha's KYB agent to verify a business and run compliance checks (registration,
sanctions/PEP screening, adverse media, document verification, etc.).

## Auth
- Send `Authorization: Bearer <API_KEY>` on every request (key from app.parcha.ai settings).
- Include your `agent_key` (from the dashboard "Test API Integration" dialog) in the body — use your own agent, not a default.
- Base URL: `https://api.parcha.ai/api/v1` (sandbox: `https://demo.parcha.ai/api/v1`).

## Steps
1. **Start the review** — `POST /api/v1/startKYBAgentJob` (`start_kyb_agent_job_api_v1_startKYBAgentJob_post`).
   Body: `agent_key`, and `kyb_schema` with `id` (your case id) and `self_attested_data`
   (business_name, website, addresses, documents). Optionally pass `webhook_url` /
   `tool_webhook_url` to receive results by callback instead of polling. Capture the returned job id.
2. **Poll job status** — `GET /api/v1/getJobById?job_id=<id>` (`get_job_by_id_api_v1_getJobById_get`)
   until the job is complete (or wait for the job webhook).
3. **Read results** — `GET /api/v1/getCheckResults` (`get_check_results_api_v1_getCheckResults_get`)
   to retrieve every check result for the job; each result carries a status and evidence.

## Conventions & errors
- No idempotency-key contract — do not blindly retry a start call; reuse your own `id` to correlate.
- Webhooks are HMAC-signed — verify the signature before trusting a callback.
- Errors: 401 (bad/missing key), 402 (insufficient credits), 404 (unknown job/case), 422 (schema validation — see `detail[]`). See errors/parcha-problem-types.yml.
