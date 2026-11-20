# Product Tree

## Purpose
Organize the agricultural satellite into subsystems and physical components.

## Structure
- Satellite: The complete space system.
- Subsystem: A major functional part of the satellite.
- Physical Part: A physical component belonging to a subsystem.

## Rules
1. Every subsystem must belong to the satellite.
2. Every physical part must belong to one subsystem.
3. Every component must have a meaningful name.

## Project Examples

### Satellite
- AgriculturalSatellite

### Subsystems
- PayloadSubsystem
- OBDHSubsystem
- PowerSubsystem
- CommunicationSubsystem
- AOCSSubsystem

### Physical Parts
- MultispectralCamera
- PayloadDataProcessor
- OnBoardComputer
- BatteryUnit
- SolarArray
- CommunicationAntenna
- Transceiver
- StarTracker
- ReactionWheel

## Rationale
Decomposition helps the concurrent design team organize the satellite architecture and understand the responsibility of each subsystem.