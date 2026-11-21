---
template:
  id: https://context-analysis/method/subsystem-parameters
  name: "Subsystem Parameters"
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
# Subsystem Parameters

Record the parameters exchanged between engineering groups during concurrent design sessions: which group provides each parameter, which groups use it, and its value and unit.

## Layout and rationale

A parameter is recorded **once**, in one row, with the providing group and the using groups side by side. A group's "inputs" and "outputs" are then *derived* from those two columns (see the exchange matrix in the analysis dashboard), instead of being typed twice and drifting apart. This replaces the earlier free-text register, where the same parameter appeared once as an output of one group and once as an input of another.

## Parameter editor

```table-editor
---
target: ${target}
columns: { this: { label: "Parameter" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix entity: <https://www.modelware.io/sierra/entity#> .
@prefix param: <https://context-analysis/method/parameters#> .

param:ParameterShape
    a sh:NodeShape ;
    sh:targetClass param:Parameter ;
    sh:sparql [
        sh:message "A parameter that has a value must also have a unit." ;
        sh:select """
            SELECT $this WHERE {
                $this param:parameterValue ?v .
                FILTER NOT EXISTS { $this param:parameterUnit ?u }
            }
        """ ;
    ] ;
    sh:sparql [
        sh:message "A group must not use a parameter that it provides itself." ;
        sh:select """
            SELECT $this WHERE {
                $this param:isProvidedBy ?g ;
                      param:isUsedBy ?g .
            }
        """ ;
    ] ;
    sh:property [
        sh:path param:isProvidedBy ;
        sh:name "Provided By" ;
        sh:class entity:Discipline ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "A parameter must have exactly one responsible (providing) group." ;
        sh:order 0 ;
    ] ;
    sh:property [
        sh:path param:isUsedBy ;
        sh:name "Used By" ;
        sh:class entity:Discipline ;
        sh:order 1 ;
    ] ;
    sh:property [
        sh:path param:parameterValue ;
        sh:name "Value" ;
        sh:datatype xsd:decimal ;
        sh:maxCount 1 ;
        sh:order 2 ;
    ] ;
    sh:property [
        sh:path param:parameterUnit ;
        sh:name "Unit" ;
        sh:maxCount 1 ;
        sh:order 3 ;
    ] ;
    sh:property [
        sh:path base:description ;
        sh:name "Description" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
        sh:order 4 ;
    ] ;
    .
```
