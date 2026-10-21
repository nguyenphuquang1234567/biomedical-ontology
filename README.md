# Biomedical ontology pilot

Draft ontology based on the union of concepts found in five Table 1 sources: Dryad HCT BAL, PRJNA984933, PRJNA808886, PRJNA1056765, and PMC Open Access.

## Start here

Download or clone this repository, then open **[ontology_explorer.html](ontology_explorer.html)** in a browser. It works offline and requires no installation. The explorer is in English; the detailed report and some ontology annotations remain in Vietnamese.

- [Detailed report (Vietnamese)](Ontology_v2_VI.md): definitions, hierarchy, attributes, relationships, source mapping, examples, and presentation outline.
- [Ontology in JSON-LD](ontology.jsonld) and [Turtle](ontology.ttl): two RDF serializations of the same ontology.
- [Factual diagram](factual_diagram.svg) and [evidence diagram](evidence_diagram.svg): compact relationship diagrams; Mermaid sources and PNGs are included.
- [Class catalog](classes.csv), [relationships](relationships.csv), [attributes](attributes.csv), and [source field mapping](source_field_mapping.csv).
- [Examples](examples.jsonld): two source-grounded instance examples, separate from class definitions.
- [Validation record](validation.json) and `*.rq`: format checks and example SPARQL queries.
- [Original five-source exploration](references/Table1_Exploration_VI.md) and its quantitative evidence are in `references/`.

## Scope and interpretation

The draft has 75 classes, including subclasses and proposed technical structures; 41 object properties; 56 datatype properties; and 255 source field/structure mapping rows. Of these mapping rows, 90 retain raw metadata rather than fully normalized semantics.

The factual and evidence diagrams show selected relationships, not every class or subclass. For the full hierarchy, consult `classes.csv` or `class_hierarchy.mmd`. Class inheritance does not mean that a separate database table is required for every class.

Source-field mapping is specific to known source structures. It is not a universal extractor for new PDFs. Assessments are linked to particular results only when the source documents that relationship.

## Status

This is a research design draft, not a fully populated knowledge graph or production database. JSON-LD and Turtle were parsed and compared; example queries were checked. Full OWL reasoning, SHACL validation, full ETL, and full-corpus claim extraction have not been completed. The `example.org` namespace is provisional. Paper/concept mapping templates are empty by design.

Raw datasets, private meeting notes, access tokens, and machine-specific absolute paths are not included. Paths under `data/` in examples and the input manifest refer to separately obtained source files; `data/` is ignored by Git. No blanket license is assigned to upstream third-party material; retain each source's terms and attribution.
