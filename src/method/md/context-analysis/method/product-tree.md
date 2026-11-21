---
template:
  id: https://context-analysis/method/product-tree
  name: "Product Tree"
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
# Product Tree

Decompose the agricultural satellite into subsystems and the physical parts that belong to each subsystem.

## Layout and rationale

A **tree** is the natural layout for a decomposition: the satellite is the root, the subsystems are its children, and the physical parts are their children. The tree editor lets the engineer add a part directly under the subsystem it belongs to, so the "belongs to a subsystem" rule is satisfied by construction instead of being fixed afterwards. The container column is composite (`dash:composite`), so the hierarchy is edited in place.

## Product tree editor

```tree-editor
---
target: ${target}
columns: { this: { label: "Component" } }
---
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix dash: <http://datashapes.org/dash#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix base: <https://www.modelware.io/sierra/base#> .
@prefix component: <https://www.modelware.io/sierra/component#> .

component:ComponentShape
    a sh:NodeShape ;
    sh:targetClass component:Component ;
    sh:property [
        sh:path base:isContainedBy ;
        sh:name "Container" ;
        sh:class component:Component ;
        dash:composite true ;
        sh:order 0 ;
    ] ;
    sh:property [
        sh:path base:description ;
        sh:name "Function" ;
        dash:editor dash:TextAreaEditor ;
        sh:maxCount 1 ;
        sh:order 1 ;
    ] ;
    .

component:SubsystemShape
    a sh:NodeShape ;
    sh:targetClass component:Subsystem ;
    sh:sparql [
        sh:message "A subsystem must be contained in the satellite (or another component)." ;
        sh:select """
            SELECT $this WHERE {
                $this a component:Subsystem .
                FILTER NOT EXISTS { $this base:isContainedBy ?container }
            }
        """ ;
    ] ;
    .

component:PhysicalPartShape
    a sh:NodeShape ;
    sh:targetClass component:PhysicalPart ;
    sh:property [
        sh:path component:belongsToSubsystem ;
        sh:name "Belongs to Subsystem" ;
        sh:class component:Subsystem ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:message "A physical part must belong to exactly one subsystem." ;
        sh:order 2 ;
    ] ;
    .
```
