# Biomedical ontology pilot — v3

**[Open the Ontology Explorer](https://nguyenphuquang1234567.github.io/biomedical-ontology/ontology_explorer.html)**

**[Grant/patent mapping: actual before-and-after examples](https://nguyenphuquang1234567.github.io/biomedical-ontology/grant_patent_mapping_v3.html)**

Version `0.3.0-draft` extends the five-source clinical/paper ontology with grant and patent metadata. The current draft has **86 classes, 55 object properties, and 84 datatype properties**. The previous public version remains in [versions/v2](versions/v2/README.md).

## What changed

- Added 11 classes for grant awards, funding records, organizations, locations, reported name mentions, grant documents, patents, classification codes, retrieval matches and unverified taxon references.
- Added 14 relationships and 28 attributes, including funding-to-award links, named roles, fiscal year, financial amount, filing date and page count.
- Assigned explicit modeling rules to all 35 previously PARTIAL/GAP columns. Resolving a storage model does not independently verify a source fact.
- Attribute search now includes inheritance: searching Document or Patent also finds label and sourceUrl.

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

No raw grant/patent CSVs or full imported metadata graphs are included. Validation counts describe a local import, not a hosted graph database. Currency, the meaning of the generic patent date, taxonomic namespaces and global person/organization identity are not inferred. Retrieval relevance is not scientific support; no hypotheses, scientific claims or discoveries were extracted in this update.

JSON-LD/Turtle equivalence, metadata term references, funding joins and inherited-attribute search were checked. Full OWL reasoning and SHACL validation remain incomplete. The example.org namespace is provisional. Third-party source terms still apply; no blanket license is assigned to upstream content.

Open ontology_explorer.html directly for offline use. The English explorer links to a Vietnamese update report.
