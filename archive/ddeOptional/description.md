## DDE Optional Metadata Properties

Non-required DDE-specific properties that complement the base CDIF optional metadata:

- **`schema:alternateName`**: Alternative names or aliases for the resource. Can be a single string or an array of strings.
- **`schema:measurementTechnique`**: Techniques, methods, or instruments used in the measurement or creation of the data. Can be plain text descriptions or `schema:DefinedTerm` values from controlled vocabularies.
- **`schema:keywords`**: Additional keywords beyond the DDE-required vocabulary-constrained keywords. Allows free-text or DefinedTerms from any vocabulary.
- **`schema:additionalType`**: Additional type classifications beyond the required DDE resource type. Can include identifiers from other vocabularies (e.g., schema.org types, Wikidata entities).
