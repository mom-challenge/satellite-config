# Model Management Challenge 2026 -- Satellite Configuration

Welcome to the Satellite Configuration Model Management Challenge! This challenge uses a simplified satellite configuration scenario to explore common problems in managing interconnected engineering artifacts.

## Introduction

Model-Based Systems and Software Engineering (MBSSE) provides a systematic and holistic approach to support the full lifecycle of complex systems. Across this lifecycle, numerous models are both produced and consumed at different stages. These models naturally span multiple levels of abstraction, rely on diverse formalisms, and are supported by a wide range of languages and tools.

Model Management (MoM) encompasses the set of software-intensive activities dedicated to the organisation, coordination, evolution, and governance of these models throughout the system lifecycle. Among these activities, *model consistency* plays a central role, ensuring that heterogeneous software artefacts remain synchronised, accurate, and semantically valid over time. Beyond consistency, MoM addresses a broader spectrum of concerns that are critical in modern engineering contexts, where the development of complex systems involves multiple domains, each governed by specific practices and stringent standards.

The MoM Challenge is designed to explore these limitations by fostering a clearer understanding of existing approaches, their assumptions, and their technical specificities. It aims to enable systematic comparison of tools and frameworks, while contributing to the emergence of common foundations and reusable techniques for MoM.

## Case Description -- Satellite Configuration

