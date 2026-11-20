---
template:
id: https://context-analysis/method/subsystem-parameters
  name: "Subsystem Interfaces"
  expose:
    - kind: compose
      params:
        - id: ontology
          type: iri
          defaultValue: ${context.ontology}
          required: true
---

# Subsystem Interfaces – Input and Output Parameters

## Purpose

Define the input and output parameters exchanged between engineering subsystems during concurrent design sessions.

## Structure

- Subsystem: An engineering discipline responsible for a specific part of the satellite design.
- Input Parameters: Data received from other subsystems.
- Output Parameters: Data produced and shared with other subsystems.
- Parameter: A design value with a name, value, and unit.

## Rules

1. Each subsystem should have defined input and output parameters.
2. Every input parameter must have a clear source.
3. Every output parameter must have a responsible subsystem.
4. Parameters must include units when applicable.
5. Input and output parameters must be consistent between connected subsystems.

## Project Examples

### Mission Analysis Subsystem

**Inputs**
- Payload Swath Width
- Satellite Altitude

**Outputs**
- Revisit Time
- Ground Coverage
- Orbit Parameters

### Power Subsystem

**Inputs**
- Subsystem Power Consumption
- Orbit Eclipse Duration

**Outputs**
- Power Budget
- Battery Capacity
- Solar Array Power

### Payload Subsystem

**Inputs**
- Ground Resolution
- Swath Width
- Imaging Requirements

**Outputs**
- Camera Specifications
- Payload Mass
- Payload Power Consumption

### Structure Subsystem

**Inputs**
- Subsystem Masses
- Equipment Dimensions

**Outputs**
- Structural Mass
- Equipment Accommodation

### Propulsion Subsystem

**Inputs**
- Orbit Requirements
- Satellite Mass

**Outputs**
- Propellant Mass
- Propulsion System Mass
- Propulsion Power Consumption

## Rationale

Defining subsystem interfaces ensures that engineering teams exchange consistent design information, understand their dependencies, and coordinate their work during concurrent design sessions.
