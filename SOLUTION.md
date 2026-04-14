# Presenting your Solution

This document specifies the expected structure and evaluation dimensions for solution submissions to the MoM Challenge 2026. A solution should describe how the proposed approach or tool supports the case study introduced in [README.md](README.md), and how it addresses the MoM concerns formalised there.

## Expected Structure of a Solution

Submissions should follow a structured format to ensure comparability across approaches. A LaTeX template is provided (available as an Overleaf project) and organised as follows:

1. **High-Level Dimensions and Functions of MoM.**
   Authors must position their approach with respect to the dimensions introduced in the [High-Level Dimensions and Functions](#high-level-dimensions-and-functions-hdf-of-mom) section below, and describe how these are supported by their tool or methodology.

2. **Responses to MTs.**
   For each MoM Task (MT) defined in [README.md](README.md#model-management-scenarios), submissions must include:
   - A **precise and concise answer** to each sub-question (e.g., MT.1.1, MT.1.2, etc.);
   - A **discussion of alternative mechanisms or extensions**, where relevant, beyond the scope of the questions;
   - A **summary score** (minimum zero, maximum thirty), assigned as one point per positively addressed sub-question (partial scores may be used where appropriate).

3. **Replication Package (optional but recommended).**
   Submissions may include a publicly accessible Replication Package, enabling demonstration and validation of the proposed solution. This may consist of:
   - A Zenodo repository with complete software artefacts and execution instructions;
   - A containerised environment (e.g., Docker image) with pre-configured setup.

   Note that a link to a GitHub or other impermanent repository is not acceptable as a replication package.

4. **Conclusion.**
   The conclusion should:
   - Summarise the **main strengths and limitations** of the approach;
   - Describe any **adaptations or assumptions** made to fit the case study;
   - Identify **unaddressed requirements or limitations** of the challenge;
   - Provide **lessons learnt** and implications for future work, both for the authors and the broader MoM community.

## High-Level Dimensions and Functions (HDF) of MoM

To support systematic comparison, submissions must explicitly position their solution with respect to the following dimensions and functions of MoM.

---

**HDF.1 -- Approach**

The ISO 14258 standard identifies four categories of system integration: *Integration*, *Unification*, *Federation*, and *Hybrid*. Authors must specify which category best characterises their approach, or justify deviations from this classification.

---

**HDF.2 -- Technological Spaces**

Authors must describe how the solution handles heterogeneity across Technological Spaces. This dimension directly relates to the requirement of preserving existing formalisms and formats (see [README.md](README.md#case-description----satellite-configuration)).

---

**HDF.3 -- Relationships**

Authors must explain how relationships between artefacts are *represented*, *interpreted*, and *managed*. This includes, for example, traceability links, dependency structures, and version relationships. They should clarify the underlying data structures, their semantics, and the mechanisms enabling their exploitation.

---

**HDF.4 -- Views**

Authors must describe how users may interact with models. Do users manipulate models directly, through views, or both? Can views integrate or compose multiple models? Are filtering, projection, and/or aggregation mechanisms supported?

---

**HDF.5 -- Collaboration**

Authors must characterise the collaboration model supported by the solution (e.g., online vs. offline). They should describe the underlying mechanisms for concurrency control, conflict detection, and reconciliation, and justify the chosen design trade-offs.
