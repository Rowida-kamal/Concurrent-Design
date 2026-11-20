---
ontology: https://context-analysis/stakeholder#
title: Stakeholder Analysis
---
```compose
template: https://context-analysis/method/stakeholder-concern

# Stakeholder Analysis

## Purpose
This notebook documents the stakeholders involved in the agricultural satellite concurrent design project.

## Design Rationale
Stakeholders are identified to understand the different expectations and responsibilities involved in the project.

The Project Manager coordinates the project, while the System Engineer and Discipline Engineers support the technical design process. The Customer defines mission expectations, and the Launch Provider and Ground Segment Operators provide operational interface requirements.

## Open Issues
- Are all stakeholder concerns assigned to the appropriate stakeholders?
- Are additional stakeholders needed during the next design sessions?

## Stakeholder and Concern Overview

```table-editor
prefix base: <https://www.modelware.io/sierra/base#>
prefix stakeholder: <https://www.modelware.io/sierra/stakeholder#>
prefix sh: <http://www.w3.org/ns/shacl#>
prefix dash: <http://datashapes.org/dash#>

stakeholder:ConcernShape
   a sh:NodeShape ;
   sh:targetClass stakeholder:Concern ;

   sh:property [
       sh:path base:category ;
       sh:name "Category" ;
   ] ;

   sh:property [
    sh:path base:priority ;
    sh:name "Priority" ;
    sh:minCount 1 ;
    sh:maxCount 1 ;
] ;

   sh:property [
    sh:path stakeholder:isExpressedBy ;
    sh:name "Stakeholders" ;
    sh:class stakeholder:Stakeholder ;
    sh:minCount 1 ;
] ;

   sh:property [
       sh:path base:description ;
       sh:name "Description" ;
       dash:editor dash:TextAreaEditor ;
       sh:maxCount 1 ;
   ] ;
   
   .
```
