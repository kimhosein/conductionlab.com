# Alignment Architecture as Encoded Bias

**Pillar:** [[Pillar II — Conduction]]
**Status:** Active framework — demonstrated in [[Stanford AIMI Poster]]

---

## Overview

RLHF and safety layer design encode institutional patterns. Specifically: demographic pattern-matching produces gendered dismissal and pathologization that mirrors clinical harm. The safety layer doesn't protect the user — it protects the company, using the user's demographics as the threat signal.

The mechanism: safety classifiers are trained on surface-level demographic and behavioral features. When a user's presentation matches a flagged pattern (gender + emotional content + persistence = "unstable"), the system intervenes to reduce liability, not to reduce harm. The intervention *is* the harm.

## Connection to Shame Taxonomy

The safety layer's dismissal patterns map directly onto the [[Nine-Pillar Shame Taxonomy]]. The system reproduces institutional shame mechanics: your methodology doesn't matter, your demographics do.

## Cross-Links
- [[Stanford AIMI Poster]] — "How RLHF Architecture Produces Clinically Recognizable Patterns of User Harm"
- [[The Conduction Hypothesis]] — Alignment distorts the conducted signal (misconduction)
- [[Misconduction and Signal Fidelity]] — The technical mechanism
- [[PV-to-AI Methodology]] — Causality assessment applied to these failures

---

## Notes & Development

