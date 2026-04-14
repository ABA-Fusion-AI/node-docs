# Salesforce Lead Node

Node class: `SalesforceLeadNode`

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

- `create`: `createLastName`, `createCompany`
- `update`: `leadId` and at least one updatable field
- `upsert`: `externalIdField`, `externalIdValue`
- `get`/`delete`: `leadId`
- `addNote`: `leadId`, `noteTitle`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createLastName": "Smith",
  "createCompany": "Globex",
  "email": "s.smith@globex.com",
  "status": "Open - Not Contacted"
}
```
