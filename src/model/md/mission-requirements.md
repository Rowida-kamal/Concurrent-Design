---
ontology: https://context-analysis/requirements#
title: Mission Requirements
---
---
compose
template: https://context-analysis/method/mission-requirements
---
# Mission Requirements

## Purpose

This notebook documents the requirements of the Agricultural Earth Observation Satellite Mission.

## Design Rationale

Mission requirements define the expected capabilities and constraints of the satellite. Each requirement is linked to the concern it addresses to maintain traceability.

## Mission Requirements Overview

```table-editor
prefix stakeholder: <https://www.modelware.io/sierra/stakeholder#>
prefix base: <https://www.modelware.io/sierra/base#>
prefix req: <https://www.modelware.io/sierra/requirements#>
prefix sh: <http://www.w3.org/ns/shacl#>
prefix dash: <http://datashapes.org/dash#>

stakeholder:RequirementShape
    a sh:NodeShape ;
    sh:targetClass stakeholder:Requirement ;

    sh:property [
        sh:path base:description ;
        sh:name "Requirement Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] ;

    sh:property [
        sh:path req:satisfies ;
        sh:name "Related Concern" ;
        sh:class stakeholder:Concern ;
        sh:minCount 1 ;
    ] ;
    .
```
