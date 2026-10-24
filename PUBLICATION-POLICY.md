# Publication and evidence policy

This repository follows the no-spoiler and redaction requirements supplied for this Mad Hat Labs training work. It does not claim to quote a separately verified official policy document.

Before publishing an update:

- Remove flags, mission answers, solution sequences, exact lab-specific policy results, and clues identifying target resources.
- Remove GUIDs, subscription and tenant identifiers, identifying resource and deployment names, account details, addresses, endpoints, and other sensitive metadata.
- Exclude credentials, tokens, storage keys, connection strings, raw exports, and unredacted screenshots. Review filenames, image metadata, and commit history as well as visible content.
- Publish only evidence permitted by the training provider. When permission or redaction is uncertain, omit the evidence and explain the limitation.
- Separate recorded observations from hypotheses and unverified notes. Do not infer compromise, successful access, or blast radius from a setting alone.
- Describe environment and access honestly. Reader-access training is not production administration or completed remediation.
- Keep future work explicitly marked as planned until it has actually been performed and documented.

## Evidence labels

| Label | Meaning |
| --- | --- |
| Recorded in learner notes | Mentioned in the original notes; not independently rechecked here |
| Unverified | Evidence or context is insufficient to establish the claim |
| General learning | A transferable concept, not a lab-specific finding |
| Recommendation | A proposed owner action, not an implemented change |

If sensitive material is published accidentally, deleting the current file is insufficient because Git history may retain it. Involve the appropriate owner and address exposure, history, and any affected secrets before republishing.