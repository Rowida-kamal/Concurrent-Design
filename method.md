
# Concurrent Design Method

## 1. Purpose

This method defines how the Agricultural Earth Observation Satellite project is described and organized using the OML modeling approach.

It provides a consistent structure for capturing stakeholders, mission requirements, satellite architecture, and engineering information during concurrent design.

## 2. Description Patterns

The method is organized into four description patterns.

### 2.1 Stakeholder and Concern Pattern

Identifies the stakeholders involved in the project and the concerns they express.

It supports understanding stakeholder expectations and responsibilities.

### 2.2 Mission Requirements Pattern

Captures the mission requirements and links them to the concerns they address.

It supports requirement traceability throughout the design process.

### 2.3 Product Tree Pattern

Describes the satellite decomposition into subsystems and physical parts.

It supports understanding the system architecture and subsystem boundaries.

### 2.4 Subsystem Parameters Pattern

Provides a structured register for documenting engineering inputs and outputs exchanged between subsystems.

It supports coordination and information exchange during concurrent design sessions.

## 3. Description Organization

The project separates method definitions from project-specific descriptions.

- `src/method/oml`: Contains the reusable OML vocabulary.
- `src/method/md`: Contains the four description patterns and their notebooks.
- `src/model/oml`: Contains project-specific OML descriptions.
- `src/model/md`: Contains project analysis notebooks.

This separation allows the method to be reused in other projects.

## 4. Editors and Validation Rules

Table editors are used to support structured data entry and validation.

The stakeholder and concern pattern includes rules requiring a concern to have a priority and to be associated with at least one stakeholder.

The product tree pattern includes rules requiring subsystem and physical part names and associating physical parts with their subsystems.

Mission requirements are linked to the concerns they satisfy.

## 5. Modeling Approach

The project uses OML to define concepts, relationships, and project-specific instances.

Markdown notebooks combine explanatory text with model-backed content and editor blocks.

This approach keeps the engineering descriptions understandable and connected to the underlying model.

## 6. Rationale

The method was selected to:

- Organize multidisciplinary engineering information.
- Maintain traceability between stakeholders and requirements.
- Represent satellite decomposition consistently.
- Support structured information exchange.
- Separate reusable modeling concepts from project-specific data.
- Facilitate collaboration during concurrent design sessions.

## 7. Expected Outcome

The method provides a reusable description structure for the early design of an agricultural Earth observation satellite.
