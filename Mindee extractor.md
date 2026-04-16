---
node_id: "mindee-document-extractor"
title: "Mindee Document Extractor"
description: "Extract beautifully clean data from any Mindee model (Invoices, Passports, Resumes, etc.)."
category: "docs"
subcategory: "mindee"
version: "1.0.0"
language: "en"
last_updated: "2026-04-16"
author: "Fusion Team"
tags:
  - mindee
  - ocr
  - ai
  - data-extraction
---

<!-- SECTION: header -->
# Mindee Document Extractor

> **Category:** AI & Machine Learning | **Type:** Action Node

Connect to the Mindee API to automatically extract structured data from various types of documents. You can process invoices, receipts, passports, resumes, and custom documents effortlessly by simply pointing the node to a local file.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Mindee Document Extractor** action node gives you programmatic access to Mindee's powerful OCR and document extraction models. It handles file reading natively and parses the complex Mindee API responses into a clean, standardized format.

### Key Features

- **Multiple Document Types:** Natively supports standard documents like Invoices, Receipts, and Passports, as well as Custom Documents configured on your Mindee dashboard.
- **Local File Processing:** Safely reads local files and directly streams them to the Mindee platform.
- **Automated Data Cleaning:** Automatically flattens and cleans Mindee's deeply nested output into highly usable key-value pairs out-of-the-box.

### Use Cases

- Automatically extract data from incoming supplier invoices and format it for an ERP system.
- Perform automated initial data entry by processing batch scans of passports or driver's licenses.
- Parse resumes sent via email and categorize candidates automatically based on key extracted fields.

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `apiKey` | `string` | ✅ Yes | — | Mindee V2 Global API Key |

### Common Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `documentType` | `enum` | ✅ Yes | `Invoice` | Select the type of document you are processing. Options: `Invoice`, `Receipt`, `Passport`, `Resume`, `Driver's License`, `International ID`, `Custom Document` |
| `modelId` | `string` | ✅ Yes | — | The specific Model ID for this document from your Mindee Dashboard |

### Message Construction

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `filePath` | `string` | ✅ Yes | Full path to the file on disk e.g. `/home/user/document.pdf`. Supports expressions. |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any` | Incoming workflow data (usable via expressions) |

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `output` | `object` | Result of the extraction containing the nicely formatted `extracted_data` dataset |
| `error` | `Error` | Emitted if file loading fails or Mindee API authentication throws an error |

### Output Schemas

**`Extract Data`**
```json
{
  "operation": "Extract Invoice",
  "success": true,
  "extracted_data": {
    "total_amount": 120.50,
    "invoice_date": "2024-05-12",
    "supplier_name": "Acme Corp"
  }
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Extracting data from an Invoice

Process a local PDF invoice file using a specific model ID to capture structured accounting metrics.

**Configuration:**
```json
{
  "apiKey": "your_mindee_api_key",
  "documentType": "Invoice",
  "modelId": "mindee/invoices_v4",
  "filePath": "/home/user/invoices/INV-2023-01.pdf"
}
```

**Output:**
```json
{
  "operation": "Extract Invoice",
  "success": true,
  "extracted_data": {
    "invoice_number": "INV-2023-01",
    "total_amount": 1450.00,
    "tax_amount": 150.00,
    "supplier": "Tech Supplies Inc."
  }
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### `Invalid credentials` error

**Cause:** The provided Mindee API Key is missing or rejected by the API.

**Solution:** Verify your `apiKey` in your Mindee API portal.

#### `File not found at path` error

**Cause:** The specified `filePath` doesn't exist on the filesystem where the process operates.

**Solution:** Ensure the local file path is absolute, accurate, and your Fusion application has proper read permissions.

#### `Model ID is required` error

**Cause:** The `modelId` value was omitted but is required to connect to the prediction instance.

**Solution:** Provide your model's ID verbatim as formatted inside the designated app project dashboard.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-16 | Initial release |

<!-- /SECTION: changelog -->