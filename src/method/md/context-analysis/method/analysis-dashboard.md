---
template:
  id: https://context-analysis/method/analysis-dashboard
  name: "Concurrent Design Analysis Dashboard"
  rank: 1
  expose:
    - kind: compose
  params:
    - id: ontology
      type: iri
      defaultValue: ${context.ontology}
      required: true
---
# Concurrent Design Analysis

Each section answers one of the five project questions (Impact, Consistency, Traceability, Completeness, Reuse) in three steps: the **Question**, the **Evidence** (a live query over the whole model), and **How to read it**. A question the model cannot answer yet is reported as a **diagnosed gap**, not hidden.

## 1. Traceability

**Question.** Which requirements does each stakeholder concern lead to, and who expressed that concern?

**Evidence.** One row per requirement, followed back to the concern it satisfies and the stakeholder who expressed it. An empty cell is a broken link.

```table
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>
PREFIX req: <https://www.modelware.io/sierra/requirements#>

SELECT ?Requirement ?Concern ?Stakeholder
WHERE {
  ?r a stakeholder:Requirement .
  OPTIONAL { ?r rdfs:label ?rl }
  OPTIONAL {
    ?r req:satisfies ?c .
    OPTIONAL { ?c rdfs:label ?cl }
    OPTIONAL {
      ?c stakeholder:isExpressedBy ?s .
      OPTIONAL { ?s rdfs:label ?sl }
    }
  }
  BIND(COALESCE(?rl, STRAFTER(STR(?r), "#")) AS ?Requirement)
  BIND(COALESCE(?cl, STRAFTER(STR(?c), "#"), "(none)") AS ?Concern)
  BIND(COALESCE(?sl, STRAFTER(STR(?s), "#"), "(none)") AS ?Stakeholder)
}
ORDER BY ?Requirement
```

**Concern coverage.** The reverse direction: which concerns are satisfied by at least one requirement, and how many.

```table
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>
PREFIX req: <https://www.modelware.io/sierra/requirements#>

SELECT ?Concern (COUNT(?r) AS ?Requirements)
WHERE {
  ?c a stakeholder:Concern .
  OPTIONAL { ?c rdfs:label ?cl }
  OPTIONAL { ?r req:satisfies ?c }
  BIND(COALESCE(?cl, STRAFTER(STR(?c), "#")) AS ?Concern)
}
GROUP BY ?Concern
ORDER BY ?Requirements ?Concern
```

**How to read it.** A concern with 0 requirements is a coverage gap: someone's need has no requirement behind it. Design elements (parameters, components) are not yet linked to requirements, so "which requirements does a design element satisfy?" is a diagnosed gap in the current vocabulary (see ANALYSIS.md).

## 2. Completeness

**Question.** Which required inputs are missing, and who owns them?

**Evidence.** Every parameter that some group uses as an input but that has no value yet, with the group that owns (provides) it.

```table
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX param: <https://context-analysis/method/parameters#>

SELECT ?Parameter ?Owner ?Needed_By
WHERE {
  ?p param:isUsedBy ?u .
  FILTER NOT EXISTS { ?p param:parameterValue ?v }
  OPTIONAL { ?p rdfs:label ?pl }
  OPTIONAL { ?p param:isProvidedBy ?o . OPTIONAL { ?o rdfs:label ?ol } }
  OPTIONAL { ?u rdfs:label ?ul }
  BIND(COALESCE(?pl, STRAFTER(STR(?p), "#")) AS ?Parameter)
  BIND(COALESCE(?ol, STRAFTER(STR(?o), "#"), "(no owner)") AS ?Owner)
  BIND(COALESCE(?ul, STRAFTER(STR(?u), "#")) AS ?Needed_By)
}
ORDER BY ?Owner ?Parameter
```

**Orphans.** Elements that the patterns require to be connected but are not. One row per finding, grouped by pattern.

