# Model Management Challenge: Satellite Configuration

Welcome to the Model Management Challenge! This challenge uses a simplified satellite configuration scenario to explore common problems in managing interconnected engineering artifacts.

## Challenge Overview

In modern engineering, designs, requirements, analyses, and reports are often spread across various files and formats. Keeping these artifacts consistent, traceable, and up-to-date is a significant challenge. This set of mock artifacts is designed to highlight these issues.

## Artifacts

The challenge revolves around the following artifacts:

1.  **`ontology.owl`**: An OWL (Web Ontology Language) file defining a vocabulary for satellite components. It specifies types of components (e.g., `ThrusterComponent`, `SolarPanelComponent`) and their properties, such as `hasMass` and `componentID`. It also contains individuals (specific instances of components) with their assigned mass values.
    *   *Purpose*: Acts as a product/part catalogue and additionally establishes a common terminology.

2.  **`system_config.json`**: A JSON file describing a specific satellite configuration, named `SatAlphaV1`. It lists the components used in this configuration, their quantities, and their individual masses (which should be consistent with the `ontology.owl` file). It also includes a `calculated_total_mass_kg`.
    *   *Purpose*: Defines a bill of materials for a specific system instance and aggregates properties like total mass.

3.  **`requirements.csv`**: A CSV (Comma Separated Values) file outlining design requirements for the satellite system. For example, it specifies a maximum allowable total system mass (`REQ001`) and a maximum allowable mass for any single component (`REQ002`).
    *   *Purpose*: Specifies constraints that the system design must satisfy.

4.  **`report.md`**: A Markdown file summarizing the `SatAlphaV1` system's properties and verifying whether the specified requirements from `requirements.csv` are met.
    *   *Purpose*: Provides a human-readable validation of the system against its requirements.

## The Stories: Model Management Scenarios

You do not have to implement all the scenarios or all of their aspects in order for a successfull submission to the challenge. The challenge questions do not necessarily cover all possible changes to keep the models consistent. Not all stories need to be implemented for a submission to this challenge. While we appreciate and favor real demonstrations with the tools, it is sufficient for a submission to discuss the features of the approach wrt. the stories and challenges therein without actually implementing them. Your task is to consider how your model management practices and tools (just `tool` from this point on) address the following scenarios:

### Story A: Change Impact & Consistency (Ontology → JSON)

**Scenario:**
The mass of a component defined in `ontology.owl` changes. In this scenario, the mass of the thruster component changes, because of a change in fuel that is needed for longer flights. Therefore, the `MainThruster001` (ID `T001`), originally 150.5 kg, is found to be heavier, now weighing 165.0 kg. This change is updated in `ontology.owl`.

**Challenge Questions:**
1.  How does your tool detect this change in `ontology.owl`?
2.  What mechanisms or processes ensure that the `mass_kg_per_unit` for component `T001` in `system_config.json` is updated to 165.0?
3.  Consequently, how does your tool recalculate and update the `calculated_total_mass_kg` in `system_config.json`?

### Story B: Unified Querying & Validation (JSON ↔ CSV → Report)

**Scenario:**
We need to verify that the current `SatAlphaV1` configuration, as defined in `system_config.json`, meets the mass requirement `REQ001` (total system mass <= 350 kg) specified in `requirements.csv`. The `report.md` should reflect this verification.

**Challenge Questions:**
1.  How does your tool automate the check that the `calculated_total_mass_kg` from `system_config.json` complies with the relevant constraint in `requirements.csv`?
2.  When the `calculated_total_mass_kg` in `system_config.json` is (automatic or manually) updated to 360.0 kg (due to the component change), how does your tool update the `report.md` to show that `REQ001` is now "NOT SATISFIED"?
3.  How does your tool reflect new requirements and their evaluation in the report? The new requirement `Every satelite needs at least one solar panel.` is added to `requirements.csv`. How does your tool ensure this new requirement is checked and reported on, e.g., by adding new rules?

### Story C: Versioning

**Scenario:**
The update to `MainThruster001`'s mass (from 150.5 kg to 165.0 kg in `ontology.owl`) is a significant design change. This implies that `ontology.owl` effectively becomes `ontology_v1.1.owl` (or `_v2.owl`).

**Challenge Questions:**
1.  In your tool, whether or how does this change trigger new versions of dependent artifacts? Please describe how your tool handles this, e.g., by creating copies, branches, or using a change-based representation. Please discuss the approach of your tool based on:
    *   `system_config.json` needs to be updated (reflecting the new mass and new total mass), becoming `system_config_v1.1.json`.
    *   `report.md` needs to be updated/regenerated (reflecting the new values and potentially a changed satisfaction status for `REQ001`), becoming `report_v1.1.md`.
2.  How does your tool trace the lineage of these artifacts? (e.g., know that `report_v1.1.md` is derived from `system_config_v1.1.json` and `ontology_v1.1.owl`).
3.  `REQ001` in `requirements.csv` changes its value (`calculated_total_mass_kg` reduced to 340 kg). How does this impact existing configurations and their reports, managed by your tool? Wether or how are these versions managed?

### Story D: Generation of Views

**Scenario:**
`REQ002` in `requirements.csv` is applicable to every single component. However, from the file itself it is not clear, what components are of interest here.

**Challenge Questions:**
1.  Whether and if so how does your tool allow for the creation of views, i.e., aggregating parts of different models into a new format to be viewed (and possibly modified) by a user?
2.  How can you create a view in your tool that shows the different components from `ontology.owl` to which `REQ002` in `requirements.csv` applies?
