---
name: Turn a maintenance issue into a completed work order in Arthur
description: Raise a task against a property, unit or tenancy in Arthur Online, dispatch it to a contractor as a work order, handle quotes and invoices, and close it out.
api: openapi/arthur-online-maintenance-openapi.yml
also_uses:
  - openapi/arthur-online-properties-openapi.yml
  - openapi/arthur-online-tenancies-openapi.yml
  - openapi/arthur-online-conversations-openapi.yml
operations:
  - listTasks
  - listTasksByStatus
  - addTaskOnProperty
  - addTaskOnTenancy
  - viewTask
  - updateTask
  - updateStatusOnTask
  - createSubtaskOnTask
  - createWorkorderOnTask
  - viewWorkorder
  - updateWorkorder
  - listQuotesOnWorkorder
  - viewQuoteOnWorkorder
  - listInvoicesOnWorkorder
  - listContractorsOnWorkorder
  - createAssetOnWorkorder
  - createConversationOnWorkorder
  - addNoteOnWorkorder
generated: '2026-07-26'
method: generated
source: openapi/ derived from https://developer.arthuronline.co.uk/
---

# Maintenance issue to completed work order

Maintenance is the largest surface in the Arthur API — 66 of the 317 operations. The spine is
Task → Work order → Quote → Contractor invoice, with conversations, notes and photos hanging off
each step.

## Before you start

- Every call: `Authorization: Bearer <token>` + `X-EntityID: <entity_id>`.
- Resolve `task_type`, `task_status` and subtask templates against the Types API
  (`taskTypes`, `taskStatuses`, `subtaskTemplates`) before writing.
- `GET /tasks/{status}` (`listTasksByStatus`) shares its shape with `GET /tasks/{task_id}`
  (`viewTask`). Arthur disambiguates by value; a generated client will not. Call them explicitly.

## Steps

1. **Find or raise the task.** `listTasks` (or `listTasksByStatus` for a queue view) to check
   whether the issue is already open. To raise one, `addTaskOnProperty`, `addTaskOnUnit` or
   `addTaskOnTenancy` depending on what the issue belongs to. Keep `data.id` as `task_id`.
2. **Break it down if needed.** `createSubtaskOnTask` for multi-part jobs; read them back with
   `listSubtasksOnTask`.
3. **Attach evidence.** `createAssetOnWorkorder` (or `createAssetOnProperty` before dispatch) with
   `file` base64, `mime_type` and `file_name`, and set the share flags so the contractor and the
   tenant can see the photos.
4. **Move the task.** `updateStatusOnTask` for pending → live → in progress → completed. Thirteen
   Task webhook triggers mirror this lifecycle — subscribe instead of polling.
5. **Dispatch to a contractor.** `createWorkorderOnTask` creates the work order from the task.
   Keep `data.id` as `workorder_id`. Confirm the assignment with `listContractorsOnWorkorder`.
6. **Handle quotes.** `listQuotesOnWorkorder` / `viewQuoteOnWorkorder` read what the contractor
   submitted. Quote acceptance by the manager or the owner happens in the Arthur apps — the API
   exposes no accept operation; watch the `WorkorderQuote` webhook triggers for the outcome.
7. **Keep everyone talking.** `createConversationOnWorkorder` (after `getRecipientsForWorkorder`
   to learn who can be addressed) and `addNoteOnWorkorder` for the internal record.
8. **Close out.** `updateWorkorder` for the final state, and `listInvoicesOnWorkorder` /
   `viewInvoiceOnWorkorder` to reconcile what the contractor billed. `listTransactionsOnWorkorder`
   ties the job to the ledger.

## Rules

- No idempotency: a retried `createWorkorderOnTask` dispatches the same job twice, to a real
  contractor. Read back with `listWorkordersOnTask` before any retry.
- Send `strict=true` on writes so an unknown task type or contractor label fails loudly instead of
  creating new reference data.
- Approval steps (property manager approves completed work order, owner accepts or rejects a quote)
  are UI-only. Model them as inbound webhook events, not as calls you can make.
- Rate budget is 5,000 requests per hour for the whole integration; a polling loop over `listTasks`
  will exhaust it long before the webhook surface would.
