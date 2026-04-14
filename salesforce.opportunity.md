# Salesforce Opportunity Node

Node class: `SalesforceOpportunityNode`

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

- `create`: `createName`, `createStageName`, `createCloseDate`
- `update`: `opportunityId` and at least one updatable field
- `upsert`: `externalIdField`, `externalIdValue`
- `get`/`delete`: `opportunityId`
- `addNote`: `opportunityId`, `noteTitle`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createName": "Q3 Renewal",
  "createStageName": "Prospecting",
  "createCloseDate": "2026-07-15",
  "amount": 25000
}
```
