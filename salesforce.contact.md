# Salesforce Contact Node

Node class: `SalesforceContactNode`

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

- `create`: `createLastName`
- `update`: `contactId` and at least one updatable field
- `upsert`: `externalIdField`, `externalIdValue`
- `get`/`delete`: `contactId`
- `addNote`: `contactId`, `noteTitle`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "upsert",
  "externalIdField": "External_Id__c",
  "externalIdValue": "contact-123",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com"
}
```
