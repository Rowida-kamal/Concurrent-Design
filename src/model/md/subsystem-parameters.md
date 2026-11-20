---
ontology: https://context-analysis/components#
title: Subsystem Parameters
---
---
compose
template: https://context-analysis/method/subsystem-parameters
---
# Subsystem Parameters

## 1. Purpose

This pattern describes the input and output parameters exchanged between engineering groups during the concurrent design process of the Agricultural Earth Observation Satellite.

It provides a structured way to record engineering data and coordinate information exchange between different disciplines.

## 2. Parameter Structure

Each parameter is defined by the following attributes:

- Engineering Group: The group responsible for the parameter.
- Parameter Name: The name of the exchanged parameter.
- Parameter Type: Input or Output.
- Value: The parameter value.
- Unit: The measurement unit.
- Source / Destination: The group providing or receiving the parameter.

## 3. Subsystem Parameters Register

The following table is used to record input and output parameters for each engineering group during concurrent design sessions.

| No. | Engineering Group | Parameter Name | Type (Input/Output) | Value | Unit | Source / Destination |
|---:|---|---|---|---|---|---|
| 1 | Mission Analysis | | | | | |
| 2 | Payload | | | | | |
| 3 | Power | | | | | |
| 4 | Structure | | | | | |
| 5 | Propulsion | | | | | |
| 6 | AOCS | | | | | |
| 7 | Communication | | | | | |
| 8 | OBDH | | | | | |

## 4. Description Rules

1. Every parameter must have a name.
2. Every parameter must be classified as Input or Output.
3. Every parameter must have a measurement unit when applicable.
4. Every output parameter must identify its responsible engineering group.
5. Every input parameter should identify its source group.
6. Parameter values should be updated when new design results become available.

## 5. Parameter Exchange Example

For example, the Payload group may provide Swath Width as an output parameter, while Mission Analysis uses it as an input to calculate revisit time.

This exchange ensures that engineering groups use consistent and updated design information.

## 6. Rationale

Organizing parameters by engineering group supports information exchange and coordination during concurrent design sessions.

It helps the team:

- Identify parameter ownership.
- Track inputs and outputs between groups.
- Maintain consistent measurement units.
- Update design values as the project evolves.
- Support multidisciplinary design decisions.
