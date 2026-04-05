# CDIF Discovery

This repository contains the specification, examples, validation tools, and documentation for the **Cross-Domain Interoperability Framework (CDIF) Discovery** metadata profile. The Discovery profile defines metadata content requirements for making digital resources (datasets, documents, software, services, etc.) findable, indexable, and cataloguable across domains.

## Specification documents

- **[cdifDiscoveryProfile.md](cdifDiscoveryProfile.md)** — The core specification defining required, conditional, and recommended metadata elements for CDIF discovery.
- **[discoverability.md](discoverability.md)** — Extended guide on metadata content requirements and implementation approaches.
- **[CDIF_Data_Classes_and_Properties.md](CDIF_Data_Classes_and_Properties.md)** — DDI-CDI metadata classes and properties for data structure documentation and integration.

## Key metadata elements

**Required:** Resource Identifier, Title, Distribution, Rights/License, Metadata Profile Identifier, Resource Type.

**Conditional:** Variable descriptions (datasets), Temporal Coverage, Geographic Extent.

**Recommended:** Description, Creator, Modification Date, Keywords, Funding, Related Resources, Version, Provenance, Data Quality.

The profile builds on Dublin Core, schema.org, ISO 19115-1, DCAT, DDI-CDI, and FDO Kernel Attributes.

## Repository structure

```
├── examples/               CDIF Discovery profile examples (16 validated JSON-LD files)
│                           Includes GeoCodes-harvested records and converted SOSO/ODIS examples
├── API-discovery/          API representation guidance (potentialAction, SearchAction, EntryPoint)
├── archive/                Archived schemas, historical crosswalks, legacy serialization examples,
│                           and superseded SHACL shapes (now generated in the validation repo)
├── CDIFDiscoveryProfile-rules.shacl   Current SHACL shapes (synced from metadataBuildingBlocks)
├── archive/                Archived schemas, crosswalks, legacy serialization and SHACL shapes
├── images/                 Diagrams (harvesting workflows, metadata embedding, digital object overview)
├── CDIF-Discovery-vs-SOSO-comparison.md   Comparison with ESIP Science-on-Schema.org
├── CDIF-metadata-crosswalks-merged.xlsx   Crosswalk mappings to DCAT, ISO 19115, DataCite, EML, etc.
├── CDIFCoreClasses.docx                   Core metadata classes documentation
├── CDIFDiscoveryClasses.docx              Discovery metadata classes documentation
├── CDIFDataDescriptionClasses.docx        Data description classes documentation
├── gen_datadesc_doc.py                    Generates CDIFDataDescriptionClasses.docx from CDIFDiscoveryClasses.docx
├── generate_property_table.py             Extracts property tables from YAML schema building blocks
└── merge_crosswalks.py                    Consolidates crosswalk Excel files into the merged spreadsheet
```

## Validation

**`CDIFDiscoveryProfile-rules.shacl`** contains the current SHACL shapes for the CDIF Discovery profile. This file is copied from [`metadataBuildingBlocks/_sources/profiles/cdifProfiles/CDIFDiscoveryProfile/rules.shacl`](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks/blob/main/_sources/profiles/cdifProfiles/CDIFDiscoveryProfile/rules.shacl) and should be updated whenever the source changes.

Full validation tools (JSON Schema framing, batch validation, composite SHACL shapes) are in the [validation repository](https://github.com/Cross-Domain-Interoperability-Framework/validation). Legacy hand-written SHACL shapes (CDIF v0.0.1, SOSO, Google Dataset Search) are preserved in `archive/shapegraphs/`.

## License

This work is dedicated to the public domain under [CC0 1.0 Universal](LICENSE).
