# Concurrent Design

OML-based Concurrent Design modeling project for an agricultural Earth observation satellite mission.

## Project overview

This project models and supports the Concurrent Design (CD) process for space missions using the Ontological Modeling Language (OML). It represents the mission design process, the multidisciplinary engineering activities, the system architecture, and the exchange of design parameters between engineering groups.

## Where to look

| File | Content |
|---|---|
| `METHOD.md` | Index of the four patterns, the business rules and the analysis artifacts |
| `ANALYSIS.md` | What the model can answer today, the diagnosed gaps, and how to read each query |
| `src/method` | The reusable method: vocabularies, templates, helper scripts |
| `src/model` | This project's descriptions, bundle and notebooks |

## Build

```
oml lint
oml reason      # reasons over the bundle and validates it
oml render      # renders src/model/md using the bundle (see .oml/settings.yml)
```

The default arguments of each command are in `.oml/settings.yml`.