```table
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX base: <https://www.modelware.io/sierra/base#>
PREFIX stakeholder: <https://www.modelware.io/sierra/stakeholder#>
PREFIX req: <https://www.modelware.io/sierra/requirements#>
PREFIX component: <https://www.modelware.io/sierra/component#>
PREFIX entity: <https://www.modelware.io/sierra/entity#>
PREFIX param: <https://context-analysis/method/parameters#>

SELECT ?Pattern ?Finding ?Element
WHERE {
  {
    ?x a stakeholder:Stakeholder .
    FILTER NOT EXISTS { ?x stakeholder:expresses ?c }
    BIND("Stakeholder and Concern" AS ?Pattern)
    BIND("Stakeholder expresses no concern" AS ?Finding)
  } UNION {
    ?x a stakeholder:Concern .
    FILTER NOT EXISTS { ?x stakeholder:isExpressedBy ?s }
    BIND("Stakeholder and Concern" AS ?Pattern)
    BIND("Concern has no stakeholder" AS ?Finding)
  } UNION {
    ?x a stakeholder:Concern .
    FILTER NOT EXISTS { ?x base:priority ?p }
    BIND("Stakeholder and Concern" AS ?Pattern)
    BIND("Concern has no priority" AS ?Finding)
  } UNION {
    ?x a stakeholder:Concern .
    FILTER NOT EXISTS { ?r req:satisfies ?x }
    BIND("Mission Requirements" AS ?Pattern)
    BIND("Concern is not satisfied by any requirement" AS ?Finding)
  } UNION {
    ?x a stakeholder:Requirement .
    FILTER NOT EXISTS { ?x req:satisfies ?c }
    BIND("Mission Requirements" AS ?Pattern)
    BIND("Requirement satisfies no concern" AS ?Finding)
  } UNION {
    ?x a component:Subsystem .
    FILTER NOT EXISTS { ?y component:belongsToSubsystem ?x }
    BIND("Product Tree" AS ?Pattern)
    BIND("Subsystem has no physical part" AS ?Finding)
  } UNION {
    ?x a component:PhysicalPart .
    FILTER NOT EXISTS { ?x component:belongsToSubsystem ?s }
    BIND("Product Tree" AS ?Pattern)
    BIND("Physical part belongs to no subsystem" AS ?Finding)
  } UNION {
    ?x a param:Parameter .
    FILTER NOT EXISTS { ?x param:isUsedBy ?u }
    BIND("Subsystem Parameters" AS ?Pattern)
    BIND("Parameter is used by no group" AS ?Finding)
  } UNION {
    ?x a entity:Discipline .
    FILTER NOT EXISTS { ?x param:provides ?p }
    FILTER NOT EXISTS { ?x param:consumes ?p }
    BIND("Subsystem Parameters" AS ?Pattern)
    BIND("Group neither provides nor uses any parameter" AS ?Finding)
  }
  OPTIONAL { ?x rdfs:label ?xl }
  BIND(COALESCE(?xl, STRAFTER(STR(?x), "#")) AS ?Element)
}
ORDER BY ?Pattern ?Finding ?Element
```

**How to read it.** Each row is a place where the model is less complete than the method requires. The first table is the work list for the design sessions; the second is the work list for the modelers.

## 3. Consistency and Impact

**Question.** How do the engineering groups depend on each other through the parameters they exchange, and who is affected when a group's output changes?

**Evidence (matrix).** The number of parameters each providing group (row) hands to each using group (column).

```matrix
---
rowColumnLabel: Provider / User
stylesheet:
  - selector: cell [Number(value) > 0]
    style:
      background-color: lightgreen
---
PREFIX entity: <https://www.modelware.io/sierra/entity#>
PREFIX param: <https://context-analysis/method/parameters#>

SELECT ?row ?column (COALESCE(?n, 0) AS ?value)
WHERE {
  ?row a entity:Discipline .
  ?column a entity:Discipline .
  OPTIONAL {
    SELECT ?row ?column (COUNT(?p) AS ?n)
    WHERE {
      ?p param:isProvidedBy ?row ;
         param:isUsedBy ?column .
    }
    GROUP BY ?row ?column
  }
}
ORDER BY ?row ?column
```

**Evidence (graph).** The same exchange as a network: group → parameter → group.

