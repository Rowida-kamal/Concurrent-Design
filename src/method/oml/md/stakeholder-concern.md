---
template:
  id: https://context-analysis/method/stakeholder-concern
  name: "Stakeholder and Concern"
  expose:
    - kind: compose
      params:
        - id: ontology
          type: iri
          defaultValue: ${context.ontology}
          required: true
---

# Stakeholder and Concern

## Purpose

Identify the stakeholders involved in the agricultural satellite project and document their concerns.

## Structure

- Stakeholder: A person, team, or organization involved in the project.
- Concern: An interest or expectation that needs to be considered.
- Relationship: A stakeholder expresses one or more concerns.

## Rules

1. Every stakeholder must have a name.
2. Every concern must have a description.
3. Every stakeholder must be linked to at least one concern.
4. Use the `isExpressedBy` relationship to connect stakeholders and concerns.

## Project Examples

### Stakeholders

- ProjectManager
- SystemEngineer
- DisciplineEngineers
- Customer
- LaunchProvider
- GroundSegmentOperators
- ITSupportTeam

### Concerns

- ProjectFeasibility
- RequirementTraceability
- DisciplineCoordination
- MissionRequirementsSatisfaction
- LaunchInterfaceConstraints
- GroundSegmentInterfaces
- ModelInfrastructureAvailability

## Rationale

This structure helps the concurrent design team identify stakeholder expectations and consider them during mission design.
