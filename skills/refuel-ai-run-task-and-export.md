---
name: refuel-run-task-and-export
description: >-
  Define an LLM transformation task, run it over a dataset, read its quality metrics, and
  export the labeled results.
api: openapi/refuel-ai-cloud-api-openapi.yml
base_url: https://cloud-api.refuel.ai
generated: '2026-08-14'
method: generated
source: openapi/refuel-ai-cloud-api-openapi.yml + https://docs.refuel.ai/guides/tasks/tasks
operations:
  - create_task_projects__project_id__tasks_post
  - get_tasks_projects__project_id__tasks_get
  - get_task_tasks__task_id__get
  - edit_task_tasks__task_id__post
  - update_task_run_v2_tasks__task_id__runs_post
  - update_task_run_tasks__task_id__runs__dataset_id__post
  - get_task_runs_tasks__task_id__runs_get
  - get_task_run_tasks__task_id__runs__dataset_id__get
  - get_metrics_for_task_run_tasks__task_id__runs__dataset_id__metrics_get
  - get_items_from_task_run_tasks__task_id__datasets__dataset_id__get
  - clear_task_run_tasks__task_id__runs__dataset_id__delete
  - export_task_tasks__task_id___tag__exports_post
  - download_export_task_tasks__task_id___tag__exports__export_id__get
  - export_dataset_datasets__dataset_id__exports_post
  - download_dataset_datasets__dataset_id__exports__export_id__get
---

# Run a Refuel task and export the results

A **Task** is a set of LLM guidelines describing the transformation to apply to a dataset.
A **TaskRun** is one execution of that task against one dataset — it is keyed by the
`(task_id, dataset_id)` pair, which is why almost every run route takes both.

## Steps

1. **Create the task.** `create_task_projects__project_id__tasks_post`
   (`POST /projects/{project_id}/tasks`). Its description names the inputs: `task_name`,
   a `task_settings` dictionary of configuration, and an optional `ref_task_id` to copy
   settings from an existing task. Inspect later with `get_task_tasks__task_id__get`, amend
   with `edit_task_tasks__task_id__post`.

2. **Start a run.** Two live routes, same job:
   - `update_task_run_v2_tasks__task_id__runs_post` (`POST /tasks/{task_id}/runs`) — the V2
     form; prefer it.
   - `update_task_run_tasks__task_id__runs__dataset_id__post`
     (`POST /tasks/{task_id}/runs/{dataset_id}`) — the V1 form, still live.
   `cancel_run` is a query parameter on the run routes — that is how you stop a run in
   flight.

3. **Track it.** `get_task_runs_tasks__task_id__runs_get` (`GET /tasks/{task_id}/runs`)
   returns, per its own docstring: `id`, `project_id`, `task_id`, `task_name`, `model_name`,
   `dataset_id`, `dataset_name`, `status`, `is_eval_run`, `created_at`, `updated_at`.
   For one run, `get_task_run_tasks__task_id__runs__dataset_id__get`
   (add `eval=true` to read the evaluation run instead).

   Polling is the only option — Refuel sends no events.

4. **Read quality.** `get_metrics_for_task_run_tasks__task_id__runs__dataset_id__metrics_get`
   for the run, `get_evalset_metrics_for_task_run_tasks__task_id__evalset_metrics_get` for
   the held-out evalset. Inspect individual labeled rows with
   `get_items_from_task_run_tasks__task_id__datasets__dataset_id__get`.

5. **Export.** Two paths:
   - Task-scoped: `export_task_tasks__task_id___tag__exports_post`
     (`POST /tasks/{task_id}/{tag}/exports`) where `tag` is `seedset` or `evalset`, then
     `download_export_task_tasks__task_id___tag__exports__export_id__get`.
   - Dataset-scoped: `export_dataset_datasets__dataset_id__exports_post`
     (`POST /datasets/{dataset_id}/exports`) with `task_id`, `task_run_id`,
     `include_labels`, `include_uuid`, `filters`, `email_address` — its description states it
     *"Exports dataset to S3 and emails the user a link to the S3 object"*. Then
     `download_dataset_datasets__dataset_id__exports__export_id__get`.

   Both return `202 Accepted`; poll the matching download route with the `export_id`.

6. **Re-run cleanly.** `clear_task_run_tasks__task_id__runs__dataset_id__delete` wipes a
   run's results. This is destructive and has no undo — confirm with a human first.

## Watch out

- No idempotency key: a retried "start run" starts a second run and bills for it.
- `200` with `success: false` is a failure. Check the flag, not the status code.
- The `{tag}` path segment only accepts `seedset` or `evalset` (the `TagType` enum).
