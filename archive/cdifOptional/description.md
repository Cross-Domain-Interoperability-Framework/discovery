## Base Metadata properties

Defines simple properties included in CDIF discovery metadata for the schema.org implementation of the [Cross Domain Interoperability Framework](https://cross-domain-interoperability-framework.github.io/cdifbook/metadata/schemaorgimplementation.html#implementation-of-metadata-content-items) (CDIF) discovery profile.

### Provenance

At this level, `prov:wasGeneratedBy` uses the simple `generatedBy` pattern from provProperties — activity items accept only string names or `@id` references to instruments/software. The extended `cdifProvActivity` pattern (with structured instruments, agents, temporal bounds, methodology, and action chaining) is introduced at the CDIFcomplete profile level or by domain-specific building blocks (e.g., ddeImagery, xasCore).