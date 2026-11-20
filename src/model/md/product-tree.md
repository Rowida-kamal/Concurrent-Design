---
ontology: https://context-analysis/components#
title: Product Tree
---

# Product Tree

## 1. Purpose

This pattern describes the hierarchical structure of the Agricultural Earth Observation Satellite.

It defines the satellite, its main subsystems, and the physical parts belonging to each subsystem.

## 2. Product Structure

The satellite is decomposed into five main subsystems. Each subsystem contains physical parts that contribute to its functions.

## 3. Existing Product Structure

| No. | Subsystem | Physical Part | Function |
|---:|---|---|---|
| 1 | PayloadSubsystem | MultispectralCamera | Captures Earth observation images |
| 2 | PayloadSubsystem | PayloadDataProcessor | Processes image data |
| 3 | OBDHSubsystem | OnBoardComputer | Controls onboard operations |
| 4 | PowerSubsystem | BatteryUnit | Stores electrical energy |
| 5 | PowerSubsystem | SolarArray | Generates electrical power |
| 6 | CommunicationSubsystem | CommunicationAntenna | Transmits and receives signals |
| 7 | CommunicationSubsystem | Transceiver | Handles communication signals |
| 8 | AOCSSubsystem | StarTracker | Determines satellite attitude |
| 9 | AOCSSubsystem | ReactionWheel | Controls satellite orientation |

## 4. Description Rules

1. Every subsystem must have a name.
2. Every physical part must have a name.
3. Every physical part must belong to a subsystem.
4. The product structure must represent the satellite decomposition consistently.

## 5. Product Tree Editor

This editor supports the definition of satellite subsystems and their physical parts.

```table-editor
prefix base: <https://www.modelware.io/sierra/base#>
prefix component: <https://www.modelware.io/sierra/component#>
prefix sh: <http://www.w3.org/ns/shacl#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>

component:SubsystemShape
    a sh:NodeShape ;
    sh:targetClass component:Subsystem ;

    sh:property [
        sh:path rdfs:label ;
        sh:name "Subsystem Name" ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] ;

    .

component:PhysicalPartShape
    a sh:NodeShape ;
    sh:targetClass component:PhysicalPart ;

    sh:property [
        sh:path rdfs:label ;
        sh:name "Physical Part Name" ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] ;

    sh:property [
        sh:path component:belongsToSubsystem ;
        sh:name "Belongs to Subsystem" ;
        sh:class component:Subsystem ;
        sh:minCount 1 ;
    ] ;

    .
```

## 6. Rationale

This product structure provides a clear representation of the satellite decomposition. It helps the concurrent design team understand subsystem boundaries and identify the physical parts associated with each subsystem.
