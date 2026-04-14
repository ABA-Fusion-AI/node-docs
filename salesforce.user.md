# Salesforce User Node

Node class: `SalesforceUserNode`

## Operations

- `get`
- `getAll`
- `getSummary`

## Key Required Fields

- `get`: `userId`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "getAll",
  "autoFetch": true,
  "maxFetch": 200
}
```
