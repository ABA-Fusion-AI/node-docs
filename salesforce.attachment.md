# Salesforce Attachment Node

Node class: `SalesforceAttachmentNode`

## Operations

- `create`
- `update`
- `get`
- `getAll`
- `delete`
- `getSummary`

## Key Required Fields

- `create`: `createParentId`, `createName`
- `update`: `attachmentId` and at least one updatable field
- `get`/`delete`: `attachmentId`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createParentId": "001xx000003DHP0AAO",
  "createName": "invoice.pdf",
  "body": "<base64-content>",
  "contentType": "application/pdf"
}
```
