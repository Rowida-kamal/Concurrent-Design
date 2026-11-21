---
template:
  id: https://context-analysis/method/group-exchange
  name: "Engineering Group Exchange"
  rank: 2
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
    - id: group
      type: iri
      required: true
---
# [[${group}]]

What this engineering group needs from the others, what it delivers to them, and who is affected if its outputs change. One page per group, built from the same template.

## Inputs this group depends on

An input without a value is a missing required input; the owner is the group that has to provide it.

```table
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX param: <https://context-analysis/method/parameters#>

SELECT ?Input ?Provided_By (COALESCE(STR(?v), "(missing)") AS ?Value) (COALESCE(?u, "") AS ?Unit)
WHERE {
  ?p param:isUsedBy <${group}> .
  OPTIONAL { ?p rdfs:label ?pl }
  OPTIONAL { ?p param:isProvidedBy ?o . OPTIONAL { ?o rdfs:label ?ol } }
  OPTIONAL { ?p param:parameterValue ?v }
  OPTIONAL { ?p param:parameterUnit ?u }
  BIND(COALESCE(?pl, STRAFTER(STR(?p), "#")) AS ?Input)
  BIND(COALESCE(?ol, STRAFTER(STR(?o), "#"), "(no owner)") AS ?Provided_By)
}
ORDER BY ?Input
```

## Outputs this group delivers

```table
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX param: <https://context-analysis/method/parameters#>

SELECT ?Output ?Value ?Unit (GROUP_CONCAT(?un; separator=", ") AS ?Used_By)
WHERE {
  ?p param:isProvidedBy <${group}> .
  OPTIONAL { ?p rdfs:label ?pl }
  OPTIONAL { ?p param:parameterValue ?v }
  OPTIONAL { ?p param:parameterUnit ?u }
  OPTIONAL { ?p param:isUsedBy ?user . OPTIONAL { ?user rdfs:label ?ul } }
  BIND(COALESCE(?pl, STRAFTER(STR(?p), "#")) AS ?Output)
  BIND(COALESCE(STR(?v), "(missing)") AS ?Value)
  BIND(COALESCE(?u, "") AS ?Unit)
  BIND(COALESCE(?ul, STRAFTER(STR(?user), "#"), "") AS ?un)
}
GROUP BY ?Output ?Value ?Unit
ORDER BY ?Output
```

## Groups affected by a change

Every group that depends on this group, directly or through a chain of exchanges.

```text
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX param: <https://context-analysis/method/parameters#>

SELECT (IF(?list = "", "No group depends on this group.", ?list) AS ?affected)
WHERE {
  {
    SELECT (GROUP_CONCAT(DISTINCT ?xn; separator=", ") AS ?list)
    WHERE {
      <${group}> (param:provides/param:isUsedBy)+ ?x .
      FILTER (?x != <${group}>)
      OPTIONAL { ?x rdfs:label ?xl }
      BIND(COALESCE(?xl, STRAFTER(STR(?x), "#")) AS ?xn)
    }
  }
}
```
