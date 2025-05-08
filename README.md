# Model Management Challenge: Satellite Configuration

Welcome to the Model Management Challenge! This challenge uses a simplified satellite configuration scenario to explore common problems in managing interconnected engineering artifacts.

## Challenge Overview

In modern engineering, designs, requirements, analyses, and reports are often spread across various files and formats. Keeping these artifacts consistent, traceable, and up-to-date is a significant challenge. This set of mock artifacts is designed to highlight these issues.

## Artifacts

The challenge revolves around the following artifacts:

1.  **`ontology.owl`**: An OWL (Web Ontology Language) file defining a vocabulary for satellite components. It specifies types of components (e.g., `ThrusterComponent`, `SolarPanelComponent`) and their properties, such as `hasMass` and `componentID`. It also contains individuals (specific instances of components) with their assigned mass values.
    *   *Purpose*: Acts as a master database or source of truth for component properties.

2.  **`system_config.json`**: A JSON file describing a specific satellite configuration, named `SatAlphaV1`. It lists the components used in this configuration, their quantities, and their individual masses (which should be consistent with the `ontology.owl` file). It also includes a `calculated_total_mass_kg`.
    *   *Purpose*: Defines a bill of materials for a specific system instance and aggregates properties like total mass.

3.  **`requirements.csv`**: A CSV (Comma Separated Values) file outlining design requirements for the satellite system. For example, it specifies a maximum allowable total system mass (`REQ001`) and a maximum allowable mass for any single component (`REQ002`).
    *   *Purpose*: Specifies constraints that the system design must satisfy.

4.  **`report.md`**: A Markdown file summarizing the `SatAlphaV1` system's properties and verifying whether the specified requirements from `requirements.csv` are met.
    *   *Purpose*: Provides a human-readable validation of the system against its requirements.

## The Stories: Model Management Scenarios

Your task is to consider how model management practices and tools could address the following scenarios:

### Story A: Change Impact & Consistency (Ontology → JSON)

**Scenario:**
The mass of a component defined in `ontology.owl` changes. For instance, the `MainThruster001` (ID `T001`), originally 150.5 kg, is found to be heavier, now weighing 165.0 kg. This change is updated in `ontology.owl`.

**Challenge Questions:**
1.  How would this change in `ontology.owl` be detected or propagated to `system_config.json`?
2.  What mechanisms or processes would ensure that the `mass_kg_per_unit` for component `T001` in `system_config.json` is updated to 165.0?
3.  Consequently, how would the `calculated_total_mass_kg` in `system_config.json` be recalculated and updated?

### Story B: Unified Querying & Validation (JSON ↔ CSV → Report)

**Scenario:**
We need to verify that the current `SatAlphaV1` configuration, as defined in `system_config.json`, meets the mass requirement `REQ001` (total system mass <= 350 kg) specified in `requirements.csv`. The `report.md` should reflect this verification.

**Challenge Questions:**
1.  How can we automate the check that the `calculated_total_mass_kg` from `system_config.json` complies with the relevant constraint in `requirements.csv`?
2.  If the `calculated_total_mass_kg` in `system_config.json` were 360.0 kg (due to a component change, for instance), how would the `report.md` be updated to show that `REQ001` is now "NOT SATISFIED"?
3.  What if a new requirement is added to `requirements.csv`? How would the system ensure this new requirement is checked and reported on?

### Story C: Versioning & Change Impact

**Scenario:**
The update to `MainThruster001`'s mass (from 150.5 kg to 165.0 kg in `ontology.owl`) is a significant design change. This implies that `ontology.owl` effectively becomes `ontology_v1.1.owl` (or `_v2.owl`).

**Challenge Questions:**
1.  How should this change trigger new versions of dependent artifacts? For example:
    *   `system_config.json` would need to be updated (reflecting the new mass and new total mass), becoming `system_config_v1.1.json`.
    *   `report.md` would need to be regenerated (reflecting the new values and potentially a changed satisfaction status for `REQ001`), becoming `report_v1.1.md`.
2.  How can we trace the lineage of these artifacts? (e.g., know that `report_v1.1.md` is derived from `system_config_v1.1.json` and `ontology_v1.1.owl`).
3.  If `REQ001` in `requirements.csv` changes its value (e.g., max mass reduced to 340 kg), how does this impact existing configurations and their reports? How are these versions managed?

