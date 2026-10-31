# Week 02 — Investigating an Identity Attack in Microsoft Entra ID

**Mad Hat Labs | Chapter 2: Entra ID**

**Status:** Training investigation completed; public case study based on learner observations and retrospective documentation.  
**Scenario role:** Azure identity investigator in the exercise narrative.  
**Actual access:** Reader (directory applications).  
**Environment:** Live multi-user Azure training tenant. **Not production experience.**

## Scenario

The Mad Hat Labs security team identified suspicious modifications to a legacy application following an employee identity compromise. The investigation sought to explain how a compromised employee session could lead to privileged application access, persistence, and a potential further identity compromise. With Reader access, I reviewed the application configurations to reconstruct the likely attack chain while distinguishing the scenario narrative from what the available evidence actually proves.

## Environment and scope

| Component | Description |
| --- | --- |
| Platform | Microsoft Azure / Microsoft Entra ID |
| Setting | Live multi-user Azure training tenant through Mad Hat Labs |
| Access | Reader within the assigned directory application scope |
| Primary tool | Azure Portal |
| Review areas | Application settings, credentials, ownership relationships, API permissions, exposed scopes, and redirect configurations |
| Activity limits | Observation and documentation; no configuration changes or remediation |

The scenario's attacker activity was not performed by me. The screenshots capture application configuration views, not an independently confirmed incident timeline.

## Attack chain overview

**Initial access → Application privilege abuse → Secondary application pivot → Persistence → Potential token acquisition**

The scenario began when Carl, an Accounting employee, encountered a counterfeit Microsoft sign-in page. The lab narrative states that the attacker obtained an authenticated session token after Multi-Factor Authentication (MFA) was satisfied. The real escalation risk was not a general administrator role assigned to Carl, but his unnecessary ownership of a legacy application with privileged directory permissions.

The attacker reportedly introduced an application secret, created a second registration named `Mad-Hat-Labs-App`, and established an additional ownership path to the original application. Later changes to API authorization and redirect configurations suggested an OAuth (Open Authorization) consent-phishing method to seek access on behalf of additional users. These stages show how a single human compromise can expose a larger application authorization chain.

## Investigation

### Stage 1 — Initial access and ownership context

**Examined:** I reviewed the legacy application's identifying properties to confirm I was examining the affected application.

**Found:** The lab narrative connected the initial access to Carl, whose account had ownership of the legacy application. The screenshot documents the application view; it is not a phishing log or session-token record.

**Concluded:** The main risk was the compromised user's ability to control an application. I cannot infer from this alone that Carl had tenant-wide administrative privileges.

### Stage 2 — Credential and privilege exposure

**Examined:** I inspected the application's credential configuration, including its expiration information.

**Found:** The client secret displayed an unusually long validity period. The scenario also described the service principal's privileged Microsoft Graph permissions.

**Concluded:** A new secret could allow non-interactive authentication as the application, using permissions previously granted to its service principal. This does not mean Carl personally acquired every administrative role, nor does the screenshot show that the secret was used.

### Stage 3 — Secondary application pivot

**Examined:** I located `Mad-Hat-Labs-App` and reviewed its application properties.

**Found:** A second application registration appeared in the scenario. The briefing says its service principal was added as an owner of the legacy application; the supplied screenshot does not independently display that owners list.

**Concluded:** An attacker-controlled application with an effective ownership relationship could potentially recreate access after a single secret was revoked. Changing Carl's password alone would not resolve every application-level persistence path.

### Stage 4 — Exposed API authorization

**Examined:** I reviewed the legacy application's exposed Application Programming Interface (API) configuration.

**Found:** A delegated authorization scope was configured. The screenshot supports the presence of the configuration, but not misuse of the backend.

**Concluded:** A separate application could request user-delegated access to the exposed API. A poorly secured backend might then act with its own elevated privileges on insufficiently authorized requests, creating a potential confused-deputy risk. The scope itself does not transfer the legacy application's application permissions.

### Stage 5 — Redirect and potential token capture

**Examined:** I reviewed the secondary application's authentication settings and registered redirect destinations.

**Found:** Alongside a development callback, the application listed an external redirect destination that warranted scrutiny. Its address and any exercise-specific values are not reproduced.

**Concluded:** The scenario illustrated a possible OAuth consent-phishing route that could obtain delegated authorization through an already signed-in user. The configuration is not proof that additional users actually granted consent or that access tokens were captured.

## What broke / what surprised me

My initial assumption was that a legacy system had automatically provided general Azure administrator privileges. Reviewing the relationship between the employee, application owner, and service principal clarified a more precise chain: application ownership permitted sensitive configuration changes, while pre-existing application permissions determined the consequences.

I was also surprised by how separate the recovery paths were. Resetting a user's password or requiring MFA again does not necessarily revoke an application secret, remove another application owner, or delete an OAuth consent grant. I had initially interpreted the redirect configuration as simply repeating password phishing; the scenario instead demonstrated authorization-based phishing.

I did not record an extended troubleshooting failure or a time-consuming dead end, so I am not claiming one.

## Findings and recommendations

The scenario highlights the combined exposure created by a compromised employee session, unnecessary application ownership, an unusually long-lived credential, a secondary application control path, and suspect authorization settings. The evidence is insufficient to establish a measured blast radius or independently verify all attacker actions.

For an authorized owner, I would recommend:

1. **Monitor registration and permission changes.** Alert on unexpected new applications, owner modifications, new client secrets, privileged permission grants, consent activity, and changed redirect destinations.
2. **Enforce phishing-resistant sign-in.** Favor passkeys and Fast Identity Online 2 (FIDO2) security keys for high-value identities, while separately restricting risky application consent. Passkeys substantially reduce fake-login credential relay risk but do not eliminate consent phishing.
3. **Reduce persistent privilege.** Remove unnecessary owners, audit application permissions, avoid long-lived reusable secrets, and investigate consent grants and application ownership during incident response—not just the affected employee account.

These are recommendations, not changes I implemented.

## What I learned

- An application owner can introduce security risk even without a tenant-wide administrator role.
- Service principal permissions and human user permissions must be assessed separately.
- Password changes do not necessarily remove malicious application credentials, ownership relationships, or consent grants.
- Redirect configuration and delegated scopes are important investigative leads, not automatic proof of token theft.
- In a future investigation, I would correlate sign-in records, directory audit changes, ownership history, and authorization grants to establish a defensible timeline.

## Evidence and screenshots

Five redacted Azure Portal evidence images are available as clickable links below rather than inline images. Answer-bearing content and sensitive identifiers have been covered or cropped. The screenshots support selected configuration observations, not an independently verified incident timeline.

- [Evidence 01 — Initial application review](images/01-initial-access.webp)
- [Evidence 02 — Application credential configuration](images/02-application-credential.webp)
- [Evidence 03 — Secondary application review](images/03-secondary-application.webp)
- [Evidence 04 — API authorization settings](images/04-api-authorization.webp)
- [Evidence 05 — Redirect configuration](images/05-redirect-configuration.webp)

## Training acknowledgment and disclosure

Thank you to **Mad Hat Labs** for the practical training environment. This is my independent learning portfolio, not an official course publication or endorsement.

The supplied course instruction screenshot is not republished. The five Azure evidence images are redacted presentation copies; flags, mission answers, identifying resource and deployment names, credentials, raw exports, and policy parameter values are excluded. See the [publication policy](../../PUBLICATION-POLICY.md).

**Experience classification: Live multi-user Azure training tenant, Reader access. Not production experience.**