# Week 03 — The Privilege Audit

**Mad Hat Labs | Chapter 3.5: Azure role-based access control (RBAC) and privileged access**

**Status:** Completed training investigation; independently written methodology report.  
**Scenario role:** Resource-access auditor.  
**Actual access:** Restricted Reader-level visibility, followed by authorized, time-limited Privileged Identity Management (PIM) activation for an assigned resource group.  
**Environment:** Live multi-user Azure **training** tenant. **Not production experience.**

## Scenario and scope

Following a simulated governance concern, I reviewed Azure **resource** permissions to understand **who can do what, where, and when**. Unlike my Week 2 incident reconstruction, this was an **access-audit methodology exercise**: I compared four ways to inspect permissions, recorded where their results disagreed, and assessed possible least-privilege issues. Microsoft Entra directory-administrator roles were outside the audit's scope.

I used the Azure Portal's Access control (Identity and Access Management, IAM) view, Azure Command-Line Interface (CLI) with PowerShell, Azure Resource Graph with Kusto Query Language (KQL), and PIM role views. I made no resource configuration or permission changes. My PIM activation was an authorized, temporary access step.

## Methodology comparison

| Method | What it showed or helped me test | Important blind spot |
| --- | --- | --- |
| **IAM blade / export** | Readable assignments, role names, scopes, and inheritance; a useful baseline. | The broad subscription export was restricted. Group assignments do not reveal all members, and identity details may be missing. |
| **Azure CLI / PowerShell** | Scriptable, repeatable enumeration and structured assignment fields that could be exported as Comma-Separated Values (CSV). | Visibility depends on the account and queried scope. A blank `principalName` is not proof of a deleted user. |
| **Resource Graph / KQL** | Cross-resource filtering of assignments within accessible subscriptions and scopes. | A zero-result query can reflect access limits or filters; it does not inventory inactive eligible PIM access. |
| **PIM Azure Resources** | Eligible and Active assignment states and time-bound role activation. | My screenshots are not a complete resource-audit history or a tenant-wide PIM export. |

## Five-stage investigation

### 1. IAM blade and role export

I started with the portal's role-assignment view. The subscription-level download was restricted, so I used the course-provided baseline for comparison and later obtained a role-assignment export from an accessible resource-group scope. I examined role scope, inheritance, and repeated high-privilege grants without assuming that several assignments meant several different people.

**Limitation:** The accessible view did not establish membership of privileged groups or a complete subscription-wide inventory.

### 2. Azure CLI enumeration

I used Azure Cloud Shell PowerShell to list assignments and export a readable CSV. A lab-supplied historical extract and my live scoped query differed: one assignment shown in the earlier extract was not returned by my focused query. Changing principal-name resolution did not explain the discrepancy. I did **not** conclude that a principal or assignment had been deleted.

A safe example of the scoped command is:

```powershell
$rg = "<authorized-resource-group>"
az role assignment list --resource-group $rg --include-inherited --output json |
    ConvertFrom-Json |
    Select-Object roleDefinitionName, principalType, scope |
    Format-Table -AutoSize
```

The more detailed [PowerShell filtering and export walkthrough](methods/cli-powershell.md) explains the pipeline, operators, and evidence limits without publishing answer-bearing values.

### 3. Azure Resource Graph / KQL

I examined the `authorizationresources` table to test a query-driven approach. Filtering can narrow a large role-assignment dataset, but results remain bounded by access and query scope.

```kql
authorizationresources
| where type =~ 'microsoft.authorization/roleassignments'
| extend principalType = tostring(properties.principalType)
| summarize assignmentCount = count() by principalType
```

**Limitation:** No matching rows within my visible scope did not establish that no assignments existed elsewhere. See the [sanitized query notes](evidence/kql-method.md).

### 4. PIM access review

I reviewed Azure-resource role eligibility and activation, including a request that required a reason and specified a duration. The later Active view showed temporary access. I separately examined PIM-managed group membership: activating **group membership** and activating an **Azure resource role** are different operations, and neither automatically grants Owner privileges.

**Limitation:** The observed screens demonstrate eligibility and activation, not every approval control, every privileged assignment, or a complete historical activation log.

### 5. Independent privilege hunt

After activating the authorized resource role, I compared CLI results and a scope-specific portal export. I observed an Owner assignment at the reviewed resource-group scope alongside higher-level Owner grants that could be inherited. Some repeated assignments referenced the same identity. A portal table initially showed no results despite data being returned through other methods, which made cross-checking essential.

**Limitation:** The role's business need, actual misuse, group-member identities, and complete blast radius were not independently established.

## Findings and recommendations

| Review priority* | Finding class | Recommendation |
| --- | --- | --- |
| **High** | Broad Owner access was visible through direct and higher-scope assignments. | Validate the task and business owner; replace unnecessary Owner grants with narrower job-function roles and scopes. |
| **Medium** | Overlapping Owner assignments involved some of the same identities. | Check inherited effective access and remove redundant grants after owner approval. |
| **Needs verification** | Historical and live results differed for a potentially stale identity assignment. | Confirm principal and assignment status using authorized directory and audit records before treating it as orphaned. |
| **Control improvement** | Permanent privileged access may increase exposure where standing access is unnecessary. | Consider PIM eligibility, Multi-Factor Authentication (MFA), justification, approval, and short activation windows where appropriate. |

*These are **review priorities**, not confirmed policy breaches or measured risk ratings. I did not verify exploitation or implement remediation.*

For repeatability, I would run quarterly reviews, examine group membership alongside direct grants, and turn confirmed excess permissions into tracked remediation tickets.

## What challenged me / what I learned

I initially expected portal exports and CLI queries to show identical results. Instead, access level, scope, identity-name resolution, and the collection date mattered. Another important distinction was that **multiple Owner assignments do not necessarily mean multiple Owner identities**.

The practical lesson: assess permissions by **role, scope, and duration**, then separate what I observed from what the available evidence cannot prove.

## Evidence and methodology references

These are **sanitized, text-based references**, not raw tenant exports or unredacted screenshots. Detailed private screenshots remain outside this public repository.

- [E01 — Role-assignment export structure and redaction notes](evidence/role-assignment-schema.md)
- [E02 — Azure CLI / PowerShell command walkthrough](methods/cli-powershell.md)
- [E03 — Azure Resource Graph / KQL methodology](evidence/kql-method.md)
- [E04 — PIM eligibility and activation observations](evidence/pim-review.md)

## Training acknowledgment and disclosure

Thank you to **Mad Hat Labs** for the hands-on Azure training environment. This is my independent learner-authored audit report, not an official course publication, a production audit, or proof of a real-world security incident. Training answers, identities, principal identifiers, private resource names, description values, and raw exports are intentionally excluded. See the [publication policy](../../PUBLICATION-POLICY.md).

**Experience classification: Authorized access-audit exercise in a live multi-user Azure training tenant. Not production employment.**
