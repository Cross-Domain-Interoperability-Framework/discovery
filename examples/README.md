# CDIF Discovery Profile Examples

JSON-LD dataset metadata examples that validate against the [CDIFDiscoveryProfile](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks) schema. All examples declare conformance to both `core/1.0` and `discovery/1.0` via `schema:subjectOf/dcterms:conformsTo`.

## Sources

### GeoCodes harvested records

Real-world dataset metadata harvested from the [EarthCube GeoCodes](https://geocodes.earthcube.org/) catalog. Original JSON-LD was extracted from source landing pages, then converted to CDIF Discovery profile format. The `subjectOf` description in each record documents all conversions applied. Harvesting was done using [`geocodes_harvester.py`](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/geocodes_harvester.py).

| File | Source | Description |
|------|--------|-------------|
| `GeoCodes-bcodmo-dataset.jsonld` | BCO-DMO | Oyster reef organisms, Mission Aransas TX |
| `GeoCodes-borealis-dataset.jsonld` | Borealis | Global permeability (GLHYMPS 2.0), converted from Croissant |
| `GeoCodes-dryad-dataset.jsonld` | Dryad | Gridded GDP and Human Development Index |
| `GeoCodes-earthchem-dataset.jsonld` | EarthChem | Glass analyses by electron microprobe |
| `GeoCodes-hydroshare-dataset.jsonld` | HydroShare | Chlorophyll-a from Konza prairie streams |
| `GeoCodes-ieda-dataset.jsonld` | IEDA | Multibeam sonar data, Pacific Ocean |
| `GeoCodes-opentopography-dataset.jsonld` | OpenTopography | Diablo Canyon LiDAR survey |
| `GeoCodes-pangaea-dataset.jsonld` | PANGAEA | Historical synthetic nitrogen fertiliser |
| `GeoCodes-seanoe-dataset.jsonld` | SEANOE | Argo float data from GDAC |
| `GeoCodes-usap-dataset.jsonld` | USAP | Taylor Dome ice core CO2 |

### Pre-existing CDIF and SOSO examples

Moved from the cdif-core repository because they use Discovery-level properties (spatialCoverage, temporalCoverage, variableMeasured, measurementTechnique).

| File | Origin |
|------|--------|
| `CDIF-aloha-dataset.json` | CDIF HOT/ALOHA example |
| `ESIP-fullDataset.jsonld` | ESIP SOSO full example with provenance |
| `ODIS-aloha-dataset.json` | ODIS HOT/ALOHA example |
| `ODIS-obisData.json` | ODIS OBIS marine biodiversity |
| `ODIS-protectedAreaData.json` | ODIS marine protected area |
| `ODIS-timeSeriesProduct-dataset.json` | ODIS time series product |

## Validation

All 16 examples pass the CDIFDiscoveryProfile JSON Schema:

```bash
python -c "
import json, glob
from jsonschema import Draft202012Validator
schema = json.load(open('path/to/CDIFDiscoveryProfile/resolvedSchema.json'))
for f in sorted(glob.glob('examples/*.json*')):
    doc = json.load(open(f))
    errors = list(Draft202012Validator(schema).iter_errors(doc))
    print(f'{'PASS' if not errors else 'FAIL'} {f}')
"
```

## Querying GeoCodes

To retrieve similar records, POST a SPARQL query to `https://graph.geocodes-aws.earthcube.org/blazegraph/namespace/geocodes_all/sparql`:

```sparql
PREFIX schema: <https://schema.org/>
CONSTRUCT { ?s ?p ?o }
WHERE {
  GRAPH <GRAPH_URN_HERE> { ?s ?p ?o }
}
```

with `Accept: application/ld+json` to get JSON-LD output. See `geocodes_harvester.py` in the validation repo for automated harvesting and CDIF conversion.
