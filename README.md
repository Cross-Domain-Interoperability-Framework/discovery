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

## JSON-LD Framing and Validation

**`FrameAndValidate.py`** frames a JSON-LD document against the Discovery profile schema and optionally validates it:

```bash
# Frame and validate
python FrameAndValidate.py examples/CDIF-aloha-dataset.json --validate

# Frame and save output
python FrameAndValidate.py examples/CDIF-aloha-dataset.json -o framed.json

# Use a different schema
python FrameAndValidate.py input.jsonld --validate --schema my-schema.json
```

The script uses **`CDIFDiscovery-frame.jsonld`** to frame JSON-LD documents into the expected property structure. Context prefixes from the input document are automatically merged into the frame, so domain-specific prefixes work without being pre-declared in the frame.

**Requirements:** `pyld`, `jsonschema` (`pip install pyld jsonschema`)

## SHACL Validation

**`discoveryRules.shacl`** contains self-contained SHACL shapes for validating CDIF Discovery profile instances. This file merges shapes from all composing building blocks (cdifCore, cdifCatalogRecord, definedTerm, variableMeasured, spatialExtent, temporalExtent, qualityMeasure) with the profile-level shapes, so it can be used standalone without referencing other repositories. Source shapes come from [`metadataBuildingBlocks/_sources/`](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks/tree/main/_sources) and should be regenerated when source shapes change.

Additional validation tools are in the [validation repository](https://github.com/Cross-Domain-Interoperability-Framework/validation):
- `validate_conformance.py` — validates JSON-LD against claimed CDIF profiles
- `batch_validate.py` — batch validation across file groups

Legacy hand-written SHACL shapes (CDIF v0.0.1, SOSO, Google Dataset Search) are preserved in `archive/shapegraphs/`.

## Repository structure

```
├── examples/                       CDIF Discovery profile examples (44 validated JSON-LD files)
├── CDIFDiscoveryProfileStructuredSchema.json   JSON Schema for validation
├── CDIFDiscovery-frame.jsonld      JSON-LD frame for document framing
├── FrameAndValidate.py             JSON-LD framing and JSON Schema validation
├── discoveryRules.shacl            Merged SHACL shapes (all composing BBs + profile)
├── API-discovery/                  API representation guidance
├── archive/                        Archived schemas, crosswalks, legacy SHACL shapes
├── images/                         Diagrams
├── CDIF-Discovery-vs-SOSO-comparison.md   Comparison with ESIP Science-on-Schema.org
└── CDIF-metadata-crosswalks-merged.xlsx   Crosswalk mappings
```

## License

This work is dedicated to the public domain under [CC0 1.0 Universal](LICENSE).
