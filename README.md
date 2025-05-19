# Model Management Challenge: Satellite Configuration

Welcome to the Satellite Configuration Model Management Challenge! This challenge uses a simplified satellite configuration scenario to explore common problems in managing interconnected engineering artifacts.

# Case Description

Different artefacts contribute to the development and management of complex engineered (cyber-physical) systems. We are inspired by a recent visit to the [Royal Belgian Institute for Space Aeronomy (BIRA-IASB)](https://www.aeronomie.be) in the design of the artefacts of this challenge. The artefacts revolve around a satellite design and configuration case study. Examples associated with the configuration of aeronautical systems have also been used before, for example, in the tutorials for the Ontological Modelling Language ([OML tutorials](https://www.opencaesar.io/oml-tutorials/)) [@opencaesar].

## Artefacts

The artefacts for the satellite configuration model management challenge are available in this [public GitHub repository](https://github.com/mom-challenge/challenge-2025).

In complex systems engineering -- designs, requirements, analyses, and reports are often spread across various files and formats, with semantics expressed in different (modelling) languages. Keeping these artefacts consistent, traceable, and up-to-date is a significant challenge. This set of example artefacts is designed to highlight these issues.

- **Part catalogue** (`ontology.owl`): We provide an [OWL](https://www.w3.org/TR/owl2-overview/) (Web Ontology Language) file defining a vocabulary for satellite components. It specifies types of components (e.g., *ThrusterComponent*, *SolarPanelComponent*) and their properties (e.g., *hasMass* and *componentID*). It also contains individuals (specific instances of components) with their assigned mass values. The ontology (and corresponding knowledge graph) acts as a product/part catalogue and additionally establishes a common terminology for the various stakeholders of the system, and for the current challenge.

- **System Decomposition / Architecture** (`system_config.json`): We provide a [JSON](https://www.json.org/json-en.html) file describing a specific satellite configuration, named *SatAlphaV1*. It lists the components used in this configuration, their quantities, and their individual masses (which should be consistent with the part catalogue file). It also includes a *calculated_total_mass_kg* property which is the sum of all parts that a system or sub-system is composed of. This specification essentially defines a 'bill of materials' for a specific system instance and aggregates properties like total mass, while also describing the architectural system decomposition.

- **Requirements Specification** (`requirements.csv`): We provide a CSV (Comma Separated Values) file outlining design requirements for the satellite system. For example, it specifies a maximum allowable total system mass (*REQ001*) and a maximum allowable mass for any single component (*REQ002*). Such a specification serves to describe constraints that the system design must satisfy.

- **Generated Report** (`report.md`): We provide a Markdown file summarizing the *SatAlphaV1* system's properties and the verification results of the specified requirements from the requirements specification. The report is a means to provide a human-readable format of the results of validation of the system design for the non-technical stakeholders of the engineering organization.

Note that we aimed to provide the artefacts in the simplest format with the semantics as described above. If your tool or framework does not support the above formats, you are free to re-implement the above artefacts in a format/notation/language of your choice, while respecting the same semantics. We invite you to contribute such new artefacts to the challenge Git repository via a pull request.

In the following section, we describe the model management scenarios of this challenge, without committing to any specific file/artefact format.

## Model Management Scenarios

1. **Change Impact & Consistency**

   The mass of a component defined in the part catalogue changes. In this scenario, the mass of the thruster component changes, because of a change in fuel that is needed for longer flights. Therefore, the *MainThruster001* (with ID *T001*), originally 150.5 kg, is found to be heavier, now weighing 165.0 kg.

   - Consequently, the *mass_kg_per_unit* for component *T001* in the architecture specification needs to be updated to 165.0.
   - Consequently, the *calculated_total_mass_kg* in the architecture specification needs to be recalculated and updated.

2. **Unified Querying & Validation**

   We need to verify that the current *SatAlphaV1* configuration, as defined in the architecture specification, meets the mass requirement *REQ001* (total system mass ≤ 350 kg) described in the requirements specification.

   - The report should reflect this verification.
   - If the *calculated_total_mass_kg* in the architecture specification is (automatically or manually) updated to 360.0 kg (due to the component change from the previous scenario), the requirement is **NOT SATISFIED**. This needs to be updated in the report.

3. **Generation of Views**

   The architecture decomposition artefact presented in the previous section describes the complete set of systems, subsystems, and components in the satellite payload. However, engineers typically focus on their specific views, and their individual view-point-specific contributions contribute to the whole satellite. Let's say there is a communications engineer who is only concerned with the communication subsystem, and would like to only *see* the *part* of the satellite associated with the communications viewpoint.

   - For the UI of the communications engineer, the irrelevant subsystems should be abstracted out and only the relevant ones presented.
   - If the communications engineer modifies a component in the communications subsystem (let's say, adds a new *AntennaComponent* with ID *ANT002*), from the communications view, that change needs to be reflected in the total system architecture.

4. **Versioning**

   The versioning scenario consolidates some and depends on the previously described scenarios:

   The update to *MainThruster001*'s mass (from 150.5 kg to 165.0 kg in the product catalogue) is a significant design change. This implies that, *product_catalogue_v1* effectively becomes *product_catalogue_v2*. This also triggers a change in the architecture specification as described in the change impact scenario (scenario 1). This also potentially impacts and necessitates an update to the report (scenario 2). Consequently, the view may also need to be updated if the *MainThruster001* component is relevant for the viewpoint of a hypothetical propulsion engineer (scenario 3).

   - The lineage of different versions of the same artefact (*product_catalogue_v1* becomes *product_catalogue_v2*, *architecture_specification_v1* becomes *architecture_specification_v2*, *report_v1* becomes *report_v2*) should be tracked.
   - The reasons for creation of a new version of the dependent artefacts (in this case, the architecture specification and report) should be tracked as well (i.e., why was a new version *architecture_specification_v2* and *report_v2* created?).

5. **Collaboration**

   Let's say there are 2 communications engineers working concurrently on the satellite design and they independently make conflicting changes to the satellite communications subsystem. Engineer 1 renames *Antenna Component* with id *ANT001* to *ANT003*, whereas Engineer 2 deletes the antenna with ID *ANT001*.

   - The engineers need live collaboration to visualize changes made by each other in real-time; in such a scenario, possibly these conflicts will not occur in the first place.
   - However, if conflicting (offline) changes are made by the engineers, they need to be handled and possible reconciliation strategies should be suggested.

