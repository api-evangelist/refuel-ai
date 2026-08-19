---
name: refuel-label-with-application
description: >-
  Transform rows of data in realtime with a deployed Refuel application — the LLM
  labeling / enrichment call, plus the feedback call that improves it.
api: openapi/refuel-ai-cloud-api-openapi.yml
base_url: https://cloud-api.refuel.ai
generated: '2026-08-14'
method: generated
source: openapi/refuel-ai-cloud-api-openapi.yml + https://docs.refuel.ai/catalog/introduction
operations:
  - get_team_applications_applications_get
  - get_application_applications__application_id__get
  - online_label_applications__application_id__label_post
  - get_items_from_application_applications__application_id__items_get
  - get_application_item_applications__application_id__items__item_id__get
  - application_feedback_applications__application_id__items__item_id__label_post
  - get_application_usage_applications__application_id__usage_get
---

# Label rows with a deployed Refuel application

A Refuel **Application** is a versioned snapshot of a Task deployed behind a REST endpoint.
This is the operation you call in production.

## Before you start

- Authenticate with `Authorization: Bearer <REFUEL_API_KEY>` on every request. One key per
  team. Every path on `cloud-api.refuel.ai` other than `/` and `/openapi.json` returns
  `401 {"message":"Unauthorized"}` without it.
- There is **no test mode and no sandbox**. This call runs against production and consumes
  quota. Verify inputs before sending.
- There is **no idempotency key**. Do not blind-retry `online_label_...` after a timeout —
  a duplicate call is a duplicate charge and a duplicate logged item. Check
  `get_items_from_application_applications__application_id__items_get` first.

## Steps

1. **Find the application.** `get_team_applications_applications_get`
   (`GET /applications`) lists everything the team has deployed. Read
   `get_application_applications__application_id__get` (`GET /applications/{application_id}`)
   for one application's expected input fields.

2. **Send rows.** `online_label_applications__application_id__label_post`
   (`POST /applications/{application_id}/label`). The request body is a **JSON array of
   objects** — each object's keys must match the application's expected input fields.
   Refuel's own catalog documentation shows the shape:

   ```
   curl -X POST 'https://cloud-api.refuel.ai/applications/resume-parsing/label' \
     -H 'accept: application/json' \
     -H 'Authorization: Bearer <YOUR_API_KEY>' \
     -H 'Content-Type: application/json' \
     -d '[{"resume_link": "https://path/to/resume.pdf"}]'
   ```

   Note that Refuel's documented example passes an application **name**
   (`resume-parsing`) where the spec declares `{application_id}` — both resolve.

   Useful query parameters, all optional, all declared in the spec:
   - `is_async` (default `false`) — accept the work and return before labeling completes.
   - `explain` (default `false`) and `explain_fields` — return the model's reasoning.
   - `redact_pii` (default `false`) — redact PII before the row reaches the LLM. **Turn this
     on for any row that carries personal data.**
   - `log_data` (default `true`) — set `false` to keep the submitted row out of Refuel's logs.
   - `model_id`, `item_id`, `telemetry`.

3. **Check the response properly.** A `200` is not sufficient proof of success. Refuel's
   envelope is `{success, error_msg, data, count}` and a soft failure arrives as
   `200` with `success: false` and a populated `error_msg`. Test `success`, not the status
   code.

4. **Improve it with feedback.** When a human corrects an output, post it back with
   `application_feedback_applications__application_id__items__item_id__label_post`
   (`POST /applications/{application_id}/items/{item_id}/label`) with the corrected label
   as the JSON body. Get `item_id` from
   `get_items_from_application_applications__application_id__items_get`.

5. **Watch consumption.** `get_application_usage_applications__application_id__usage_get`
   (`GET /applications/{application_id}/usage`).

## Rate limits and errors

- Published limits: **300 requests/minute and 100 concurrent requests**. Exceeding either
  returns `HTTP 429` (https://docs.refuel.ai/catalog/introduction).
- Refuel returns **no** `RateLimit-*`, `X-RateLimit-*` or `Retry-After` headers, so you
  cannot read remaining budget at runtime. Track your own send rate and back off
  exponentially with jitter on 429.
- `422` returns `{detail: [{loc, msg, type}]}` — `loc` names the failing field.
- Full catalog: `errors/refuel-ai-problem-types.yml`.
