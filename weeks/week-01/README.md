# Week 01 — Investigating Azure Resource Governance and Deployment Activity

**Mad Hat Labs | Chapter 1: The Blueprint**

**Status:** Training investigation completed; public case study based on learner notes and retrospective documentation.  
**Scenario role:** On-call Azure investigator in the exercise narrative.  
**Actual access:** Reader.  
**Environment:** Live multi-user Azure training tenant. **Not production experience.**

## Scenario

The first week of Mad Hat Labs introduced Microsoft Azure's resource hierarchy, naming conventions, tagging, deployment records, and governance policies, followed by a practical investigation. The scenario involved an inexperienced intern who deployed an experimental Azure environment without fully following organizational standards. Working with Reader access, my objective was to investigate the resources, identify potential governance failures, and document my findings while keeping all mission-specific answers confidential.

## Environment and scope

| Component | Description |
| --- | --- |
| Platform | Microsoft Azure |
| Setting | Live multi-user Azure training tenant through Mad Hat Labs |
| Access | Reader within the assigned training scope |
| Primary tool | Azure Portal |
| Review areas | Resource organization, tags, configuration, deployment history, and governance controls |
| Activity limits | Observation and documentation|

The investigation is described at a general level. Exact targets, configuration outcomes that answer the mission, and a reproducible solution sequence are withheld.

## Investigation
Stage 1 — Resource Organization and Naming Conventions

Examined: Used the Azure Portal search bar to locate the assigned subscription, then reviewed its resource groups and compared their names against the organization's naming conventions.

Found: Identified an inconsistency in the resource naming pattern that warranted further investigation.

Concluded: Pattern recognition helps analysts identify potentially misconfigured or undocumented resources, including possible shadow IT. However, unusual naming alone does not establish unauthorized activity.

Stage 2 — Resource Tags and Cost Management

Examined: Opened the relevant resource's Tags section to review ownership, environment classification, and cost-related metadata.

Found: The available metadata provided operational context, while highlighting the importance of consistent tagging and cost allocation.

Concluded: Tags support accountability, cost management, and security investigations but do not independently establish who created or authorized a resource.

Stage 3 — Deployment History and Resource Investigation

Examined: Navigated to the relevant resource group through the Azure Portal and explored its deployment history.

Found: Located the deployment records associated with the resource group, providing a starting point for understanding how its infrastructure was provisioned.

Concluded: Deployment history supports infrastructure investigations, but a complete timeline and reliable attribution may require additional evidence from Azure Activity Logs.

Stage 4 — Azure Policy and Governance Controls

Examined: Navigated to Azure Policy and reviewed the relevant policy assignment, including its configuration and parameters.

Found: Identified a configuration relevant to understanding the observed naming inconsistency. The mission-specific parameter and value remain confidential.

Concluded: An assigned policy does not necessarily guarantee active enforcement. Additional organizational context would be needed to determine whether its configuration was intentional or an oversight.
---

Overall Finding

This investigation demonstrated how Azure's resource hierarchy, naming conventions, tagging, deployment history, and governance policies work together to support cloud security investigations.

The key takeaway was understanding how to navigate an unfamiliar environment, recognize anomalies, investigate potential governance failures, and evaluate security controls. Most importantly, I learned that identifying an unusual configuration is only the beginning; understanding why it occurred and supporting conclusions with evidence is what makes an investigation meaningful.

In accordance with Mad Hat Labs' spoiler policy, all mission-specific answers, identifiers, and configuration values have been withheld.

### Resource organization and ownership context

I reviewed resource organization and naming patterns as a way to identify items requiring closer examination. I also examined metadata for context about ownership, purpose, environment classification, and cost allocation.

The practical lesson was to treat naming and tagging as investigative context. An unusual name does not prove unauthorized provisioning, and a tag does not independently establish who created or approved a resource. Lab-specific naming and tagging results are omitted.

### Security configuration review

My notes raised concerns about possible public network accessibility and storage shared-key authorization. These remain **unverified security concerns** in this public report: I have not established effective external reachability, successful authentication, accessible data, or compromise.

A configuration review should distinguish what a setting appears to allow from what access is actually possible. Understanding impact would require further authorized validation of network restrictions, authentication, permissions, and relevant dependencies.

### Deployment activity

In my updated account of the exercise, I reviewed deployment history in the Azure Portal to understand the available provisioning records. My initial notes did not preserve a complete deployment analysis, so this retrospective description does not establish a verified timeline, deployment method, or responsible identity.

