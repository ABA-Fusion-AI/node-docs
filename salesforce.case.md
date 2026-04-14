# Salesforce Case Node

Node class: `SalesforceCaseNode`

## Operations

- `create`
- `update`
- `get`
- `getAll`
- `delete`
- `addComment`
- `getSummary`

## Key Required Fields

- `create`: `createSubject`
- `update`: `caseId` and at least one updatable field
- `get`/`delete`: `caseId`
- `addComment`: `caseId`, `commentBody`

## Example

```json
{
  "authType": "usernamePassword",
  "username": "integration.user@company.com",
  "password": "{{secrets.salesforcePassword}}",
  "securityToken": "{{secrets.salesforceSecurityToken}}",
  "operation": "addComment",
  "caseId": "500xx000001ABCdAAO",
  "commentBody": "Customer contacted and issue reproduced.",
  "commentIsPublished": true
}
```
