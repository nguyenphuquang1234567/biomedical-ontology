# Biomedical ontology pilot — v3.1

**[Classes, Relationships & Attributes — complete schema catalog](https://nguyenphuquang1234567.github.io/biomedical-ontology/schema/)**

**[Open the Ontology Explorer](https://nguyenphuquang1234567.github.io/biomedical-ontology/ontology_explorer.html)**

**[Grant/patent mapping: actual before-and-after examples](https://nguyenphuquang1234567.github.io/biomedical-ontology/grant_patent_mapping_v3.html)**

Version `0.3.1-draft` extends the clinical, grant and patent ontology with paper extraction: experiments, process steps, claim-to-entity links, and claim support/contradiction relationships. The current draft has **88 classes, 66 object properties, and 86 datatype properties**. The previous public version remains in [versions/v2](versions/v2/README.md).

## What changed in v3.1

- Added `Experiment` and `ProcessStep`, 11 object properties, and 2 datatype properties.
- Added a [compact paper extraction guide](https://nguyenphuquang1234567.github.io/biomedical-ontology/paper-extraction/) with downloadable schema and a synthetic RDF example.
- Added links for process order, inputs/outputs, section attribution, claims about entities, and support/contradiction between claims.
- Schema validation passed; extraction quality on a real paper has not yet been evaluated.

## Earlier v3 changes

- Added 11 classes for grant awards, funding records, organizations, locations, reported name mentions, grant documents, patents, classification codes, retrieval matches and unverified taxon references.
- Added 14 relationships and 28 attributes, including funding-to-award links, named roles, fiscal year, financial amount, filing date and page count.
- Assigned explicit modeling rules to all 35 previously PARTIAL/GAP columns. Resolving a storage model does not independently verify a source fact.
- Attribute search now includes inheritance: searching Document or Patent also finds label and sourceUrl.

## Metadata subgraphs

**[Open the interactive subgraph viewer](https://nguyenphuquang1234567.github.io/biomedical-ontology/subgraphs/)**

**[Browse the RDF subgraphs](subgraphs/README.md)** · **[Download all subgraphs](https://nguyenphuquang1234567.github.io/biomedical-ontology/subgraphs/CSV_Subgraphs.zip)**

The combined graph contains 309,927 triples from all three grant/patent CSVs. Separate Turtle files, a combined Turtle file, JSON-LD and validation are included.

## Files

- [Ontology JSON-LD](ontology.jsonld) / [Turtle](ontology.ttl): the complete v3 ontology.
- [Extension JSON-LD](extension.jsonld) / [Turtle](extension.ttl): the changes from v2.
- [Update explanation (Vietnamese)](Resolution_VI.md).
- [Class catalog](classes.csv), [relationships](relationships.csv), [attributes](attributes.csv).
- [Full source mapping](source_field_mapping.csv): 319 field/structure mapping rows, including the original five sources and 64 new grant/patent columns.
- [Detailed new mapping](resolved_mapping.json): before/after decisions for those 64 columns.
- [Full 64-column mapping table](grant_patent_mapping_v3.html) and [downloadable CSV](full_mapping.csv): searchable by file, field, value, class or relationship.
- [Validation](validation.json): local checks and remaining limits.
- [Original five-source report](Ontology_v2_VI.md) and [earlier exploration](references/Table1_Exploration_VI.md) are retained as historical documentation.

The factual/evidence diagrams summarize selected clinical and paper relationships; they do not show every v3 class. The original examples remain source-grounded v2 examples compatible with the extended ontology.

## Data and verification boundaries

The generated grant/patent metadata graphs are published in subgraphs/. Raw input CSVs are not included. Validation counts describe a local import, not a hosted graph database. Currency, the meaning of the generic patent date, taxonomic namespaces and global person/organization identity are not inferred. Retrieval relevance is not scientific support; no hypotheses, scientific claims or discoveries were extracted in this update.

JSON-LD/Turtle equivalence, metadata term references, funding joins and inherited-attribute search were checked. Full OWL reasoning and SHACL validation remain incomplete. The example.org namespace is provisional. Third-party source terms still apply; no blanket license is assigned to upstream content.

Open ontology_explorer.html directly for offline use. The English explorer links to a Vietnamese update report.

## Paper extraction — v3.1 draft

[Compact guide](https://nguyenphuquang1234567.github.io/biomedical-ontology/paper-extraction/) with schema downloads and a synthetic RDF example. The shared ontology now includes Experiment, ProcessStep and extraction relationships. New terms retain the `https://example.org/nih-pilot/extraction/` namespace.
