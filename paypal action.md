---
node_id: "paypal-action"
title: "PayPal Action"
description: "Perform PayPal Payouts operations: create batch payouts, view batch or item details, cancel unclaimed items"
category: "finance"
subcategory: "payouts"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - paypal
  - payouts
  - payments
  - finance
  - action
related_nodes:
  - stripe-action
  - function
---

<!-- SECTION: header -->
# PayPal Action

> **Category:** Finance | **Type:** Action Node

Connect to PayPal and execute payout operations — create batch payouts, view batch or item details, and cancel unclaimed items.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **PayPal Action** node establishes a connection to the PayPal API to perform Payout operations using your PayPal Business account credentials. It supports handling multiple payout items in a batch, viewing the status of a batch or individual payout items, and canceling any items that remain unclaimed.

### Key Features

- **Four Operations:** Create Batch Payout, Get Batch Payout, Get Payout Item, Cancel Payout Item
- **Batch Payouts:** Send multiple payments in a single batch directly to email addresses, phone numbers, or PayPal IDs
- **Detailed Status:** Retrieve granular details for a specific payout batch or item
- **Security:** Requires OAuth `clientId` and `clientSecret` from a PayPal Developer portal
- **Sandboxing:** Native support for toggling between `sandbox` and `live` environments
- **HATEOAS Link Filtering:** Automatically cleans HATEOAS references from outputs to provide clean, manageable objects.

### Use Cases

- Pay multiple freelancers or vendors at designated times
- Trigger mass rebate distributions or promotional cashback processing
- View the detailed status of a specific batch payout
- Reclaim funds by canceling unclaimed payments automatically

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `clientId` | `string` | ✅ Yes | — | PayPal Developer Client ID |
| `clientSecret` | `string` | ✅ Yes | — | PayPal Developer Client Secret |
| `environment` | `enum` | ❌ No | `sandbox` | API environment: `sandbox` or `live` |

### Common Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `resource` | `enum` | ❌ No | `payout` | Resource category (`payout` or `payoutItem`) |
| `operation` | `enum` | ❌ No | `createBatchPayout` | Operation to perform (see below) |

### Available Operations

| Operation | Resource | Description |
|-----------|----------|-------------|
| `createBatchPayout` | `payout` | Create a new batch of payout items |
| `getBatchPayout` | `payout` | Retrieve details for an existing batch payout |
| `getPayoutItem` | `payoutItem`| Retrieve details for a specific payout item |
| `cancelPayoutItem` | `payoutItem`| Cancel a specific unclaimed payout item |

---

### Operation: createBatchPayout

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `senderBatchId` | `string` | ✅ Yes | A unique ID to track this batch (e.g. 'batch-2024-001') |
| `emailSubject` | `string` | ❌ No | Subject line for the email sent to recipients |
| `emailMessage` | `string` | ❌ No | Message body for the email sent to recipients |
| `itemsInputMode` | `enum` | ❌ No | Defaults to `form`, used to construct the items payload |
| `batchCurrency` | `enum` | ❌ No | Currency for the batch (`USD` or `EUR`, defaults to `USD`) |
| `itemsForm` | `array` | ✅ Yes | List of payouts: `[{recipient_type, receiver, value, note, sender_item_id}]` |

### Operation: getBatchPayout

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `payoutBatchId` | `string` | ✅ Yes | The PayPal-generated batch ID |

### Operation: getPayoutItem

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `payoutItemId` | `string` | ✅ Yes | The PayPal-generated item ID |

