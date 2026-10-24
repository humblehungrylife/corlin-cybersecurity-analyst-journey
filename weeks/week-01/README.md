# Week 1 - Reviewing Azure Governance with Evidence Discipline

**Status: Redacted learning reflection based on learner notes. Tenant findings have not been independently verified for publication.**

## Scenario

The training exercise involved reviewing Azure resource organization and governance. My goal was to understand how metadata and available deployment records support an investigation, and how to communicate concerns without treating assumptions as findings.

The exact target, lab outcome, and solution are deliberately withheld. This report explains learning without providing a mission walkthrough.

## Environment and scope

- Platform: Microsoft Azure.
- Setting: live multi-user Azure training tenant provided through Mad Hat Labs.
- Access: Reader access; visibility depends on assigned scope and available records.
- Review areas recorded in my notes: resource organization, tags, deployment information, and governance controls.
- Activity: read-only learning. No production responsibility, exploitation, configuration changes, or remediation is claimed.

## Investigation reflection

My original notes refer to reviewing resource organization, ownership metadata, deployment information, and policy behavior. Exact names, values, navigation sequences, and lab-specific outcomes are omitted to protect the exercise.

The transferable method is to define the question, record the scope and source of each observation, distinguish metadata from corroborated evidence, and identify what further evidence is needed before reaching a security conclusion. Deployment history can contribute context, but it should not be treated as a complete attribution record on its own.

## Evidence and uncertainty

| Area | Public assessment | Evidence limitation |
| --- | --- | --- |
| Governance review | Recorded in learner notes; outcome withheld | No permitted, independently checked tenant evidence is published |
| Ownership and tagging | General learning: metadata supports accountability | Ownership, intent, and responsibility are not established here |
| Possible public network access | Unverified concern in learner notes | Effective connectivity, access controls, and reachability were not validated |
| Possible storage key access setting | Unverified concern in learner notes | No key possession, successful authentication, data access, or compromise was demonstrated |
| Deployment history | Source notes do not contain a completed deployment analysis | Actor, method, chronology, and authorization remain unverified |
| Impact or blast radius | Not established | Requires validated permissions, accessible data, connectivity, and dependencies |

A naming inconsistency does not by itself establish shadow IT or compromise. Public network availability does not by itself establish anonymous data access. A configurable authentication option does not prove that credentials were exposed or used.

## What was difficult or incomplete

My notes mixed observations with possible explanations for another person's choices. Those explanations were not verified, so I do not attribute actions or intent to another learner or operator. My deployment-history notes were incomplete, and I cannot substantiate a timeline or identity from them.

The main improvement is evidence discipline: record what a view actually shows, its scope, and its limitations before proposing a cause. I have not invented a troubleshooting story or filled evidence gaps with a training debrief.

## Findings and recommendations

The defensible outcome is a learning reflection and questions for further authorized review, not a confirmed incident or verified vulnerability report.

Recommendations for an authorized resource owner, subject to operational requirements:

1. Review ownership metadata and governance expectations. Document approved exceptions and responsible owners.
2. Validate effective network and authentication controls before deciding whether access should be restricted. Assess dependencies and test changes in an approved scope.
3. Correlate available deployment records with activity and identity evidence before attributing a change. Document visibility gaps and distinguish recommendations from verified remediation.

These recommendations have not been implemented or tested by me in this report.

## What I learned

- Resource organization and metadata provide context but do not establish intent or authorization.
- Governance controls must be evaluated in scope and operating context; exact lab configuration results are withheld.
- Risk statements need evidence of effective access and impact, not just a concerning setting.
- Next time, I would capture permitted evidence and unknowns as I work, especially for chronology and attribution.
- A useful analyst report communicates uncertainty and gives an owner clear next steps.

## Redaction statement

No flags, GUIDs, mission answers, identifying deployment or resource names, screenshots, raw logs, or credentials are included. Details are intentionally withheld, so readers cannot independently verify tenant observations from this public reflection.