# Refuel (refuel-ai)

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
