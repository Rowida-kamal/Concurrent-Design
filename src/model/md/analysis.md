---
ontology: https://context-analysis/bundle
title: Analysis Dashboard
---

# Analysis Dashboard

This page evaluates the whole Concurrent Design model, so it reads the **bundle** (all descriptions together), not a single description. The sections below come from the method's analysis template; the findings that follow are what this project's data currently says. The complete write-up, including the diagnosed gaps, is in `ANALYSIS.md`.

```compose
template: https://context-analysis/method/analysis-dashboard
```

## Findings for this project

1. **Traceability.** All five mission requirements trace to one concern (*Mission Requirements Satisfaction*, expressed by the Customer). The other six concerns have no requirement behind them. This is a coverage gap in the requirements, not a modeling error.
2. **Completeness.** Seven parameters are used as inputs but have no value yet. Eight are provided by Payload, Mission Analysis and Propulsion; AOCS, Communication and OBDH exchange nothing yet, and *Revisit Time* is provided but used by nobody.
3. **Impact.** A change in a Payload output reaches four other groups through the exchange chain; Mission Analysis reaches three and Propulsion two.
4. **Gaps in the method.** Design elements are not linked to requirements, and there is no *Assumption* concept; so two of the five project questions (design element to requirement, and shared assumptions) can only be answered partially. See `ANALYSIS.md`.
