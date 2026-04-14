# Salesforce Account Node

Node class: `SalesforceAccountNode`

## Operations

- `create`
- `update`
- `upsert`
- `get`
- `getAll`
- `delete`
- `addNote`
- `getSummary`

## Key Required Fields

- `create`: `createName`
- `update`: `accountId` and at least one updatable field
- `upsert`: `externalIdField`, `externalIdValue`
- `get`/`delete`: `accountId`
- `addNote`: `accountId`, `noteTitle`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createName": "Acme Corp",
  "industry": "Technology"
}
```
