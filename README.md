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

## Examples

The `examples/` directory contains 26+ validated JSON-LD dataset examples that conform to the CDIF Discovery profile. Sources include:

- **GeoCodes** — 10 records harvested from EarthCube GeoCodes (BCO-DMO, PANGAEA, EarthChem, SEANOE, etc.)
- **NCEI NOAA** — 7 records (GHCN Daily, Global Temperature, Sea Surface Temperature, etc.)
- **Copernicus CDS** — 3 records (ERA5 reanalysis, sea level, sea ice)
- **Dataverse** — 13 records from Harvard Dataverse and Borealis (hydrology, ecology, remote sensing, etc.)
- **Pre-existing CDIF/ESIP/ODIS** — 6 converted reference examples

All examples declare `conformsTo` for both `core/1.0` and `discovery/1.0` and pass CDIFDiscoveryProfile JSON Schema validation. See [examples/README.md](examples/README.md) for details.

## Repository structure

```
├── examples/               CDIF Discovery profile examples (26+ validated JSON-LD files)
├── CDIFDiscoveryProfile-rules.shacl   Current SHACL shapes (synced from metadataBuildingBlocks)
├── API-discovery/          API representation guidance (potentialAction, SearchAction, EntryPoint)
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

Full validation tools (JSON Schema framing, batch validation, composite SHACL shapes) are in the [validation repository](https://github.com/Cross-Domain-Interoperability-Framework/validation):
- `validate_conformance.py` — validates JSON-LD against claimed CDIF profiles
- `geocodes_harvester.py` — harvests and converts metadata from GeoCodes and other repositories
- `FrameAndValidate.py` — JSON-LD framing and JSON Schema validation
- `batch_validate.py` — batch validation across file groups

Legacy hand-written SHACL shapes (CDIF v0.0.1, SOSO, Google Dataset Search) are preserved in `archive/shapegraphs/`.

## License

This work is dedicated to the public domain under [CC0 1.0 Universal](LICENSE).
