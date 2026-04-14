# Salesforce Document Node

Node class: `SalesforceDocumentNode`

## Operations

- `create`
- `get`
- `getAll`
- `delete`
- `getSummary`

## Key Required Fields

- `create`: `createName`, `createFolderId`
- `get`/`delete`: `documentId`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createName": "Release Notes",
  "createFolderId": "00lxx000000XYZAA2",
  "body": "<base64-content>",
  "contentType": "text/plain"
}
```
