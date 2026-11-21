# Concurrent Design Method

## 1. Purpose

This method defines how the early design (Phase 0/A) of the Egypt Agricultural Earth Observation Satellite is described, checked and analyzed with OML. It captures stakeholders and their concerns, mission requirements, the satellite's product tree, and the parameters exchanged between engineering groups.

The method is separated from the model: everything under `src/method` is reusable for another mission, and everything under `src/model` is this project's data.

## 2. Index of patterns

| # | Pattern | Method template (`src/method/md/context-analysis/method/`) | Model notebook (`src/model/md/`) | Description (`src/model/oml/context-analysis/`) | Editor |
|---|---|---|---|---|---|
| 1 | Stakeholder and Concern | `stakeholder-concern.md` | `stakeholder-analysis.md` | `stakeholder.oml` | table-editor |
| 2 | Mission Requirements | `mission-requirements.md` | `mission-requirements.md` | `requirements.oml` | table-editor |
| 3 | Product Tree | `product-tree.md` | `product-tree.md` | `components.oml` | tree-editor |
| 4 | Subsystem Parameters | `subsystem-parameters.md` | `subsystem-parameters.md` | `parameters.oml` | table-editor |

Each notebook is thin: a title, the design rationale and open issues, and one `compose` block that calls its template. The template owns the shape, the business rules, the editor and the layout rationale.

The vocabulary used by pattern 4 is `src/method/oml/context-analysis/method/parameters.oml` (a `Parameter` data product, `isProvidedBy`, `isUsedBy`, value and unit). The other three patterns use the Sierra vocabulary in `src/method/oml/www.modelware.io/sierra`.

## 3. Business rules (SHACL)

| Pattern | Rule | Where |
|---|---|---|
| Stakeholder and Concern | A stakeholder must express at least one concern (`sh:sparql`) | `stakeholder-concern.md` |
| Stakeholder and Concern | A concern needs at least one stakeholder, exactly one priority (Low, Medium or High) and one description | `stakeholder-concern.md` |
| Mission Requirements | A requirement text must contain "shall" (`sh:sparql`) | `mission-requirements.md` |
| Mission Requirements | A requirement must satisfy at least one concern | `mission-requirements.md` |
| Product Tree | A subsystem must be contained in the satellite (`sh:sparql`) | `product-tree.md` |
| Product Tree | A physical part must belong to exactly one subsystem | `product-tree.md` |
| Subsystem Parameters | A parameter with a value must have a unit (`sh:sparql`) | `subsystem-parameters.md` |
| Subsystem Parameters | A group must not use a parameter it provides (`sh:sparql`) | `subsystem-parameters.md` |
| Subsystem Parameters | A parameter has exactly one providing group | `subsystem-parameters.md` |

Every rule carries an `sh:message`, so the editor says what is wrong in the engineer's words.

## 4. Analysis (Module 5)

| Artifact | File |
|---|---|
| Analysis dashboard template (table, matrix, graph, text, scripted Python chart) | `src/method/md/context-analysis/method/analysis-dashboard.md` |
| View template, one page per engineering group | `src/method/md/context-analysis/method/group-exchange.md` |
| Dashboard page for this project, with findings | `src/model/md/analysis.md` |
| Two group pages from the view template | `src/model/md/discipline-views.md` |
| Write-up: answers, diagnosed gaps, how to read each query | `ANALYSIS.md` |

The dashboard reads the description bundle `src/model/oml/context-analysis/bundle.oml`, which includes all four descriptions, so queries and the reasoner see the whole model.

## 5. Folder organization

- `src/method/oml`: reusable vocabularies (Sierra base, stakeholder, requirements, component, entity, process, plus `context-analysis/method/parameters.oml`).
- `src/method/md`: the templates (shapes, editors, rules, dashboard, view).
- `src/method/py`: small helper functions for scripted blocks (`utils.py`, from the Sierra method).
- `src/model/oml`: project descriptions and the bundle.
- `src/model/md`: project notebooks.

One concern per description file: stakeholders and concerns, requirements, components, and parameters are separate descriptions.

## 6. Rationale

- Organize multidisciplinary engineering information in one model.
- Keep the traceability chain stakeholder → concern → requirement checkable by query.
- Represent the satellite decomposition once, as a tree.
- Record each exchanged parameter once, with its provider and users, so inputs and outputs are derived instead of typed twice.
- Keep the reusable method apart from the project data, so the method can be reused for the next study.

## 7. Known limits

See `ANALYSIS.md`: design elements are not yet linked to requirements, there is no *Assumption* concept, and there is no model of a previous study to reuse. These are diagnosed gaps, with the vocabulary change each one needs.
