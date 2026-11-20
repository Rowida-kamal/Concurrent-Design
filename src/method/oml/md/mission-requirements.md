
---
template:
  id: https://context-analysis/method/mission-requirements
  name: "Mission Requirements"
  expose:
    - kind: compose
      params:
        - id: ontology
          type: iri
          defaultValue: ${context.ontology}
          required: true
---

# Mission Requirements

## Purpose

Define and organize the requirements of the agricultural satellite mission.

## Structure

- Requirement: A condition or capability that the satellite must satisfy.
- Stakeholder Concern: The concern addressed by the requirement.
- Relationship: Each requirement is linked to the relevant concern.

## Rules

1. Every requirement must have a unique name.
2. Every requirement must have a clear description.
3. Every requirement should be linked to a relevant stakeholder concern.

## Project Examples

- REQ_MissionLifetime: The satellite shall operate for at least 5 years.
- REQ_MaximumMass: The satellite mass shall be less than 350 kg.
- REQ_SunSynchronousOrbit: The satellite shall operate in Sun-Synchronous Low Earth Orbit.
- REQ_RevisitTime: The satellite revisit time shall be less than 5 days.
- REQ_RGBImaging: The satellite shall capture RGB images covering Egyptian territory.

## Rationale

Organizing mission requirements helps the design team trace stakeholder concerns to measurable mission constraints and capabilities.
