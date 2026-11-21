---
ontology: https://context-analysis/parameters
title: Subsystem Parameters
---

# Subsystem Parameters

## Purpose

This notebook records the parameters exchanged between engineering groups during the concurrent design process of the Agricultural Earth Observation Satellite.

## Design Rationale

Organizing parameters by the group that provides them and the groups that use them supports information exchange and coordination during concurrent design sessions. It helps the team identify parameter ownership, track the flow of information between groups, keep measurement units consistent, and update values as the design evolves.

## Parameter Exchange Example

The Payload group provides the Payload Swath Width, while Mission Analysis uses it as an input to compute the revisit time.

## Parameters

```compose
template: https://context-analysis/method/subsystem-parameters
target: https://context-analysis/parameters
```
