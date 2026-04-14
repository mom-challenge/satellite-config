# Contributing to the Model Management Challenge

## Guidelines for Contributing Artefacts

We welcome contributions of artefacts that represent the same semantics as the existing reference models but in different formalisms or formats. The five reference artefacts and their current formats are:

| Artefact | Reference File | Format |
|---|---|---|
| Parts Catalogue | `catalogue.owl` | OWL 2 / RDF-XML |
| Bill of Materials | `bom.json` | JSON |
| Network Topology | `comm-network.json` | JSON |
| Requirements Specification | `requirements.csv` | CSV |
| Report | `report.md` | Markdown |

For example, you could contribute:

- A Parts Catalogue in a different knowledge representation language (e.g., OML, Turtle, Manchester Syntax, UML/SysML)
- A Bill of Materials in a different structured format (e.g., XML, YAML, TOML)
- A Requirements Specification in a different notation (e.g., ReqIF, SysML requirements)
- A Report in a different format (e.g., HTML, PDF, AsciiDoc)

### Where to place re-implemented artefacts

Re-implemented artefacts should be placed alongside the reference file in the same role-based folder within `v1/`. For example, a Parts Catalogue re-implemented in OML would go in `v1/manufacturer/` next to `catalogue.owl`. Use a distinct file name that makes the format clear (e.g., `catalogue.oml`, `bom.yaml`).

### Semantic equivalence

The semantic reference for each artefact is described in detail in Appendix A of [`challenge.tex`](challenge.tex) and in the [Zenodo archive](https://doi.org/10.5281/zenodo.15285131). When contributing an alternative artefact, please ensure:

1. **Semantic Preservation**
   - Clearly describe how your artefact maintains the same semantics as the reference
   - Explain the mapping between your formalism and the reference format
   - Document any semantic differences or intentional extensions

2. **Model Meaning**
   - Provide a clear explanation of what your model represents
   - Describe the domain concepts and their relationships
   - Explain any assumptions or constraints

3. **Documentation**
   - Include a brief note (inline comment or accompanying README) explaining your formalism choice
   - Provide instructions for any tools or libraries needed to work with your artefact

## Guidelines for Contributing Stories

We welcome new stories that demonstrate additional model management challenges beyond the Engineering Scenarios (ES.1--3) and MoM Tasks (MT.1--5) already described in [README.md](README.md). Your story should follow the same structure:

1. **Artefact Involvement**
   - Clearly identify which of the five artefacts are involved (using the names above)
   - Describe what changes occur to these artefacts
   - Explain how the artefacts interact or depend on each other

2. **Real-world Motivation**
   - Provide a realistic scenario from systems engineering
   - Explain why this scenario is important
   - Describe the stakeholders involved

3. **Model Management Implications**
   - Explain the model management challenges raised by your story
   - Connect it to the MoM dimensions discussed in [SOLUTION.md](SOLUTION.md) (e.g., change impact, views, versioning, collaboration)
   - Describe what new insights it provides beyond the existing scenarios

4. **Relation to Existing Scenarios**
   - Indicate whether your story extends, composes, or is independent of ES.1--3
   - If it introduces new requirements or constraints, specify them clearly

## Submission Process

1. Fork the repository
2. Create a new branch for your contribution
3. Add your artefacts and/or story description
4. Include clear documentation as described above
5. Submit a pull request with:
   - A description of your contribution
   - Explanation of formalism choice (for artefacts)
   - Real-world context (for stories)
   - Any additional resources needed
