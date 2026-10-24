# Week 01 — Investigating Azure Resource Governance and Deployment Activity

**Mad Hat Labs | Chapter 1: The Blueprint**

**Status:** Training investigation completed; public case study based on learner notes and retrospective documentation.  
**Scenario role:** On-call Azure investigator in the exercise narrative.  
**Actual access:** Reader.  
**Environment:** Live multi-user Azure training tenant. **Not production experience.**

## Scenario

The exercise presented a temporary deployment that required a governance review. In the scenario, a junior team member had provisioned a test environment, and the investigator was asked to assess resource organization, configuration, and deployment activity using read-only access.

My objective was to understand what the available information supported, identify concerns requiring follow-up, and document recommendations without overstating the evidence. The scenario's job title and events describe a training narrative, not employment responsibilities or a real organizational incident.

## Environment and scope

| Component | Description |
| --- | --- |
| Platform | Microsoft Azure |
| Setting | Live multi-user Azure training tenant through Mad Hat Labs |
| Access | Reader within the assigned training scope |
| Primary tool | Azure Portal |
| Review areas | Resource organization, tags, configuration, deployment history, and governance controls |
| Activity limits | Observation and documentation; no exploitation, configuration changes, or remediation |

The investigation is described at a general level. Exact targets, configuration outcomes that answer the mission, and a reproducible solution sequence are withheld.

## Investigation

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

This is a learner-authored case study. The observations below have not been independently rechecked against the tenant for publication, and no supporting tenant screenshots or raw records are included.

| Review area | Basis | What remains unresolved |
| --- | --- | --- |
| Resource organization and metadata | Learner notes and retrospective account | Lab-specific results withheld; ownership and authorization not independently established |
| Network accessibility | Concern recorded in learner notes | Effective reachability and data exposure unverified |
| Storage authorization | Concern recorded in learner notes | Credential possession, successful access, and impact unverified |
| Deployment history | Updated learner account; incomplete original notes | No substantiated chronology or attribution |
| Governance controls | Learner account of the review | Exact configuration and mission outcome withheld |
| Compromise or blast radius | Not established | No confirmed incident, exploitation, or measured impact |

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
