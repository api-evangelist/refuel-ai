---
name: refuel-finetune-model
description: >-
  Improve task quality with feedback, then finetune a Refuel LLM-2 model on the labeled
  data and track the resulting model.
api: openapi/refuel-ai-cloud-api-openapi.yml
base_url: https://cloud-api.refuel.ai
generated: '2026-08-14'
method: generated
source: openapi/refuel-ai-cloud-api-openapi.yml + https://docs.refuel.ai/guides/models/finetuning
operations:
  - add_item_to_seedset_tasks__task_id__seedset_items_post
  - get_items_from_seedset_tasks__task_id__seedset_get
  - create_evalset_tasks__task_id__evalset_post
  - add_item_to_evalset_tasks__task_id__evalset_items_post
  - edit_labels_v2_tasks__task_id__datasets__dataset_id__items__item_id__label_post
  - trainable_task_tasks__task_id__trainable_get
  - get_models_models_get
  - get_task_models_tasks__task_id__models_get
  - finetune_model_for_task_projects__project_id__finetuned_models_post
  - get_finetuned_models_for_project_task_projects__project_id__finetuned_models_get
  - get_finetuned_model_from_id_finetuned_models__model_id__get
  - update_finetuned_model_finetuned_models__model_id__patch
  - delete_finetuned_model_from_id_finetuned_models__model_id__delete
  - calibrate_task_tasks__task_id__calibrations_post
  - get_calibration_models_tasks__task_id__calibrations_get
---

# Finetune a Refuel model on your labeled data

Refuel's quality loop is: label with a base model, correct the labels, use the corrections
as training data, finetune, then calibrate confidence.

## Steps

1. **Build the seedset.** The seedset is the human-labeled set that steers the task.
   `add_item_to_seedset_tasks__task_id__seedset_items_post`
   (`POST /tasks/{task_id}/seedset/items`); read it back with
   `get_items_from_seedset_tasks__task_id__seedset_get`. Correct a model output in place
   with `edit_labels_v2_tasks__task_id__datasets__dataset_id__items__item_id__label_post`.

2. **Hold out an evalset.** `create_evalset_tasks__task_id__evalset_post`
   (`POST /tasks/{task_id}/evalset`), then
   `add_item_to_evalset_tasks__task_id__evalset_items_post`. Keep it separate from the
   seedset — it is what the finetuned model is measured against.

3. **Check the task is trainable.** `trainable_task_tasks__task_id__trainable_get`
   (`GET /tasks/{task_id}/trainable`) returns a `TrainableResponse`. Do not attempt a
   finetune until this says yes; the alternative is a rejected job you still waited on.

4. **Pick a base model.** `get_models_models_get` (`GET /models`) lists what is available;
   `get_task_models_tasks__task_id__models_get` lists what is attached to the task. The
   trainable base models declared in the contract (`TrainableBaseModels` enum) are:
   `refuel-llm-2-large`, `refuel-llm-2-small`, `refuel-llm-2-mini`.

5. **Start the finetune.** `finetune_model_for_task_projects__project_id__finetuned_models_post`
   (`POST /projects/{project_id}/finetuned_models`) with a `TrainModelRequest`:
   `project_id`, `task_id`, `base_model`, `max_training_rows`, `lora`,
   `augmented_finetuning_model`, `hyperparameters`, `datasets`, `comparison_model`.
   Its description states the training data needs, per row: *the task input columns, the
   task instructions, and the label.*

6. **Track it.** `get_finetuned_models_for_project_task_projects__project_id__finetuned_models_get`
   (filter by `task_id`), then
   `get_finetuned_model_from_id_finetuned_models__model_id__get`. Rename or retag with
   `update_finetuned_model_finetuned_models__model_id__patch`; remove with
   `delete_finetuned_model_from_id_finetuned_models__model_id__delete`.

7. **Calibrate confidence.** `calibrate_task_tasks__task_id__calibrations_post`
   (`POST /tasks/{task_id}/calibrations`) fits a confidence-calibration model so you can
   auto-accept high-confidence rows and route the rest to a human. Watch
   `get_calibration_models_tasks__task_id__calibrations_get` — `CalibrationStatus` is one of
   `IN_PROGRESS`, `COMPLETED`, `INTERRUPTED`, `DELETED`. A calibration conflict returns
   `409`.

## Watch out

- Finetuning is long-running, asynchronous and expensive. It returns immediately; poll.
  There is no idempotency key, so a retried start is a second training job with a second bill.
  **Confirm with a human before starting or deleting a finetune.**
- `delete_finetuned_model_from_id_finetuned_models__model_id__delete` is irreversible.
- Refuel LLM-2 is also served on Together AI's platform following the 2025 acquisition —
  see `lifecycle/refuel-ai-lifecycle.yml`.
