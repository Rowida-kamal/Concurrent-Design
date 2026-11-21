---
template:
  id: https://context-analysis/method/stakeholder-concern
  name: "Stakeholder and Concern"
  rank: 0
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
    - id: target
      type: iri
      required: true
---
# Stakeholder and Concern

Identify the stakeholders of the agricultural satellite project and the concerns each of them expresses.

## Layout and rationale

The editor is a **table of concerns**, not a table of stakeholders. A concern is the unit that later requirements trace to, and every concern must name the stakeholders who express it, so one row per concern shows the whole chain (stakeholder → concern) at a glance. The stakeholder rows are shown below it with their own business rule, so an unreferenced stakeholder is flagged next to the table that would have revealed it.

## Concern editor

```table-editor
---
target: ${target}
columns: { this: { label: "Concern" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix stakeholder: <https://www.modelware.io/sierra/stakeholder#> .

stakeholder:ConcernShape
    a sh:NodeShape ;
    sh:targetClass stakeholder:Concern ;
    sh:property [
        sh:path stakeholder:isExpressedBy ;
        sh:name "Stakeholders" ;
        sh:class stakeholder:Stakeholder ;
        sh:minCount 1 ;
        sh:message "A concern must be expressed by at least one stakeholder." ;
        sh:order 0 ;
    ] ;
    sh:property [
        sh:path base:priority ;
        sh:name "Priority" ;
        sh:in ( "Low" "Medium" "High" ) ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "A concern must have exactly one priority: Low, Medium or High." ;
        sh:order 1 ;
    ] ;
    sh:property [
        sh:path base:category ;
        sh:name "Category" ;
        sh:order 2 ;
    ] ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "A concern must have exactly one description." ;
        sh:order 3 ;
    ] ;
    .

stakeholder:StakeholderShape
    a sh:NodeShape ;
    sh:targetClass stakeholder:Stakeholder ;
    sh:sparql [
        sh:message "A stakeholder must express at least one concern; otherwise it is not considered in the design." ;
        sh:select """
            SELECT $this WHERE {
                $this a stakeholder:Stakeholder .
                FILTER NOT EXISTS { $this stakeholder:expresses ?concern }
            }
        """ ;
    ] ;
    sh:property [
        sh:path base:category ;
        sh:name "Category" ;
        sh:order 0 ;
    ] ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "A stakeholder must have exactly one description." ;
        sh:order 1 ;
    ] ;
    .
```
