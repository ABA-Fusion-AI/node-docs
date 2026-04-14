# Salesforce Task Node

Node class: `SalesforceTaskNode`

## Operations

- `create`
- `update`
- `get`
- `getAll`
- `delete`
- `getSummary`

## Key Required Fields

- `create`: `createSubject`
- `update`: `taskId` and at least one updatable field
- `get`/`delete`: `taskId`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createSubject": "Follow up call",
  "status": "Not Started",
  "priority": "Normal"
}
```
