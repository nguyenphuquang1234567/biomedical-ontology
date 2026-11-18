# Document graphs with reviewed local concept links

The paper and grant now share local concept IRIs for Ebola virus and its glycoprotein. The grant hypothesis references Ebola and integrin with ext:aboutEntity. Research tasks reference their subjects with ext:taskSubject, added to the shared schema for ResearchTask → Entity.

The patent retains its independently sourced two-step workflow. No cross-document patent link was established from the extracted passage.

The concept IRIs are local integration identifiers. mappingStatus remains unresolved for correspondence to the team's fact-layer entities and for precise strain/sequence scope. Request those identifiers from the integration team before treating these concepts as resolved fact-layer entities.

The GP2-region-to-grant-aim proposal remains unresolved. No grant-to-paper funding relationship is asserted.

The paper graph preserves Insiya's extraction, including the support/contradiction assertions that still require source review. The validation performed here covers RDF round-trips and the new grant links against the schema, not scientific verification of every paper claim.

Each document remains available separately as Turtle and JSON-LD. merged contains the union. link-review.csv records seven proposal decisions. original-proposed-links.jsonld preserves the received proposal.

The grant source remains the abstract in stg_grant_funding.csv, appl_id 2564563. Existing SourceLocators identify the relevant passages.
