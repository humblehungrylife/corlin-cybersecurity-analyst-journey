# Week 03 — Azure CLI and PowerShell role-assignment walkthrough

This reference explains a **read-only** access-review pipeline. Names, scopes, and patterns are neutralized to protect the Mad Hat Labs exercise. It is a transferable method, **not** a solution or an answer key.

## Full sanitized example

```powershell
$rg = "<authorized-resource-group>"

az role assignment list --resource-group $rg --include-inherited --output json |
ConvertFrom-Json |
Where-Object {
    $_.roleDefinitionName -in @(
        "Owner",
        "Contributor",
        "User Access Administrator",
        "Role Based Access Control Administrator"
    ) -or
    $_.description -match 'AUDIT\{[^}]+\}'
} |
Select-Object roleDefinitionName, principalName, principalType, principalId, scope, description |
Format-List |
Out-File -FilePath "privileged-access-review-private.txt" -Width 4096
```

`AUDIT{...}` is a **generic example pattern** to illustrate regular expressions. It is not a training flag, and the actual description values must not be published.

## Step-by-step

| Part | Explanation |
| --- | --- |
| `$rg = "…"` | Stores an authorized resource-group name in a reusable PowerShell variable. |
| `az role assignment list` | Retrieves visible Azure RBAC role assignments at that resource-group scope. `--include-inherited` also requests ancestor-scope assignments. |
| `--output json` | Returns JavaScript Object Notation (JSON) records. |
| `ConvertFrom-Json` | Converts JSON into PowerShell objects with accessible properties. |
| `Where-Object` | Keeps records that match either the privileged-role list **or** the example description pattern. |
| `Select-Object` | Keeps six investigation fields; it does not change Azure permissions. |
| `Format-List` | Displays the selected properties on separate lines for readability. |
| `Out-File -Width 4096` | Writes a private text report with a wide output width to reduce truncation. |

### The role filter

`$_` refers to the current pipeline object. `-in @(...)` checks whether `roleDefinitionName` is among the named roles. The `-or` operator includes the object if **either** that test or the description test matches.

### The regular expression

| Pattern part | Meaning |
| --- | --- |
| `AUDIT` | Literal demonstration text. |
| `\{` | An opening curly brace. |
| `[^}]` | A character other than a closing brace. |
| `+` | One or more such characters. |
| `\}` | A closing curly brace. |

This filter may include an assignment with a lower-privilege role if its description matches. That is **not itself a security finding**.

## What each selected field means

| Field | Audit question |
| --- | --- |
| `roleDefinitionName` | Which RBAC role was assigned? |
| `principalName` | Which identity name was resolved, if available? |
| `principalType` | Is the principal a user, group, or service principal? |
| `principalId` | Which unique identity is recorded? **Do not publish actual IDs.** |
| `scope` | At which Azure scope was the assignment granted? |
| `description` | What optional assignment metadata was recorded? **Do not publish raw values.** |

## Limitations

This pipeline does not reveal each role's `Actions`, `NotActions`, `DataActions`, or `NotDataActions`, enumerate all group members, or capture inactive PIM eligibility. It does not automatically inventory role assignments made directly to all child resources. A blank `principalName` is insufficient to prove deletion, and a zero-result query cannot establish absence outside the account's visibility.

The output is for **private evidence handling**. Publish the sanitized method, not the raw report.

[Back to Week 03](../README.md)
