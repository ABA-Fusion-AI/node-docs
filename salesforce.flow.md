# Salesforce Flow Node

Node class: `SalesforceFlowNode`

## Operations

- `invoke`
- `getAll`

## Key Required Fields

- `invoke`: `flowApiName`
- `inputsJson` is optional; when omitted, object-shaped incoming data is used.

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "invoke",
  "flowApiName": "My_Flow_API_Name",
  "inputsJson": "{\"recordId\":\"001xx000003DHP0AAO\"}"
}
```
