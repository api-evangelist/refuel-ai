---
name: refuel-ingest-dataset
description: >-
  Get data into Refuel — create a project, upload or point at a CSV, infer its schema,
  ingest it as a dataset, and append rows later.
api: openapi/refuel-ai-cloud-api-openapi.yml
base_url: https://cloud-api.refuel.ai
generated: '2026-08-14'
method: generated
source: openapi/refuel-ai-cloud-api-openapi.yml + https://docs.refuel.ai/guides/datasets/datasets
operations:
  - get_projects_projects_get
  - create_project_projects_post
  - get_presigned_url_datasets_url_post
  - infer_schema_datasets_infer_schema_post
  - ingest_dataset_datasets_ingest_post
  - ingest_dataset_v2_projects__project_id__datasets_post
  - get_datasets_datasets_get
  - get_items_from_dataset_datasets__dataset_id__get
  - add_items_to_dataset_datasets__dataset_id__items_post
  - append_dataset_datasets__dataset_id__append_post
  - update_dataset_datasets__dataset_id__patch
  - add_project_to_dataset_datasets__dataset_id__projects__project_id__post
---

# Ingest a dataset into Refuel

A **Dataset** is a collection of rows of structured or semi-structured data. Datasets live
inside **Projects**, and a dataset can belong to more than one project.

## Before you start

`Authorization: Bearer <REFUEL_API_KEY>` on every request. Supported sources
(`DatasetSource` enum): `file`, `uri`, `s3`, `snowflake`, `databricks`.

## Steps

1. **Get or create a project.** `get_projects_projects_get` (`GET /projects`), then
   `create_project_projects_post` (`POST /projects`) if you need a new one.

2. **Pick an ingestion route.** Two exist and both are live:

   - **V2 / multipart (preferred for local files).**
     `ingest_dataset_v2_projects__project_id__datasets_post`
     (`POST /projects/{project_id}/datasets`, `multipart/form-data`). Its own description
     reads: *"Ingests a dataset into the platform. We support the following ingestion
     methods: 1. Upload a CSV from local disk 2. Provide an accessible URI to a CSV file."*

   - **V1 / presigned-URL (for large files and object storage).**
     a. `get_presigned_url_datasets_url_post` (`POST /datasets/url`) → a signed upload URL.
     b. `PUT` the file to that URL directly (object storage, not the Refuel API).
     c. `ingest_dataset_datasets_ingest_post` (`POST /datasets/ingest`) with query
        parameters `s3_path`, `dataset_name`, `project_id`, and optionally `redact_pii`,
        `ref_dataset_id`, `append_dataset_id`. The JSON body is the column schema dict.

   Neither route is marked deprecated. Prefer V2 for new work.

3. **Infer the schema first if you don't know it.**
   `infer_schema_datasets_infer_schema_post` (`POST /datasets/infer_schema`) with
   `integration_id` and `source_path` — use this when the data sits behind a configured S3 /
   GCS / Snowflake / Databricks integration rather than in a local file.

4. **Set `redact_pii=true`** on ingest for any dataset carrying personal data. It is `false`
   by default.

5. **Verify.** `get_datasets_datasets_get` (`GET /datasets`), then
   `get_items_from_dataset_datasets__dataset_id__get`
   (`GET /datasets/{dataset_id}`) with `offset`, `max_items`, `filters`, `order_bys`.
   The row count comes back as `count` on the envelope; there is no cursor.

6. **Grow it later.** `add_items_to_dataset_datasets__dataset_id__items_post` for individual
   rows, `append_dataset_datasets__dataset_id__append_post` for a bulk append.
   `update_dataset_datasets__dataset_id__patch` renames it. Share it with another project
   with `add_project_to_dataset_datasets__dataset_id__projects__project_id__post`.

## Watch out

- Ingest is asynchronous — several of these routes return `202 Accepted`. Poll
  `get_datasets_datasets_get` / `get_items_from_dataset_datasets__dataset_id__get` for
  completion. There are no outbound webhooks; Refuel will not call you back.
- No idempotency key. A retried ingest is a second ingest.
- `200` with `success: false` and `error_msg` is a failure. Check the flag.