I considered correlation with Azure Activity Log records as a follow-up method. **A completed Activity Log correlation is not documented or claimed here.** No timestamps, deployment identifiers, actor details, or mission-specific results are published.

### Governance controls

I reviewed governance configuration and how an assigned control's behavior and applicability affect its practical value. The learning point was to assess the relevant scope, configuration, exceptions, and enforcement context instead of assuming that the presence of a policy guarantees prevention.

The exercise-specific policy value, failure mechanism, and final answer are withheld.

## Evidence and confidence
The following screenshots document my investigative approach within the Mad Hat Labs Azure training environment. Mission-specific findings and identifiers are intentionally withheld.

Evidence 01 — Azure Resource Hierarchy

Stage 1 | Management Groups

Reviewed Azure's organizational hierarchy to understand the relationship between management groups and subscriptions.

Confidence: High — hierarchy visible in the screenshot.

Evidence 02 — Resource Naming and Location

Stage 1–2 | Resource Groups

Examined resource group naming patterns and associated locations to identify inconsistencies and understand resource organization.

Confidence: High — visible resource information; investigation-specific findings withheld.

Evidence 03 — Deployment Investigation

Stage 3 | Resource Group Overview

Located the relevant resource group and identified the navigation options for examining deployment activity and associated configurations.

Confidence: High for navigation; deployment details and attribution not independently verified.

Evidence 04 — Governance and Policy Review

Stage 4 | Azure Policy

Reviewed policy assignments and parameters to understand how governance controls influence resource deployment and compliance.

Confidence: Based on investigation notes; mission-specific configuration withheld.

Evidence limitations: Screenshots demonstrate navigation and investigative methodology, not confirmed exploitation or compromise. No verified attack, responsible identity, or measurable blast radius was established. All sensitive identifiers and mission answers must be redacted before publication.
## What surprised me and what I would improve

**Organization can support investigation.** Naming and ownership metadata can help frame useful questions, even though they cannot establish malicious intent.

**Configuration and impact are different questions.** A potentially concerning setting needs context before it becomes a defensible security finding.

**Governance requires more than checking that a control exists.** Applicability and behavior matter when assessing whether a control supports its intended purpose.

**My evidence capture needs to improve.** The initial deployment notes were incomplete. Next time, I would record permitted evidence, scope, and unanswered questions as I work, rather than reconstructing the investigation afterward.

I also learned to remove unsupported explanations about another person's decisions. The exercise narrative provides background; it does not independently establish a real person's actions or intent.

## Assessment and recommendations

The review developed my ability to recognize governance questions and communicate potential risks. It did not establish a confirmed incident or independently verified vulnerability.

Recommendations for an authorized resource owner:

1. **Clarify ownership and provisioning expectations.** Document naming and metadata requirements, responsible owners, and approved exceptions.
2. **Validate effective access before recommending changes.** Review network restrictions and authentication requirements, establish the operational need, and assess dependencies before restricting access.
3. **Complete the change-history review.** Correlate available deployment and activity records before attributing actions or presenting a timeline.
4. **Review governance applicability.** Evaluate scope, exceptions, intended behavior, and operational impact before proposing stronger enforcement.

These are recommendations, not changes I implemented or verified.

## What I learned

- Resource discovery and metadata review help an analyst ask better questions.
- Configuration observations need context before supporting claims about exposure or impact.
- Deployment records can contribute to a timeline, but attribution requires corroboration.
- Governance effectiveness depends on applicability and configuration.
- Clear reporting separates observations, hypotheses, missing evidence, and proposed actions.

## Reflection

This exercise helped me bring Azure resource organization, configuration review, deployment records, and governance into one investigation. My strongest takeaway was that identifying something unusual is the beginning of analysis: the next task is to establish what the evidence supports and explain what still needs checking.

## Training acknowledgment and disclosure

Thank you to **Mad Hat Labs** for the training environment and practical learning exercise. This is an independent portfolio case study, not an official course publication or endorsement.

The exercise briefing is summarized in my own words; the supplied course screenshot is not republished. No flags, GUIDs, mission answers, identifying deployment or resource names, credentials, raw tenant exports, or screenshots are included. Public detail is intentionally limited under the [publication policy](../../PUBLICATION-POLICY.md).

**Experience classification: Live multi-user Azure training tenant, Reader access. Not production experience.**