### Operation: cancelPayoutItem

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `payoutItemId` | `string` | ✅ Yes | The PayPal-generated item ID (must be unclaimed to cancel) |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any` | Incoming workflow data (usable via expressions in fields) |

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `output` | `object` | Result of the executed API operation |
| `error` | `Error` | Emitted if the operation fails |

### Output Schemas by Operation

**`createBatchPayout`**
```json
{
  "batch_header": {
    "payout_batch_id": "1234ABCD",
    "batch_status": "PENDING",
    "sender_batch_header": {
      "sender_batch_id": "batch-2024-001",
      "email_subject": "You have a payout!"
    }
  },
  "items": [
    {
      "recipient_type": "EMAIL",
      "receiver": "user@example.com",
      "amount": { "value": "10.00", "currency": "USD" },
      "note": "Thanks for your service"
    }
  ]
}
```

**`getBatchPayout`**
```json
{
  "batch_header": {
    "payout_batch_id": "1234ABCD",
    "batch_status": "SUCCESS",
    "sender_batch_header": {
      "sender_batch_id": "batch-2024-001"
    }
  },
  "items": [
    {
      "payout_item_id": "ITEM9876",
      "transaction_status": "SUCCESS",
      "payout_item_amount_value": "10.00",
      "payout_item_amount_currency": "USD"
    }
  ]
}
```

**`getPayoutItem`**
```json
{
  "payout_item_id": "ITEM9876",
  "transaction_id": "TRANS123",
  "transaction_status": "SUCCESS",
  "payout_batch_id": "1234ABCD",
  "payout_item_amount_value": "10.00",
  "payout_item_amount_currency": "USD"
}
```

**`cancelPayoutItem`**
```json
{
  "payout_item_id": "ITEM9876",
  "transaction_status": "RETURNED",
  "payout_batch_id": "1234ABCD"
}
```
*(If already cancelled, returns item details with a `_meta` key denoting prior cancellation)*

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Create a Batch Payout

Send payouts to two different email addresses.

**Configuration:**
```json
{
  "clientId": "your_client_id",
  "clientSecret": "your_client_secret",
  "environment": "sandbox",
  "resource": "payout",
  "operation": "createBatchPayout",
  "senderBatchId": "monthly-payout-{{input.month}}",
  "emailSubject": "Your monthly payment from Acme Corp",
  "batchCurrency": "USD",
  "itemsForm": [
    {
      "recipient_type": "EMAIL",
      "receiver": "contractor1@example.com",
      "value": "150.00",
      "note": "Payment for invoice #1"
    },
    {
      "recipient_type": "EMAIL",
      "receiver": "contractor2@example.com",
      "value": "200.00",
      "note": "Payment for invoice #2"
    }
  ]
}
```

---

### Example: Check Batch Status

Retrieve the full status of a pending batch.

**Configuration:**
```json
{
  "clientId": "your_client_id",
  "clientSecret": "your_client_secret",
  "environment": "sandbox",
  "resource": "payout",
  "operation": "getBatchPayout",
  "payoutBatchId": "1234ABCD"
}
```

---

### Example: Cancel Unclaimed Payout

Reclaim funds for an unclaimed payout.

**Configuration:**
```json
{
  "clientId": "your_client_id",
  "clientSecret": "your_client_secret",
  "environment": "sandbox",
  "resource": "payoutItem",
  "operation": "cancelPayoutItem",
  "payoutItemId": "ITEM12345"
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: workflow-example -->
## Workflow Integration

### Sample Workflow: Contractor Payout Process

Trigger mass payouts using data fetched from a database and alert Slack when done.

```json
{
  "nodes": [
    {
      "id": "fetch-contractors",
      "type": "mysql-action",
      "position": { "x": 100, "y": 100 },
      "config": {
        "operation": "Select",
        "tableName": "contractor_invoices",
        "selectParams": {
            "where": "status = 'approved'"
        }
      }
    },
    {
      "id": "format-payouts",
      "type": "function",
      "position": { "x": 300, "y": 100 },
      "config": {
        "code": "return { items: input.rows.map(row => ({ recipient_type: 'EMAIL', receiver: row.email, value: row.amount_due, note: row.invoice_id })) };"
      }
    },
    {
      "id": "send-payouts",
      "type": "paypal-action",
      "position": { "x": 500, "y": 100 },
      "config": {
        "clientId": "secret_id",
        "clientSecret": "secret_key",
        "environment": "live",
        "operation": "createBatchPayout",
        "senderBatchId": "batch_{{new Date().getTime()}}",
        "itemsForm": "{{input.items}}"
      }
    }
  ]
}
```

<!-- /SECTION: workflow-example -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### Invalid Credentials

**Cause:** The `clientId` or `clientSecret` is missing, blank, or invalid.

**Solution:** Double-check your app credentials in the PayPal Developer Dashboard. Ensure you are using the correct credentials for the selected `environment`.

#### Items missing or malformed

**Cause:** `itemsForm` is empty or incorrectly formatted.

**Solution:** Ensure you are providing an array of payout items (e.g., via a Function node) or have configured at least one form entry containing a `receiver` and `value`.

#### Currency Mismatch

**Cause:** PayPal requires all payouts in a single batch to use the same currency.

**Solution:** Ensure `batchCurrency` is explicitly set to match what your recipients should receive, and avoid mixing currencies in the same `createBatchPayout` call.

### Error Codes

| Code | Message | Solution |
|------|---------|----------|
| `INVALID_CREDENTIALS` | Invalid client credentials | Verify developer ID/Secret |
| `ITEM_ALREADY_CANCELLED` | Item already cancelled | Handled gracefully. Node will return item status and proceed. |
| `INVALID_CURRENCY` | Invalid currency for item | Ensure all items match the batch currency limit |
| `AUTHORIZATION_ERROR` | Need payout privileges | Payouts API requires specific enablement on your PayPal account. |

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->