# Evidence E01 — Role-assignment export structure

**Evidence type:** Sanitized description of the Azure Portal role-assignment export; **not** the raw CSV.

The accessible export contained role-assignment records at multiple scopes. Its structure was useful for comparing the role, assignee, scope, and optional metadata, but the full raw export included identifying and answer-bearing fields and is intentionally withheld.

| Field class | What an auditor checks | Public treatment |
| --- | --- | --- |
| Role name / role definition | Level and category of access | Discuss role categories only |
| Scope / inheritance | Whether access is direct or inherited | Generalize to management group, subscription, resource group, or resource |
| Principal details | Which identity received access | Omit names, email addresses, and object IDs |
| Assignment description / conditions | Notes or restrictions associated with a grant | **Exclude values entirely** |
| Eligibility / activation | Whether privilege is standing or time-bound | Verify separately in PIM |

**Observation supported:** Multiple assignments can affect the same resource group through different scopes; repeated assignments may reference one identity.  
**Not established:** A full directory membership inventory, whether every grant is inappropriate, or any actual compromise.

Raw CSVs, raw screenshots, and individual role-assignment rows are not published.

[Back to Week 03](../README.md)
