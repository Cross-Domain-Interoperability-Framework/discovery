# CDIF Discovery Profile vs ESIP Science on Schema.org (SOSO) Dataset Recommendations

Comparison of the [CDIF Discovery profile](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/bblock/cdif.bbr.metadata.profiles.cdifProfiles.CDIFDiscovery) (cdifCore + cdifOptional) with the [ESIP Science on Schema.org v1.3 Dataset guide](https://github.com/ESIPFed/science-on-schema.org/blob/main/guides/Dataset.md).

### Sources

- **CDIF JSON Schema**: cdifCore [`schema.yaml`](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks/blob/main/_sources/cdifProperties/cdifCore/cdifCoreStructuredSchema.json) + cdifOptional [`schema.yaml`](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks/tree/main/_sources/cdifProperties/cdifOptional) building blocks
- **CDIF Specification**: [Schema.org Implementation of CDIF Metadata](https://cross-domain-interoperability-framework.github.io/cdifbook/metadata/schemaorgimplementation.html)
- **SOSO Guide**: [ESIP Science on Schema.org Dataset.md](https://github.com/ESIPFed/science-on-schema.org/blob/main/guides/Dataset.md)
- **SOSO SHACL**: [soso_common_v1.3.0.ttl](https://github.com/ESIPFed/science-on-schema.org/blob/v1.3-SHACL/validation/shapegraphs/soso_common_v1.3.0.ttl)

Generated 2026-03-19, by Claude; edited by S.M. Richard.

## Summary

CDIF Discovery and SOSO share a common foundation in schema.org vocabulary but differ in scope and strictness:

- **CDIF** is a formal JSON Schema with machine-enforceable constraints, designed for cross-domain interoperability with explicit conformance declaration
- **SOSO** is a guidance document with recommendations, focused on Google Dataset Search discoverability. SOSO v1.3 also provides SHACL validation shapes
- **CDIF requires more fields** at the core level (identifier, dateModified, subjectOf, conditionsOfAccess/license)
- **CDIF separates** CDIF separates metadata-about-metadata (CdifCatalogRecord) from the resource description
- **CDIF supports** a simple data quality scheme.
- **SOSO SHACL is stricter than SOSO guide** — the SHACL shapes enforce `schema:url` (Violation) and `schema:version` (Violation) as required, while the guide lists `url` and `version` as Recommended

## Property-by-Property Comparison

### Legend

| Abbrev | Meaning |
|--------|---------|
| **cdifCore** | CDIF Core building block (required for all CDIF records) |
| **cdifOptional** | CDIF Optional building block (discovery-level extensions) |
| **CDIF Spec** | Obligation from the [CDIF specification](https://cross-domain-interoperability-framework.github.io/cdifbook/metadata/schemaorgimplementation.html) (M=Mandatory, C=Conditional, O=Optional) |
| **SOSO Guide** | Obligation from Dataset.md (Required, Recommended, Optional) |
| **SOSO SHACL** | Enforcement in soso_common_v1.3.0.ttl (V=Violation, W=Warning, I=Info, —=not checked) |

---

### Identity and Type

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `@context` | Implicit (`http://schema.org/`) | Namespace V (rejects `https:`) | M | **cdifCore Required** | SOSO SHACL enforces `http://schema.org/` namespace; CDIF requires explicit context object with `schema`, `dcterms`, `dcat` prefixes |
| `@id` | Not mentioned | **V** (sh:IRI required) | M | **cdifCore Required** | Both SOSO SHACL and CDIF require an IRI identifier |
| `@type` | `schema:Dataset` assumed | — | M | **cdifCore Required** — must contain `schema:Dataset` | CDIF enforces via `contains`; allows additional types |
| `schema:additionalType` | Not mentioned | — | O | **cdifOptional** | CDIF supports typing from external vocabularies |

### Basic Metadata

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:name` | Required | **V** (minCount 1, literal) | M | **cdifCore Required** | All three agree: required |
| `schema:description` | Required | **V** (minCount 1) | O | **cdifOptional** | SOSO requires (guide + SHACL); CDIF treats as optional |
| `schema:url` | Recommended | **V** (minCount 1, maxCount 1) | C | **cdifCore Conditional** | SOSO SHACL requires exactly 1 URL (Violation!); CDIF requires url OR distribution |
| `schema:version` | Recommended | **V** (minCount 1, maxCount 1) | O | **cdifOptional** | SOSO SHACL enforces as Violation; CDIF treats as optional |
| `schema:inLanguage` | Not mentioned | — | — | **cdifOptional** | CDIF only |
| `schema:dateModified` | Optional | — | M | **cdifCore Required** | CDIF promotes to required; neither SOSO guide nor SHACL enforce |
| `schema:datePublished` | Recommended | — | O | **cdifOptional** | Match |
| `schema:dateCreated` | Recommended | — | — | Not included | SOSO only |
| `schema:expires` | Optional | — | — | Not included | SOSO only |

### Identifiers

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:identifier` | Recommended | **V** (minCount 1, literal/URL/PropertyValue) | M | **cdifCore Required** | SOSO SHACL and CDIF both require; PropertyValue pattern recommended |
| `schema:sameAs` | Recommended | **W** (minCount 1, IRIOrLiteral) | O | **cdifOptional** | SOSO SHACL warns if missing; CDIF optional with richer value types |

### Rights and Access

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:license` | Recommended | **V** (minCount 1, CreativeWork/IRI/literal) | C | **cdifCore Conditional** | SOSO SHACL requires; CDIF requires license OR conditionsOfAccess |
| `schema:conditionsOfAccess` | Not mentioned | — | C | **cdifCore Conditional** | CDIF only — alternative to license |
| `schema:isAccessibleForFree` | Recommended | **W** (minCount 1, boolean) | — | Not included | SOSO only; SHACL warns if missing |
| `schema:publishingPrinciples` | Not mentioned | — | O | **cdifOptional** | CDIF only — maintenance/update policies |

### Keywords and Subject

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:keywords` | Recommended | **W** (minCount 1, literal/DefinedTerm) | O | **cdifOptional** | SOSO SHACL warns; CDIF recommends DefinedTerm for vocabulary-sourced keywords |
| `schema:measurementTechnique` | Not mentioned | — | O | **cdifOptional** | CDIF only |

### Agents (People and Organizations)

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:creator` | Recommended | — | O | **cdifOptional** — `@list` wrapper | CDIF uses JSON-LD `@list` for order; SOSO uses `schema:Role` |
| `schema:contributor` | Optional | — | O | **cdifOptional** — with Role support | CDIF adds agentInRole BB |
| `schema:publisher` | Recommended | — | O | **cdifOptional** | Match |
| `schema:provider` | Recommended | — | O | **cdifOptional** (array) | Match |
| `schema:funding` | Optional | — | O | **cdifOptional** — Funder (MonetaryGrant) | Match |

### Coverage (Spatial and Temporal)

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:spatialCoverage` | Recommended | — | O | **cdifOptional** — array of SpatialExtent | CDIF adds geosparql:asWKT geometry support |
| `schema:temporalCoverage` | Recommended | — | O | **cdifOptional** — array of TemporalExtent | CDIF supports structured time objects + string intervals |

### Variables

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:variableMeasured` | Recommended | **W** (minCount 1, class PropertyValue) | O | **cdifOptional** — VariableMeasured or StatisticalVariable | SOSO SHACL warns; CDIF adds StatisticalVariable support |

### Distribution and Access

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:distribution` | Recommended | — | C | **cdifCore Conditional** — DataDownload or WebAPI | CDIF requires url OR distribution; adds WebAPI type |
| `schema:potentialAction` | Optional | — | — | Nested inside WebAPI distribution | CDIF handles via WebAPI BB, not at root |
| `spdx:checksum` | Optional | — | O | On DataDownload | Both support checksums on distributions |

### Provenance

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `prov:wasGeneratedBy` | Optional | — | O | **cdifOptional** — GeneratedBy (prov:Activity) | CDIF provides structured activity schema |
| `prov:wasDerivedFrom` | Optional | — | O | **cdifOptional** — DerivedFrom | Match |
| `prov:wasRevisionOf` | Optional | — | — | Not included | SOSO only |

### Data Quality

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `dqv:hasQualityMeasurement` | Not mentioned | — | O | **cdifOptional** — QualityMeasure | CDIF only — W3C DQV |

### Catalog and Metadata Records

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:subjectOf` | Optional (alt metadata links) | — | M | **cdifCore Required** — CdifCatalogRecord | Major difference: CDIF requires nested catalog record with conformance |
| `schema:includedInDataCatalog` | Optional | — | O | On CdifCatalogRecord | CDIF nests inside catalog record |
| `schema:about` | Optional (inverse link) | — | M (on catalog record) | On CdifCatalogRecord (required) | Points back to root `@id` |
| `dcterms:conformsTo` | Not mentioned | — | M (on catalog record) | Must include `core/1.0/` + `discovery/1.0/` | CDIF only — profile conformance |

### Related Resources

| Property | SOSO Guide | SOSO SHACL | CDIF Spec | CDIF Schema | Notes |
|----------|-----------|------------|-----------|-------------|-------|
| `schema:citation` | Optional | — | — | Not included | SOSO only |
| `schema:relatedLink` | Not mentioned | — | O | **cdifOptional** — LinkRole with target | CDIF only — typed relationships |

## Validation Approach Comparison

### SOSO SHACL (soso_common_v1.3.0.ttl)

The SOSO SHACL shapes target `schema:Dataset` nodes and enforce:

| Severity | Properties |
|----------|-----------|
| **Violation** | `@id` (must be IRI), `schema:name`, `schema:description`, `schema:identifier`, `schema:url` (exactly 1), `schema:version` (exactly 1), `schema:license` |
| **Warning** | `schema:isAccessibleForFree`, `schema:keywords`, `schema:sameAs`, `schema:variableMeasured` |
| **Namespace** | Rejects `https://schema.org/` (must use `http://schema.org/`) |

Notable: the SHACL is **stricter than the guide** — `schema:url`, `schema:version`, and `schema:description` are enforced as Violations even though the guide lists them as Recommended. `schema:url` has `maxCount 1`.

The SOSO SHACL also validates `schema:PropertyValue` nodes (used in `variableMeasured`), requiring either `schema:name` or `schema:propertyID`.

### CDIF SHACL

CDIF uses per-building-block SHACL shapes that validate:
- **Catalog record structure** (cdifCatalogRecord): `dcterms:conformsTo` must include `core/1.0/` URI
- **Conformance URIs** (per-profile): each profile shape checks `schema:subjectOf → dcterms:conformsTo` for its specific URI
- **DataDownload** requires `schema:contentUrl`
- **Person/Organization** shapes recommend `contactPoint`, `identifier`, `name`
- **Instrument** shapes recommend `schema:category` classification
- **Action** shapes (for WebAPI) require `schema:target` with `urlTemplate`

CDIF SHACL shapes use SPARQL targets to select the correct nodes (root Dataset, catalog record, instruments, etc.) and distinguish between context-dependent validation (e.g., provenance Activities vs WebAPI Actions).

### Key Differences

| Aspect | SOSO SHACL | CDIF SHACL |
|--------|-----------|------------|
| **Scope** | Single shape for Dataset + PropertyValue | Multiple shapes per building block |
| **Target** | `sh:targetClass schema:Dataset` | SPARQL-based targets (root Dataset, catalog record, instruments) |
| **Conformance** | No conformance checking | Required `dcterms:conformsTo` with specific URIs |
| **Strictness** | `url` and `version` as Violations | `url` conditional (OR distribution); `version` optional |
| **Namespace** | Enforces `http://schema.org/` | Enforces via `@context` JSON Schema constraint |
| **Composability** | Monolithic | Modular — shapes compose via profile inheritance |

## Key Architectural Differences

### 1. Conformance Declaration
CDIF requires every record to include a `schema:subjectOf` node (CdifCatalogRecord) that declares which CDIF profiles the record conforms to via `dcterms:conformsTo`. This enables machine agents to determine how to process the record. SOSO has no equivalent mechanism.

### 2. Conditional Requirements
CDIF uses `anyOf` constraints for conditional requirements:
- Must have `schema:license` OR `schema:conditionsOfAccess`
- Must have `schema:url` OR `schema:distribution`

SOSO lists these as independent recommendations without enforcement.

### 3. Distribution Types
CDIF supports two distribution types: `DataDownload` (file-based) and `WebAPI` (service-based, with `potentialAction` for parameterized access). SOSO handles service access via `schema:potentialAction` at the root level.

### 4. JSON-LD Structure
CDIF requires explicit `@context` with namespace prefixes (`schema:`, `dcterms:`, `dcat:`) and uses `schema:` prefixed property names throughout. SOSO examples use the bare `https://schema.org/` context and un-prefixed property names.

### 5. Author Ordering
CDIF uses the JSON-LD `@list` construct on `schema:creator` to preserve author order in RDF serialization. SOSO recommends `schema:Role` with `schema:author` for ordered authorship.

### 6. Vocabulary-backed Keywords
CDIF encourages `schema:DefinedTerm` for keywords with vocabulary references (`inDefinedTermSet`, `termCode`). SOSO supports this but emphasizes plain text keywords for Google Dataset Search compatibility.

## Coverage Summary

| Category | SOSO properties | In CDIF | CDIF-only properties |
|----------|----------------|---------|---------------------|
| Required (SOSO guide) | 2 | 2/2 (100%) | — |
| Required (SOSO SHACL Violation) | 7 | 5/7 (71%) | — |
| Recommended (SOSO guide) | 16 | 14/16 (88%) | — |
| Warned (SOSO SHACL) | 4 | 3/4 (75%) | — |
| Optional (SOSO guide) | 13 | 8/13 (62%) | — |
| CDIF Spec Mandatory | 8 | 8/8 (100%) | — |
| CDIF-only | — | — | 12 |

**SOSO SHACL Violations not enforced by CDIF:** `schema:url` (CDIF makes conditional), `schema:version` (CDIF optional)

**SOSO properties not in CDIF Discovery:** `schema:isAccessibleForFree`, `schema:dateCreated`, `schema:expires`, `schema:citation`, `prov:wasRevisionOf`

**CDIF properties not in SOSO:** `@context` (required), `@id` (required), `schema:dateModified` (required), `schema:subjectOf/CdifCatalogRecord` (required), `dcterms:conformsTo`, `schema:conditionsOfAccess`, `schema:additionalType`, `schema:inLanguage`, `schema:relatedLink`, `schema:publishingPrinciples`, `schema:measurementTechnique`, `dqv:hasQualityMeasurement`

**Properties where SOSO SHACL is stricter than CDIF:**
- `schema:url` — SOSO SHACL: Violation, exactly 1 required; CDIF: conditional (url OR distribution)
- `schema:version` — SOSO SHACL: Violation, exactly 1 required; CDIF: optional
- `schema:description` — SOSO SHACL: Violation, required; CDIF: optional (in cdifOptional, not cdifCore)

**Properties where CDIF is stricter than SOSO:**
- `schema:dateModified` — CDIF: required; SOSO: optional (not in SHACL)
- `schema:identifier` — CDIF: required; SOSO guide: recommended (SHACL: Violation — aligned)
- `schema:subjectOf` — CDIF: required catalog record with conformance; SOSO: optional metadata links
- `schema:license/conditionsOfAccess` — CDIF: at least one required; SOSO: license recommended (SHACL: Violation)
