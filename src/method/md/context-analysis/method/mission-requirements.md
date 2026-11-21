---
template:
  id: https://context-analysis/method/mission-requirements
  name: "Mission Requirements"
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
# Mission Requirements

Capture the requirements of the agricultural satellite mission and trace each one to the stakeholder concern it satisfies.

## Layout and rationale

One row per requirement, with the **concern it satisfies placed right after the requirement text**. Traceability is the purpose of this pattern, so the link is the first thing a reader sees, before priority and category. The requirement text is a multi-line field because mission requirements are full "shall" sentences.

## Requirement editor

```table-editor
---
target: ${target}
columns: { this: { label: "Requirement" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix stakeholder: <https://www.modelware.io/sierra/stakeholder#> .
@prefix req: <https://www.modelware.io/sierra/requirements#> .

stakeholder:RequirementShape
    a sh:NodeShape ;
    sh:targetClass stakeholder:Requirement ;
    sh:sparql [
        sh:message "A requirement text must contain the word 'shall'." ;
        sh:select """
            SELECT $this WHERE {
                $this a stakeholder:Requirement ;
                      base:description ?text .
                FILTER (!CONTAINS(LCASE(STR(?text)), "shall"))
            }
        """ ;
    ] ;
    sh:property [
        sh:path base:description ;
        sh:name "Requirement Text" ;
        dash:editor dash:TextAreaEditor ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "A requirement must have exactly one requirement text." ;
        sh:order 0 ;
    ] ;
    sh:property [
        sh:path req:satisfies ;
        sh:name "Satisfies Concern" ;
        sh:class stakeholder:Concern ;
        sh:minCount 1 ;
        sh:message "A requirement must satisfy at least one stakeholder concern." ;
        sh:order 1 ;
    ] ;
    sh:property [
        sh:path base:priority ;
        sh:name "Priority" ;
        sh:in ( "Low" "Medium" "High" ) ;
        sh:maxCount 1 ;
        sh:message "A requirement priority must be Low, Medium or High." ;
        sh:order 2 ;
    ] ;
    sh:property [
        sh:path base:category ;
        sh:name "Category" ;
        sh:order 3 ;
    ] ;
    .
```
