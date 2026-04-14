# Salesforce Search Node

Node class: `SalesforceSearchNode`

## Operations

- `soql`
- `sosl`
- `parameterized`

## Key Required Fields

- `soql`: `soqlQuery`
- `sosl`: `soslQuery`
- `parameterized`: `searchTerm`

## Examples

### SOQL

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "soql",
  "soqlQuery": "SELECT Id, Name FROM Account LIMIT 50"
}
```

### SOSL

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "sosl",
  "soslQuery": "FIND {Acme} RETURNING Account(Id, Name), Contact(Id, Name)"
}
```
