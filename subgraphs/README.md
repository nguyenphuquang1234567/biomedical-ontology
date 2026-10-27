# CSV metadata subgraphs

**[Download the complete bundle](https://nguyenphuquang1234567.github.io/biomedical-ontology/subgraphs/CSV_Subgraphs.zip)**

687 grant awards, 2,426 funding records and 72 patent records were mapped using ontology v3. These RDF graphs contain 309,927 unique triples.

## Choose an import

- **[all_subgraphs.ttl](all_subgraphs.ttl)**: the complete combined RDF graph, ready for an RDF triplestore import.
- **[all_subgraphs.jsonld](all_subgraphs.jsonld)**: the same graph in JSON-LD; choose this or Turtle, not both.
- [stg_grant_award.ttl](stg_grant_award.ttl), [stg_grant_funding.ttl](stg_grant_funding.ttl), [patent_evidence.ttl](patent_evidence.ttl): separate source subgraphs. Funding links reference award entities defined in the award subgraph. Their union exactly matches the combined graph.
- [ontology_v3.ttl](ontology_v3.ttl): ontology definitions, separate from instance data.
- [Validation](validation.json) and [Vietnamese instructions](README_VI.md).

The bundle is an export of the validated local package; its validation file records the state at packaging time. This folder's validation record additionally marks publication.

These are source-reported metadata, not extracted scientific claims. Source uncertainty in currency, patent date meaning, taxonomic namespaces and real-world identities remains explicit. No claim or hypothesis extraction was performed. No graph database has been created or populated. Raw input CSVs are not bundled.