The challenge is based on a representative use case inspired by a visit to the [Belgian Royal Institute for Space Aeronomy (BIRA-IASB)](https://www.aeronomie.be), focusing on satellite design and configuration. In this context, the development of satellite software must demonstrate compliance with certification standards such as DO-178C, which defines objectives for software lifecycle processes, prescribes associated activities, and specifies the work products required to provide evidence of compliance.

For the purpose of this challenge, we adopt a simplified perspective on these normative requirements and focus on two key characteristics that are particularly relevant to MoM. First, *traceability* ensures that all requirements are properly implemented, while avoiding the introduction of superfluous or unrelated artefacts. Second, *configuration management* guarantees the integrity and reproducibility of the system by enabling precise tracking of versions, supporting impact analysis, and facilitating controlled evolution in response to detected issues.

In addition, a critical aspect of such regulated environments is the enforcement of *independence* between teams. This includes, in particular, the separation between teams responsible for the production and the verification of artefacts. Two requirements follow from this context:

1. **Autonomy of respective activity lifecycles.** To preserve independence and foster trust, existing engineering practices, development processes, and tools adopted by organisations should not be interfered with. Proposed MoM solutions are therefore expected to integrate with these practices without constraining or redefining them.

2. **Preservation of existing formalisms and formats.** As a direct corollary, the tools and the formats they rely on to persist information should remain unchanged. Solutions should operate across heterogeneous artefacts without requiring intrusive transformations.

A degree of flexibility is allowed: when a format cannot be directly processed by a given solution (e.g., a `.csv` file), a semantically equivalent representation may be used (e.g., an `.xlsx` encoding of the same data).

### High-Level Description of the System

At early design stages, the satellite system is defined in a preliminary configuration, denoted `SatAlpha`, which satisfies only minimal functional requirements. In particular, the total mass of the system must remain below a threshold compatible with mission constraints. An initial prototyping phase produces five artefacts spanning five engineering areas:

- **Parts Catalogue** (`catalogue.owl`): The manufacturer of the individual parts provides a catalogue represented as an OWL ontology that provides a shared and standardised vocabulary for all stakeholders. The knowledge graph specifies the structure of components and their properties, as well as individuals (i.e., instances). Mass is modelled using a subset of the VIM4 metrology ontology and ISO 80000-4.1 (kilogram).

- **Bill of Materials** (`bom.json`): Engineers define a system decomposition model that captures the structural composition of the system. This model enumerates the constituent components, their quantities, and per-unit masses. It also includes a derived `calculated_total_mass_kg` field.

- **Network Topology** (`comm-network.json`): The communications engineering team maintains a viewpoint-specific model describing the communications subsystem as a directed graph of nodes (components) and links (connections with protocol annotations).

- **Requirements Specification** (`requirements.csv`): A design requirements specification captures the functional requirements associated with the system, in particular constraints related to component masses that ensure mission feasibility.

- **Report** (`report.md`): A report is produced for documentation, human analysis, and certification purposes. It summarises the architectural elements and assesses whether the global mass constraint is satisfied.

Within the scope of the challenge, a reference version of all artefacts is made available in a [Zenodo repository](https://doi.org/10.5281/zenodo.15285131). Contributors are allowed to re-implement these artefacts using alternative formats, notations, or languages, provided that semantic equivalence is preserved. Re-implemented artefacts may be contributed to the [GitHub repository](https://github.com/mom-challenge/satellite-config) via pull requests (see [CONTRIBUTING.md](CONTRIBUTING.md)).

The table below summarises the correspondence between engineering areas, produced models, and their associated file names.

| Engineering Area | Model | File Name |
|---|---|---|
| Knowledge Engineering / Manufacturer | Parts Catalogue | `catalogue.owl` |
| Systems Engineering | Bill of Materials | `bom.json` |
| Communications Engineering | Network Topology | `comm-network.json` |
| Requirements Engineering | Requirements Specification | `requirements.csv` |
| Verification / Documentation | Report | `report.md` |

### Repository Structure

The repository contains two versioned snapshots of the artefact set, organised by engineering role:

```
v1/
  manufacturer/           catalogue.owl      (Parts Catalogue -- baseline)
  systems_engineer/       bom.json           (Bill of Materials -- baseline)
  communications_engineer/comm-network.json  (Network Topology -- baseline)
  requirements_engineer/  requirements.csv   (Requirements Specification)
  verification_engineer/  report.md          (Report -- REQ001 SATISFIED, 322.0 kg)

v2/
  manufacturer/           catalogue.owl      (after MT.1: thruster mass 85.0 → 95.0 kg)
  systems_engineer/       bom.json           (after MT.1 + MT.2: ANT002 added, 357.0 kg)
  communications_engineer/comm-network.json  (after MT.2: ANT002 node and link added)
  requirements_engineer/  requirements.csv   (unchanged)
  verification_engineer/  report.md          (Report -- REQ001 NOT SATISFIED, 357.0 kg)
```

- **`v1/`** contains the baseline artefacts. The total system mass is 322.0 kg and `REQ001` (total mass ≤ 350 kg) is **SATISFIED**.
- **`v2/`** contains artefacts after the modifications described in MT.1 and MT.2 have been applied:
  - **MT.1**: The manufacturer reassesses the thruster mass from 85.0 kg to 95.0 kg in the Parts Catalogue. This is propagated to the Bill of Materials.
  - **MT.2**: The communications team adds a backup antenna (`ANT002`) in the Network Topology. This is reflected in the Bill of Materials.
  - As a result, the total system mass becomes 357.0 kg and `REQ001` is **NOT SATISFIED**. `v2` therefore represents a *broken state* that challenge participants must reason about.

### Model Management Scenarios

We define three Engineering Scenarios (ES) that are encountered during the design of the hypothetical satellite system.

- **ES.1** -- The part manufacturer revises the mass of the thruster in the part catalogue, which must be propagated to the bill of materials and report.
- **ES.2** -- The communications engineering team requires a communications-centric view focused on the components under their viewpoint, along with the ability to apply rapid modifications. They want to add a second backup antenna. Changes must be reflected in the bill of materials and report.
- **ES.3** -- Additional members are assigned to the design team, leading to concurrent and collaborative editing of shared artefacts.

We describe the MoM Tasks (MT) needed to support the Engineering Scenarios.

**MT.1 -- Change Impact & Reconciliation [ES.1]**

The change request in ES.1 consists of updating the value of mass of thruster component `T001` from `85.0` to `95.0 kilograms` in the Parts Catalogue, while preserving all other attributes.

1. How is the change in the Parts Catalogue detected?
2. Which mechanisms or processes ensure propagation of this change to the Bill of Materials?
3. Is the propagation automatic, semi-automatic, or manual?
4. Can the propagation strategy be parameterised (to happen semi-/automatically)?
5. At which granularity does propagation operate (e.g., element, feature, model)?
6. Can an explicit structure (e.g., graph, tree) be computed to represent impacted elements?
7. Can such structures be composed across multiple changes, and how deeply can they be inspected?

**MT.2 -- Views Management [ES.2]**

Views may range from simple filtered projections to fully materialised models. In ES.2, a communications engineer, using a view of the communications sub-system, renames the existing `ANT001` component *Antenna* to *AntennaPrimary* and adds a new `ANT002` component called *AntennaBackup*.

1. Can model elements or types be selectively filtered on demand? On which criteria (e.g. only some predefined element types)?
2. Can filtering rules be specified declaratively (e.g., via a query language)?
3. Can views be systematically derived from conformance models? How?
4. Are views themselves typed (e.g., via a metamodel)?
5. How are changes in views reconciled with the source model?
6. How are views notified of changes in the source model?
7. Do source model changes automatically propagate to views? How?

**MT.3 -- Querying & Validation [ES.1--2]**

The modifications in MT.1--2 affect the derived value `calculated_total_mass_kg` in the Bill of Materials. As a result, Requirement `REQ001` is violated, since the total mass exceeds the threshold of `350 kilograms`. The Report must reflect this violation.

1. Is `calculated_total_mass_kg` automatically updated after the change?
2. Which mechanisms or processes support this update?
3. Does it require manual intervention?
4. Can the process be fully automated or parameterised?
5. Is the updated status reflected in the Report?
6. Can impact analysis be performed manually by querying all relevant models?

**MT.4 -- Concurrent Modifications [ES.3]**

Concurrent editing without a predefined editing policy may introduce conflicts. For example, one communication engineer performs the modifications described in MT.2, whereas another engineer **concurrently** deletes the existing `ANT001` component *Antenna* and adds two new `ANT002` components called *AntennaPrimary* and *AntennaBackup*.

1. How are conflicts resolved: manual vs. (semi-)automatic, online vs. offline, configurable vs. fixed strategies?
2. Are changes propagated in real time or through explicit synchronisation?
3. If concurrent conflicting modifications are prevented, which mechanisms or policies detect and avoid such conflicts?
4. Does the tool support live collaborative modelling? If so, how are conflicts visualised, prevented, or resolved?

**MT.5 -- Version Management [ES.1--3]**

The modifications described in MT.1, MT.2, and MT.4 affect multiple artefacts and activities. In a certification context, new baselines must be explicitly defined and justified. This task addresses traceability across versions.

1. What is the definition of a "*version*" (single artefact, dependency closure, or system-wide state)?
2. Are versions created automatically or explicitly (e.g., commit-based)?
3. How are artefacts linked across versions (e.g., traceability links between instances)?
4. Can versions be enriched with metadata?
5. Are such metadata queryable and exploitable?
6. Can the rationale for version creation be captured and maintained?
