# Evidence E03 — Resource Graph / KQL review

This is a **sanitized, read-only** Kusto Query Language (KQL) example for exploring role assignments available through Azure Resource Graph.

```kql
authorizationresources
| where type =~ 'microsoft.authorization/roleassignments'
| extend principalType = tostring(properties.principalType)
| summarize assignmentCount = count() by principalType
| order by assignmentCount desc
```

The `authorizationresources` table can help an auditor group assignments by principal type or investigate patterns across the subscriptions and scopes they can query.

**Visibility caveat:** Results depend on the account's permissions, scope, and query filters. No returned matches does not prove an assignment is absent from the tenant. Eligible but inactive PIM role schedules require separate review.

This example omits private principal IDs, descriptions, and training answers.

[Back to Week 03](../README.md)
