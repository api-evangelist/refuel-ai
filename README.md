# Refuel (refuel-ai)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Refuel is an AI data-labeling and data-enrichment platform that uses LLMs to label, clean, structure, and enrich enterprise datasets. Refuel Cloud exposes a REST API where datasets, tasks, and deployed applications transform new data in realtime, and the open-source autolabel library lets teams run the same LLM labeling workflows in their own environment.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/refuel-ai/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/refuel-ai/refs/heads/main/apis.yml)

## Tags

- AI
- LLM
- Data Labeling
- Data Enrichment
- Autolabel

## Timestamps

- **Created:** 2026-06-21
- **Modified:** 2026-06-21

## APIs

### Refuel Applications API

Deployed applications are versioned snapshots of a Task served behind a REST API. Call POST /applications/{application-name}/label with a JSON array of rows to transform new data in realtime using the application's LLM guidelines.

- **Human URL:** [https://docs.refuel.ai/catalog/introduction](https://docs.refuel.ai/catalog/introduction)
- **Base URL:** `https://cloud-api.refuel.ai`

#### Tags

- Applications
- Label
- Predict

#### Properties

- [Documentation](https://docs.refuel.ai/catalog/introduction)
- [API Reference](https://docs.refuel.ai)
- [OpenAPI](openapi/refuel-ai-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/refuel-ai.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/refuel-ai.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [GitHub](https://github.com/refuel-ai)

### Refuel Tasks API

A Task is a set of LLM guidelines defining the transformation to perform on a dataset (classification, extraction, structured output). Tasks support structured outputs, batch processing, task chaining, evaluation, and scheduled runs before being deployed as an Application.

- **Human URL:** [https://docs.refuel.ai/quickstart](https://docs.refuel.ai/quickstart)
- **Base URL:** `https://cloud-api.refuel.ai`

#### Tags

- Tasks
- Guidelines
- Transformations

#### Properties

- [Documentation](https://docs.refuel.ai/quickstart)
- [OpenAPI](openapi/refuel-ai-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/refuel-ai.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/refuel-ai.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [GitHub](https://github.com/refuel-ai)

### Refuel Datasets API

A dataset is a collection of structured / semi-structured rows you want to transform with LLMs. Datasets can be uploaded directly or imported from cloud storage (S3, GCS) and data warehouses (Snowflake, Databricks), including documents and images.

- **Human URL:** [https://docs.refuel.ai/quickstart](https://docs.refuel.ai/quickstart)
- **Base URL:** `https://cloud-api.refuel.ai`

#### Tags

- Datasets
- Upload
- Data

#### Properties

- [Documentation](https://docs.refuel.ai/quickstart)
- [OpenAPI](openapi/refuel-ai-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/refuel-ai.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/refuel-ai.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [GitHub](https://github.com/refuel-ai)

### Refuel Labeling and Predict API

Submit rows to a deployed application to run LLM labeling / prediction in realtime via the label endpoint, returning enriched output values plus confidence and explanations for each transformed field.

- **Human URL:** [https://docs.refuel.ai/catalog/introduction](https://docs.refuel.ai/catalog/introduction)
- **Base URL:** `https://cloud-api.refuel.ai`

#### Tags

- Labeling
- Predict
- Runs

#### Properties

- [Documentation](https://docs.refuel.ai/catalog/introduction)
- [OpenAPI](openapi/refuel-ai-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/refuel-ai.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/refuel-ai.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [GitHub](https://github.com/refuel-ai)

### Refuel Models API

Refuel hosts and finetunes LLMs purpose-built for data labeling. Tasks can use Refuel-hosted base models or custom finetuned models, with finetuning runs tracked and finetuned models reusable across applications.

- **Human URL:** [https://docs.refuel.ai/quickstart](https://docs.refuel.ai/quickstart)
- **Base URL:** `https://cloud-api.refuel.ai`

#### Tags

- Models
- Finetuning
- LLM

#### Properties

- [Documentation](https://docs.refuel.ai/quickstart)
- [OpenAPI](openapi/refuel-ai-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/refuel-ai.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/refuel-ai.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [GitHub](https://github.com/refuel-ai)

### Refuel Autolabel (Open Source)

Autolabel is the open-source Python library (pip install refuel-autolabel) to label, clean, and enrich text datasets with any LLM (OpenAI, Anthropic, Google, HuggingFace, vLLM, Refuel-hosted). It is a local library and SDK, not the hosted Refuel Cloud REST API; it covers classification, NER, entity matching, and question answering with few-shot and chain-of-thought prompting.

- **Human URL:** [https://github.com/refuel-ai/autolabel](https://github.com/refuel-ai/autolabel)
- **Base URL:** `https://github.com/refuel-ai/autolabel`

#### Tags

- Open Source
- Autolabel
- Python

#### Properties

- [GitHub](https://github.com/refuel-ai/autolabel)
- [Documentation](https://docs.refuel.ai)

## Common Properties

- [GitHub Organization](https://github.com/refuel-ai)
- [LinkedIn](https://www.linkedin.com/company/refuel-ai)
- [Website](https://www.refuel.ai)
- [Documentation](https://docs.refuel.ai)
- [Plans](plans/refuel-ai-plans-pricing.yml)
- [Rate Limits](rate-limits/refuel-ai-rate-limits.yml)
- [Fin Ops](finops/refuel-ai-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
