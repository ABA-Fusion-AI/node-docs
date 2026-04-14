# Salesforce Einstein Node

Node class: `SalesforceEinstienNode`

## Authentication

Einstein uses these parameters:

- `accessToken` (required)
- `baseUrl` (optional, default `https://api.einstein.ai/v2`)

## Operations

### API Usage

- `getApiUsage`

### Language

- `getLanguageDatasets`
- `predictSentiment`
- `predictIntent`
- `trainLanguageModel`
- `getLanguageTrainingStatus`

### Vision

- `getVisionDatasets`
- `predictImage`
- `detectObjects`
- `ocrImage`
- `trainVisionModel`
- `getVisionTrainingStatus`

## Example

```json
{
  "accessToken": "{{secrets.salesforceEinsteinToken}}",
  "operation": "predictSentiment",
  "sentimentData": {
    "document": "This experience was excellent.",
    "modelId": "CommunitySentiment"
  }
}
```