```graph
---
layout:
  mode: force
  running: true
  fit: true
  padding: 24
stylesheet:
  - selector: node [value.endsWith("Group")]
    group: group
    style:
      fill: yellow
      stroke: grey
      stroke-width: 1
  - selector: node [!value.endsWith("Group")]
    group: parameter
    style:
      fill: cyan
      stroke: grey
      stroke-width: 1
---
PREFIX param: <https://context-analysis/method/parameters#>

CONSTRUCT {
  ?provider param:provides ?p .
  ?p param:isUsedBy ?user .
}
WHERE {
  ?p param:isProvidedBy ?provider .
  OPTIONAL { ?p param:isUsedBy ?user }
}
```

**Evidence (impact).** Every group that is affected, directly or through a chain of exchanges, when the origin group changes one of its outputs.

```table
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX param: <https://context-analysis/method/parameters#>

SELECT ?Origin (COUNT(DISTINCT ?x) AS ?Affected_Groups) (GROUP_CONCAT(DISTINCT ?xn; separator=", ") AS ?Groups)
WHERE {
  ?o param:provides ?anyp .
  ?o (param:provides/param:isUsedBy)+ ?x .
  FILTER (?x != ?o)
  OPTIONAL { ?o rdfs:label ?ol }
  OPTIONAL { ?x rdfs:label ?xl }
  BIND(COALESCE(?ol, STRAFTER(STR(?o), "#")) AS ?Origin)
  BIND(COALESCE(?xl, STRAFTER(STR(?x), "#")) AS ?xn)
}
GROUP BY ?Origin
ORDER BY DESC(?Affected_Groups) ?Origin
```

**How to read it.** A green cell is a dependency: the row group must deliver before the column group can finish. A group with a high affected count is the critical path of a design session. A parameter node with no outgoing edge in the graph has no consumer. The model has no *Assumption* concept yet, so "do all groups use the same orbit and eclipse assumptions?" can only be approximated by looking at parameters with several users; this is a diagnosed gap (see ANALYSIS.md).

## 4. Scripted analysis: coupling of the groups

**Question.** Which groups are the most coupled, as a source of information (outputs) and as a sink (inputs)?

**Evidence.** A Python script fetches the exchange from the model, computes the number of parameters each group provides and uses, and plots them.

```python
include('src/method/py/utils.py')
import micropip
await micropip.install(['matplotlib'])
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt

result = await query("""
  PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
  PREFIX entity: <https://www.modelware.io/sierra/entity#>
  PREFIX param: <https://context-analysis/method/parameters#>
  SELECT ?g ?gLabel ?provided ?used
  WHERE {
    ?g a entity:Discipline .
    OPTIONAL { ?g rdfs:label ?gLabel }
    OPTIONAL { SELECT ?g (COUNT(?pp) AS ?provided) WHERE { ?g param:provides ?pp } GROUP BY ?g }
    OPTIONAL { SELECT ?g (COUNT(?pu) AS ?used) WHERE { ?g param:consumes ?pu } GROUP BY ?g }
  }
  ORDER BY ?gLabel
""")

rows = result['rows']
names = [display_label(r.get('gLabel'), r.get('g', '')).replace(' Group', '') for r in rows]
provided = [int(r.get('provided') or 0) for r in rows]
used = [int(r.get('used') or 0) for r in rows]

fig, ax = plt.subplots(figsize=(max(6, len(names) * 0.9), 3.5))
x = range(len(names))
ax.bar([i - 0.2 for i in x], provided, width=0.4, label='Provides (outputs)', color='#2e86c1')
ax.bar([i + 0.2 for i in x], used, width=0.4, label='Uses (inputs)', color='#f5b041')
ax.set_xticks(list(x))
ax.set_xticklabels(names, rotation=30, ha='right', fontsize=9)
ax.set_ylabel('Parameters')
ax.set_title('Parameters provided and used per engineering group', fontsize=11)
ax.legend()
plt.tight_layout()
display(image_html(fig))

idle = [n for n, p, u in zip(names, provided, used) if p == 0 and u == 0]
if idle:
    display('Groups with no parameter exchange yet: ' + ', '.join(idle))
```

**How to read it.** Tall blue bars are the groups whose outputs many others wait for; tall orange bars are the groups that depend on many inputs. A group with no bars has not been modeled yet, and the line below the chart names it.
