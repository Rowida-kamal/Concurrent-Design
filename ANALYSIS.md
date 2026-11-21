# Analysis

This document answers the five project questions from the proposal (Impact, Consistency, Traceability, Completeness, Reuse) with queries over the model, and states plainly where the model cannot answer yet. The live evidence is on the dashboard page `src/model/md/analysis.md`; the per-group pages are in `src/model/md/discipline-views.md`.

The figures below describe the model at the time of writing (4 descriptions: 7 stakeholders, 7 concerns, 5 requirements, 15 components, 8 engineering groups, 8 parameters). Re-render the dashboard to see current values.

## Summary

| Question | Status | Where |
|---|---|---|
| Traceability: which requirements does a design element satisfy? | Partly answered. Requirement → concern → stakeholder works; design element → requirement is a **diagnosed gap** | Dashboard section 1 |
| Completeness: which required inputs are missing, and who owns them? | **Answered** | Dashboard section 2 |
| Impact: if a payload requirement changes, what is affected? | Answered at group level; at requirement and subsystem level it is a **diagnosed gap** | Dashboard section 3 |
| Consistency: do all groups use the same assumptions? | **Diagnosed gap** | Dashboard section 3 |
| Reuse: what can be reused from a previous study? | **Diagnosed gap** | Not queryable |

## Traceability

**Evidence.** The five mission requirements each satisfy a concern, and that concern is expressed by a stakeholder. The chain stakeholder → concern → requirement is complete for the requirements that exist.

**Finding 1: coverage.** All five requirements satisfy the same concern (*Mission Requirements Satisfaction*, expressed by the Customer). The other six concerns (Project Feasibility, Requirement Traceability, Discipline Coordination, Launch Interface Constraints, Ground Segment Interfaces, Model Infrastructure Availability) are satisfied by no requirement. This is a coverage gap in the requirements. Either requirements are missing (for example launch interface and ground segment requirements for mass, volume, data rate and contact time, which the proposal lists as the launch provider's and ground operators' needs), or those concerns are managed outside the model and should say so.

**Gap 1: design element to requirement.** The vocabulary only has `satisfies` from a requirement to a concern. Nothing links a requirement to a parameter, a subsystem or a component, so "which requirements does this design element satisfy?" cannot be answered. Needed: a relation from requirement to design element (for example `isAllocatedTo` from `Requirement` to `base:Element`, with reverse `allocatedRequirement`), plus a coverage query for requirements with no allocation.

## Completeness

**Evidence.** Seven parameters are used as inputs by another group, and none of them has a value yet. The table names the owner of each (Payload: 3, Mission Analysis: 2, Propulsion: 2).

**Finding 2: orphans.** AOCS, Communication and OBDH neither provide nor use any parameter. *Revisit Time* is provided by Mission Analysis but used by nobody. The first is a modeling backlog (the proposal's examples only cover five groups); the second is probably a missing link (a requirement or the Customer's check consumes it).

**Finding 3: near miss.** All seven concerns lack a priority, which the Stakeholder and Concern editor requires. This is data entry that has not been done yet, not a flaw in the method.

## Impact

**Evidence.** A change to a Payload output reaches four groups (Mission Analysis, Power, Propulsion, Structure) through the exchange chain; Mission Analysis reaches three and Propulsion two. The matrix and the graph show which exchange carries the dependency.

**Gap 2: requirement and subsystem level.** The question asks for the affected *parameters, subsystems and requirements*. Groups are linked to parameters, but a group is not linked to the subsystem it is responsible for, and a requirement is not linked to a parameter. Needed: `Requirement` → `Parameter` (or the allocation from Gap 1), and `Discipline` → `Subsystem` (for example `isResponsibleFor`). With both, the impact query becomes a property path from the changed requirement.

## Consistency

**Gap 3: assumptions.** The proposal asks whether all disciplines use the same orbit, eclipse duration and modes. The model has parameters but no *Assumption* concept, so two groups using different values of an assumption cannot be told apart. The closest available check is parameters with more than one user; at present every parameter has at most one user, so even this proxy finds nothing. Needed: an `Assumption` concept (with a value and unit) and a relation from a group to the assumptions it uses, then a query that returns assumptions of the same kind with different values.

## Reuse

**Gap 4: previous studies.** There is no model of a previous study, so there is nothing to reuse and no way to tell which assumptions must be revisited. Needed: a `Study` concept, a relation from a parameter or assumption to the study it came from, and a flag for items reused from another study.

## How to read each query

| Query | Empty result means | A row means |
|---|---|---|
| Requirement → concern → stakeholder | No requirements yet | A `(none)` cell is a broken link |
| Concern coverage | No concerns yet | A concern with 0 is not covered by any requirement |
| Missing inputs | All inputs have values | A parameter that a group is waiting for, and its owner |
| Orphans | The model is as connected as the patterns require | A place where the model is less complete than the method requires |
| Exchange matrix | No parameters exchanged | A positive cell is a dependency (row delivers to column) |
| Impact | Nothing is provided | The groups affected when the origin changes an output |
| Scripted coupling chart | No groups | A group without bars is not modeled yet |

## Limits of this analysis

- The queries run over the asserted facts and the entailments of the bundle. Open-world reasoning means that "no requirement" is reported for a concern because the model has no requirement for it, not because one is known not to exist.
- The groups' dependency is derived from who provides and who uses a parameter. It does not say how large the effect of a change is.
