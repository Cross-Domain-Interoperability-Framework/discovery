---
title: CDIF Discovery Profile Classes and Properties
date: 2026-04-24
---

# Introduction

This document includes all of the classes and properties for the CDIF Discovery profile implementation using the schema.org vocabulary. The profile consists of 1) a set of classes and properties containing content that is required in every valid CDIF discovery profile instance; and 2) a set of classes and properties that are expected to be applicable to a wide variety of datasets across knowledge domains. Although classes and properties in the second group are optional, this profile provides recommendations for implementation that will enable interoperability. As used here, classes are named objects with a set of named properties, each of which has an expected data type that specifies its value. Datatypes can be simple literals (number, string, boolean), references to objects that are not serialized inline, or instances of a class. Conformance to the profile entails populating all mandatory content, and using the recommended classes and properties for the optional content. The implementation target is an rdf serialization, which is an open world logical model; users are thus free to add additional properties that they find useful for dataset documentation in their community, but these can be ignored by other users without penalty.

The JSON syntax is defined by the [ECMA JSON specification](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/), and JSON-LD is specified in the [JSON-LD 1.1 recommendation](https://www.w3.org/TR/json-ld11/) from the World Wide Web Consortium (W3C). This serialization is designed for linked data applications that will translate the JSON into a set of {subject, predicate, object} triples that can be loaded into an RDF database for processing. The JSON-LD context binds JSON keys to URIs for more precise semantics, and the use of URIs to identify entities and property values in the metadata will maximize the linkage with resources on the wider web to build an ever-expanding global knowledge graph.

# Model

The information model for the base discovery profile is defined in the [cdifBook](https://cross-domain-interoperability-framework.github.io/cdifbook/metadata/contentmodel.html#basic-discovery-metadata-content-model). This section outlines a logical model focused on a JSON-LD implementation of the content items defined in the information model. All classes and properties are implemented with schema.org types and attributes unless there is a prefix indicating use of elements from other vocabularies. See the context section for prefixes used and their mapping to URIs.

## Base Class

### DataSet {#sec-dataset}

This profile applies to description of resources that can be described using the properties defined in the [CDIF discovery information model](https://cross-domain-interoperability-framework.github.io/cdifbook/metadata/contentmodel.html#basic-discovery-metadata-content-model) . For implementation using the schema.org vocabulary, these are typed as schema:Dataset.

#### Required Properties from cdif Core profile

#### @id

**Cardinality:** Required

**Content:** string

**Description:** This is an identifier for this node in an rdf graph. JSON-LD key is \@id.


#### @type

**Cardinality:** Required -- \"Dataset\", Repeatable

**Content:** string.uri

**Description:** The type property specifies the rdf:type classification. For this implementation, the type is represented with the JSON-LD \@type property, and must include \'Dataset\'. JSON-LD key is \@type. Type assertions here should be understood to imply the usage of properties associated with the identified type, whether from schema.org or other vocabularies that might define the type.


#### name

**Cardinality:** Required

**Content:** string

**Description:** A descriptive name of a dataset (e.g., \'Snow depth in Northern Hemisphere\'). The name should uniquely identify the described resource for human use, in the scope of the metadata catalog containing this metadata record. Schema.org property, in namepace \'http://schema.org/\'.


#### @identifier

**Cardinality:** Required

**Content:** string.uri or [PropertyValue-(identifier)](#sec-propertyvalue-id)

**Description:** The primary identifier for the described resource; other identifiers should be listed in the sameAs field. CDIF recommends that if the identifier is a resolvable URI, use the string option; if the identifier is a string that is not a resolvable URI, use the schema:PropertyValue class to provide context for interpreting the identifier. Schema.org property, in namepace \'http://schema.org/\'.


#### dateModified

**Cardinality:** Required

**Content:** string, ISO 8601 format

**Description:** ISO8601 formatted date (and optional time if relevant) when Dataset was last updated


**CHOICE at least one of two options:**

#### conditionsOfAccess

**Cardinality:** Required if no license, Repeatable

**Content:** string, [object reference](#object-reference), or [LabeledLink](#sec-labeledlink)

**Description:** Text statement of conditions for use and access; if an online resource documents the restrictions or a URI is used to identify the conditions, recommend using the LabeledLink option, implemented as schema:CreativeWork, to provide a label (name) and an identifier (URI or URL).


#### license

**Cardinality:** Required if no conditionsOfAccess

**Content:** string, [object reference](#object-reference), or [LabeledLink](#sec-labeledlink)

**Description:** Legal statement of conditions for use and access; recommend using the [LabeledLink](#sec-labeledlink) option, implemented by schema:CreativeWork to provide a label (name) for the license, and an identifier. Sources of license identifiers: https://opensource.org/licenses/, https://creativecommons.org/about/cclicenses/, https://spdx.org/licenses/, http://cor.esipfed.org/ont/earthcube/swl. If only a string is provided, it should be recognizable name for the license. If resolvable URI is available, use the object reference.


**CHOICE at least one of two options:**

#### url

**Cardinality:** Required if no distribution

**Content:** string.uri

**Description:** Web Location of a page describing the dataset (landing page), typically providing links or instructions to get the actual resource content; analogous to dcat:accessURL. If a direct link is available to get the data, put in distribution/DataDownload/contentUrl


#### distribution

**Cardinality:** Required if no url

**Content:** [DataDownload](#sec-datadownload) or [WebAPI](#sec-webapi)

**Description:** specifies how to download the data in a specific format or access via a web API. This property describes where to get the data and in what format by using the schema:DataDownload type. If user must access data through a landing page, provide link to landing page in the \'url\' property for the dataset, not a distribution contentUrl.


#### subjectOf

**Cardinality:** Required

**Content:** [Dataset/dcat:CatalogRecord](#sec-catalogrecord)

**Description:** This property contains information about the metadata record itself, as opposed to the resource the record describes. See Uses of dcat:CatalogRecord and https://github.com/Cross-Domain-Interoperability-Framework/Discovery/issues/13 for discussion on how to make assertion about the metadata record distinct from statements about the described resource. Use the dcat:CatalogRecord as additionalType to distinguish this schema:Dataset from the schema:Dataset about a described external resource. see <https://cross-domain-interoperability-framework.github.io/cdifbook/metadata/contentmodel.html#properties-for-metadata-management>. Introduction of this is novel for schema.org implementations.


#### Optional properties from CDIF Core

#### description

**Cardinality:** Optional

**Content:** string

**Description:** Abstract describing the content, format, origin, quality or any other aspects of the resource that might be useful to future users evaluating the resource for usage.


#### additionalType

**Cardinality:** Optional, Repeatable

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** Use to assert semantics for the JSON object using concepts from other vocabularies. Type assertions here are purely for semantic information, and do not imply presence of properties assigned to a class in some other vocabulary.


#### sameAs

**Cardinality:** Optional, Repeatable

**Content:** string, [object reference](#object-reference), or PropertyValue

**Description:** Other identifiers for the dataset, as IRI references, literal strings, or structured identifiers using schema:PropertyValue.


#### version

**Cardinality:** Optional

**Content:** string or number

**Description:** The version number or identifier for this dataset (text or numeric). The values should sort from oldest to newest using an alphanumeric sort on version strings


#### inLanguage

**Cardinality:** Optional

**Content:** string

**Description:** The language of the dataset content. Use [ISO 639 code](https://www.loc.gov/standards/iso639-2/php/code_list.php) for language or language:locale


#### datePublished

**Cardinality:** Optional

**Content:** string, [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601)

**Description:** ISO8601 formatted date (and optional time if relevant) when Dataset was made public.


#### relatedLink

**Cardinality:** Optional, Repeatable

**Content:** [LinkRole](#sec-linkrole)

**Description:** links to related resources; linkRelationship specifies how the resource is related. Use schema.org LinkRole type for values, with a linkRelationship and target that documents the url and encoding format of the linked content.


#### publishingPrinciples

**Cardinality:** Optional, Repeatable

**Content:** string, [object reference](#object-reference), or [LabeledLink](#sec-labeledlink)

**Description:** Policies related to maintenance, update, expected time to live, e.g. FDOF digitalObjectMutability, RDA digitalObjectPolicy, FDOF PersistencyPolicy. If an online resource documents the policies or a URI is used to identify the conditions, recommend using [LabeledLink](#sec-labeledlink), implemented as schema:CreativeWork to provide a label (name) and an identifier (URI or URL).


#### keywords

**Cardinality:** Optional, Repeatable

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** Keywords are an array of strings, an array of schema:DefinedTerms, or some combination of these. If you have information about a controlled vocabulary from which keywords come from, use schema:DefinedTerm to descibe that keyword. This allowed variability complicates parsing the metadata record; recommend using DefinedTerm for all keywords if any of them are from a known vocabulary, otherwise an array of strings.


#### creator

**Cardinality:** Optional, Repeatable

**Content:** List of [object reference](#object-reference), [Person](#sec-person), or [Organization](#sec-organization)

**Description:** Author or orginator of intellectual content of dataset. Use the JSON-LD \@list construct to preserve author order. Use contributor with the Role property to specify other roles related to creation or stewardship of the resource.


#### contributor

**Cardinality:** Optional, Repeatable

**Content:** [object reference](#object-reference), [Person](#sec-person), or [Organization](#sec-organization)

**Description:** Other parties who played a role in production of dataset


#### publisher

**Cardinality:** Optional

**Content:** [object reference](#object-reference), [Person](#sec-person), or [Organization](#sec-organization)

**Description:** Identify Party who made the dataset publicly available


#### provider

**Cardinality:** Optional, Repeatable

**Content:** [object reference](#object-reference), [Person](#sec-person), or [Organization](#sec-organization)

**Description:** Party who maintains the distribution options for the dataset (i.e. the hosting web server). If there are multiple distributions from different providers, use the provider property on distribution/DataDownload. Contact information for the provider is important if there are malfunctions in the data access workflow.


#### funding

**Cardinality:** Optional, Repeatable

**Content:** [MonetaryGrant](#sec-monetarygrant)

**Description:** Acknowledgement for sources of financial or other material resources important for the creation of the described resource. Allows identification of specific funding instruments (grants, contracts, scholarships...) or institutions providing resources.


#### prov: wasGeneratedBy

**Cardinality:** Optional, Repeatable

**Content:** prov:Activity/prov:used

**Description:** For Discovery profile provide brief information about instruments, software or experimental protocols used, using the value of prov:used either a string or [object reference](#object-reference).


#### prov: wasDerivedFrom

**Cardinality:** Optional, Repeatable

**Content:** string, [object reference](#object-reference), [LabeledLink](#sec-labeledlink)

**Description:** Brief information about sources of data used in aggregate datasets. String bibliographic citations, URIs as object references, or LabeledLink, implemented as schema:CreativeWork, to provide a title, description and URL.


#### Properties added in Discovery Profile

#### measurementTechnique

**Cardinality:** Optional, Repeatable

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** The technique, technology, or methodology used for measurement or determination of the dataset values.


#### variableMeasured

**Cardinality:** Optional, Repeatable

**Content:** [PropertyValue-(variableMeasured)](#sec-propertyvalue-vm)

**Description:** Use schema:PropertyValue to describe the variables assigned values in the dataset. Provide names, labels used in the data serialization, definitions, propertyID as a link to formal property definitions. Min and max values that occur in numeric data to support search criteria based on observed values.


#### spatialCoverage

**Cardinality:** Optional, Repeatable

**Content:** [Place](#sec-place)

**Description:** Document spatial extent to which the resource content is relevant. Can be expressed with a simple text place name, a place name from an identified gazeteer (using schema: [DefinedTerm](#sec-definedterm)), a point location, a bounding box (.e.g. for a map extent), a line (e.g. a ship track or foot traverse), or a general geometry. Registered place names from a gazeteer or a simple bounding box are widely recognized and indexed approaches used by spatially aware metadata aggregators.


#### temporalCoverage

**Cardinality:** Optional, Repeatable

**Content:** string or [ProperInterval](#sec-properinterval)

**Description:** The time interval during which data was collected or observations were made; or a time period that an activity or collection is linked to intellectually or thematically (for example, 1997 to 1998; the 18th century) (see https://documentation.ardc.edu.au/display/DOC/Temporal+coverage). For documentation of Earth Science, Paleobiology or Paleontology datasets, we are interested in the second case\-- the time period that data are linked to thematically. NOTE---the implementation of temporal intervals uses OWL Time, so the context must include \"time\": [http://www.w3.org/2006/time#](http://www.w3.org/2006/time). Simple ISO8601 time intervals can be represented using the description property with a text string value.


#### dqv:hasQualityMeasurement

**Cardinality:** Optional, Repeatable

**Content:** [dqv:QualityMeasurement](#sec-qualitymeasurement)

**Description:** Quality measurements reported to assess the resource. Reported with a measurement type, specified by name, an [object reference](#object-reference) or as a [DefinedTerm](#sec-definedterm), and the reported result of the quality measure, either as a string or a [DefinedTerm](#sec-definedterm) from a vocabulary.


## Other Classes used for CDIF Core

### Data Download {#sec-datadownload}

file-based access to a resource via URL; the DataDownload object provides a link to get the resource content, along with information about the serialization format and conventions used.

#### @id

**Cardinality:** Optional

**Content:** string:uri

**Description:** Graph node identifiers are only necessary if the node content will be referenced in other places


#### @type

**Cardinality:** Required -- \'DataDownload\', other types optional

**Content:** string.uri

**Description:** This is the rdf:type.


#### contentUrl

**Cardinality:** Required

**Content:** string.uri

**Description:** Expected to be an http uri that will directly GET the content of the resource described by this metadata record, in the format specified by the encodingFormat property, and conforming to any specifications identified in the dcterms:conformsTo property.


#### name

**Cardinality:** Optional

**Content:** string

**Description:** String to identify this download option in user interface


#### description

**Cardinality:** Optional

**Content:** string

**Description:** string providing information to document this download option in user interface


#### encodingFormat

**Cardinality:** Optional, Repeatable

**Content:** string:MIME Type

**Description:** Identifier for format from a registry


#### spdx:checksum

**Cardinality:** Optional

**Content:** [spdx:Checksum](#sec-spdxchecksum)

**Description:** Checksum string that is \'footprint\' of the described file to enable testing for file modification. Algorithm used is specified by spdx:algorithm property.


#### dcterms:conformsTo

**Cardinality:** Optional, Repeatable

**Content:** [object reference](#object-reference)

**Description:** An identifier for a specification that the distribution conforms to. Recommended to enable machine-actionable data access. The target download might conform to more that one profile specification.


#### provider

**Cardinality:** Optional, Repeatable

**Content:** [object reference](#object-reference), [Person](#sec-person), or [Organization](#sec-organization)

**Description:** The agent responsible for acces to the described resource. Use contact for this agent to report access problems.


### Web API {#sec-webapi}

Provides information to request data through a web accessible service endpoint. This implementation uses the schema.org Action to document url or url template and parameters. At this point, schema is set up for one action\-- an HTTP Get that requests data. The url template parameters (in curly brackets \'{}\') specify query parameters to filter the source data, request particular output formats or other options offered by the interface.

#### @type

**Cardinality:** Required -- \'WebAPI\', other types optional

**Content:** string.uri

**Description:** This is the rdf:type.


#### serviceType

**Cardinality:** Required

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** Specify the kind of service. Ideally this should be a resolvable identifier. Currently there is no widely adopted registry for serviceType identifiers. Services might be defined at different levels of granularity, and classifications might focus on function, data formats, thematic content, security, or other aspects of the service definition. For interoperability, there must be an external arrangement between data providers and consumers on the strings that will be used to specify service types.


#### termsOfService

**Cardinality:** Required, Repeatable

**Content:** string or [LabeledLink](#sec-labeledlink)

**Description:** Description of access privileges required to use the API, e.g. registration, licensing, payments. Note that access constraints applying to all distributions of the resource should be specified in the access constraints for the resource description as a whole.


#### documentation

**Cardinality:** Optional

**Content:** string or [LabeledLink](#sec-labeledlink)

**Description:** A machine-actionable description of a service instance. Examples include OpenAPI documents, OGC Capabilities documents. Software designed to utilise a particular service type will typically include functionality to parse such a description document and engage with the service endpoint. If such a document is available for the service instance providing the resource distribution, it should be included in the distribution metadata.


#### potentialAction

**Cardinality:** Required, Repeatable

**Content:** [object reference](#object-reference), [Action](#sec-action)

**Description:** Description of the operations offered by the interface.


### Action {#sec-action}

#### @type

**Cardinality:** Required, Repeatable

**Content:** string.uri

**Description:** The rdf type default is \'Action\', but any of these schema.org actions will validate: {Action, AssessAction, ConsumeAction, ControlAction, CreateAction, DeleteAction, FindAction, InteractAction, MoveAction, PlayAction, SearchAction, TransferAction, UpdateAction}


#### name

**Cardinality:** Required

**Content:** string

**Description:** text label for the action


#### target

**Cardinality:** Required

**Content:** [EntryPoint](#sec-entrypoint)

**Description:** specifies the request target location and request syntax


#### result

**Cardinality:** Optional

**Content:** [DataDownload](#sec-datadownload)

**Description:** specifies the serialization scheme (encoding format, information model) for expected representation of the data


#### object

**Cardinality:** Optional

**Content:** Thing

**Description:** Specifies the resource that is the object (input) of the action. The value is an open ended class (Thing can be anything...) for general description of Actions. When schema:Action (or a subclass) is used to descibe operations for a WebAPI distribution (the normal CDIF usage), the object is implicitly the resource that is the subject of the containing metadata record, so this property would be superfluous.


#### query-input

**Cardinality:** Optional, Repeatable

**Content:** [PropertyValueSpecification](#sec-propertyvaluespec)

**Description:** set of explanations of the parameters in the URL template for the target EntryPoint.


### Person {#sec-person}

Object representing a person.

#### @id

**Cardinality:** Optional

**Content:** string.uri

**Description:** Identifier for this graph node. Useful to reference this Person using object references if they appear more that once in the metadata record.


#### @type

**Cardinality:** Required -- \'Person\', Repeatable

**Content:** string.uri

**Description:** rdf:type for this JSON-LD object.


#### name

**Cardinality:** Required if no identifier

**Content:** string

**Description:** Label for person that is meaningful for human users, should format consistently. Recommend \'Family Name, Given Name\' format.


#### @identifier

**Cardinality:** Required if no name

**Content:** string.uri or [PropertyValue-(identifier)](#sec-propertyvalue-id)


#### description

**Cardinality:** Optional

**Content:** string

**Description:** Other useful information about the person.


#### alternateName

**Cardinality:** Optional

**Content:** string

**Description:** Other names by which the person is known.


#### affiliation

**Cardinality:** Optional

**Content:** [Organization](#sec-organization)

**Description:** Organization that the person is associated with.


#### contactPoint

**Cardinality:** Optional

**Content:** [ContactPoint](#sec-contactpoint)

**Description:** email is required property if a contactPoint is included. Schema.org allows telephone and postal contacts as well.


#### sameAs

**Cardinality:** Optional, Repeatable

**Content:** string, [object reference](#object-reference)


### Organization {#sec-organization}

#### @id

**Cardinality:** Optional

**Content:** string.uri

**Description:** Identifier for this graph node. Useful to reference this Organization using object references if they appear more that once in the metadata record.


#### @type

**Cardinality:** Required -- \'Organization\', Repeatable

**Content:** string.uri

**Description:** rdf:type, list must include Organization, but other schema.org types can be added for more precision: FundingAgency, Consortium, Corporation, EducationalOrganization, FundingScheme, GovernmentOrganization, NGO, Project, ResearchOrganization, defined by enumeration in the schema.


#### name

**Cardinality:** Required if no identifier

**Content:** string

**Description:** Label for the Organization


#### @identifier

**Cardinality:** Required if no name

**Content:** string.uri or [PropertyValue-(identifier)](#sec-propertyvalue-id)


#### additionalType

**Cardinality:** Optional, Repeatable

**Content:** string or [DefinedTerm](#sec-definedterm)


#### alternateName

**Cardinality:** Optional

**Content:** string

**Description:** other labels by which the organization might be known


#### description

**Cardinality:** Optional

**Content:** string


#### sameAs

**Cardinality:** Optional, Repeatable

**Content:** string, [object reference](#object-reference)


### ContactPoint {#sec-contactpoint}

Information about how to communicate with a person or organization. CDIF only includes e-mail in its schema.

#### @type

**Cardinality:** Required -- \'ContactPoint\', Repeatable

**Content:** string.uri


#### email

**Cardinality:** Required

**Content:** string

**Description:** Property is required if a contactPoint property is included. Use missing@example.org if e-mail address is not available. Recommend using position-based contact point because people move around.


### Contributor {#sec-contributor}

For more granularity on how an agent contributed to a resource, use schema:Role. The schema.org documentation does not state that the Role type is an expected data type for the contributor property, but that is addressed in this blog post (http://blog.schema.org/2014/06/introducing-role.html). see also [ESIPfed Science on Schema.org roles of people note](https://github.com/ESIPFed/science-on-schema.org/blob/develop/guides/Dataset.md#roles-of-people).

#### @type

**Cardinality:** Required \-- \'Role\', Repeatable

**Content:** string.uri

**Description:** rdf:type


#### roleName

**Cardinality:** Required

**Content:** string, [DefinedTerm](#sec-definedterm)

**Description:** term that specifies the relationship between the contributor and the described resource.


#### contributor

**Cardinality:** Required

**Content:** [object reference](#object-reference), [Person](#sec-person) or [Organization](#sec-organization)


### MonetaryGrant {#sec-monetarygrant}

#### @type

**Cardinality:** Required \-- \'MonetaryGrant\', Repeatable

**Content:** string.uri


**CHOICE (at least one of identifier, name, or funder**

#### @identifier

**Cardinality:** Required if no name or funder

**Content:** string.uri or [PropertyValue-(identifier)](#sec-propertyvalue-id)

**Description:** identifier for a particular grant


#### name

**Cardinality:** Required if no identifer or funder

**Content:** string

**Description:** title of the grant


#### funder

**Cardinality:** Required if no identifier or name

**Content:** [object reference](#object-reference), [Person](#sec-person), or [Organization](#sec-organization)


#### description

**Cardinality:** Optional

**Content:** string

**Description:** description of the funding or grant


### Dataset/dcat:CatalogRecord {#sec-catalogrecord}

This is the class used to provide information about the metadata record itself.

#### @id

**Cardinality:** Required

**Content:** string.uri

**Description:** Identifier for the metadata record.


#### @type

**Cardinality:** Required -- \"Dataset\", Repeatable

**Content:** string.uri


#### additionalType

**Cardinality:** Required -- \"dcat:CatalogRecord\", Repeatable

**Content:** string


#### about

**Cardinality:** Required

**Content:** [object reference](#object-reference)

**Description:** This must be a reference to the metadata record that this node documents, using the \@id of that record.


#### conformsTo

**Cardinality:** Required, Repeatable

**Content:** [object reference](#object-reference)

**Description:** Identifiers for conformance classes/profiles that the metadata record follows. For CDIF discovery must include 'https://w3id.org/cdif/discovery/1.0' and 'https://w3id.org/cdif/core/1.0' because conforms to both profiles.


#### description

**Cardinality:** Optional

**Content:** string

**Description:** other information about the metadata record that might be useful.


#### maintainer

**Cardinality:** Optional

**Content:** [Person](#sec-person) or [Organization](#sec-organization)

**Description:** Identification of the agent that maintains the metadata, with contact information. Should include person name and affiliation, or position name and affiliation, or just organization name. e-mail address is preferred contact information.


#### sdDatePublished

**Cardinality:** Optional

**Content:** ISO 8601 formatted date/datetime

**Description:** date of most recent update to the metadata content


#### includedInDataCatalog

**Cardinality:** Optional

**Content:** [DataCatalog](#sec-datacatalog)

**Description:** identify the source for the origin the metadata record


### DataCatalog {#sec-datacatalog}

An accessible collection of data. The data might be metadata (about other resources) or datasets.

#### @type

**Cardinality:** Required -- \'DataCatalog\', Repeatable

**Content:** string.uri


#### @id

**Cardinality:** Optional

**Content:** string.uri

**Description:** identifier for graph node.


#### name

**Cardinality:** Optional

**Content:** string

**Description:** Label for the data catalog.


#### url

**Cardinality:** Optional

**Content:** string.url

**Description:** Url to access catalog landing page.


#### @identifier

**Cardinality:** Optional

**Content:** string.uri or [PropertyValue-(identifier)](#sec-propertyvalue-id)

**Description:** Identifier for the data catalog.


## Classes added by CDIF Discovery profile

### PropertyValue-(variableMeasured) {#sec-propertyvalue-vm}

#### @type

**Cardinality:** Required -- \"PropertyValue\", Repeatable

**Content:** string.uri


#### @id

**Cardinality:** Optional

**Content:** string.uri


#### name

**Cardinality:** Required

**Content:** string

**Description:** string label associated with the variable in the dataset serialization


#### description

**Cardinality:** Optional

**Content:** string

**Description:** description of variable intention and implementation. Default is 'missing'


#### alternateName

**Cardinality:** Optional, Repeatable

**Content:** string

**Description:** human intelligible name for variable that conveys semantics


#### measurementTechnique

**Cardinality:** Optional

**Content:** string, [object reference](#object-reference), or [DefinedTerm](#sec-definedterm)

**Description:** Text description or URI specifying how values for the variable were obtained.


#### propertyID

**Cardinality:** Optional, Repeatable

**Content:** string, [object reference](#object-reference), or [DefinedTerm](#sec-definedterm)

**Description:** identifier or name for the property concept


#### unitText

**Cardinality:** Optional

**Content:** string

**Description:** unit of measurement as text


#### unitCode

**Cardinality:** Optional

**Content:** string, [object reference](#object-reference), or [DefinedTerm](#sec-definedterm)

**Description:** URI or code identifying the unit of measure


#### minValue

**Cardinality:** Optional

**Content:** number

**Description:** minimum numeric value for this variable in the dataset


#### maxValue

**Cardinality:** Optional

**Content:** number

**Description:** maximum numeric value for this variable in the dataset


#### url

**Cardinality:** Optional

**Content:** string or [LabeledLink](#sec-labeledlink)

**Description:** references additional information, and label could be used to indicate type of description -- e.g., I-ADOPT, CDIF, etc.


### Place {#sec-place}

#### @type

**Cardinality:** Required -- \"Place\", Repeatable


CHOICE. At least one of the following four is required

#### additionalType

**Cardinality:** optional, Repeatable

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** Domain-specific type classifications for this place (e.g. facility type, laboratory classification, feature type)


#### name

**Cardinality:** Conditional, Repeatable

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** multiple place names or DefinedTerms that have a place name and URI for the location


#### @identifier

**Cardinality:** Conditional

**Content:** string.uri or [PropertyValue-(identifier)](#sec-propertyvalue-id)


#### geo

**Cardinality:** Conditional

**Content:** [GeoCoordinates](#sec-geocoordinates) or [GeoShape](#sec-geoshape)

**Description:** Either a bounding box or a point location. Use WGS 84 latitude and longitude coordinates


#### geosparql:HasGeometry

**Cardinality:** Conditional

**Content:** [sf:SimpleFeature](#sec-sfsimplefeature)

**Description:** Optional geographic extent using [wkt geometry](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry), see [Ocean InfoHub](https://book.oceaninfohub.org/thematics/spatial/README.html#simple-geosparql-wkt). Other geometry schemes might be specified in a specific domain profile, e.g. for atmospheric, subsurface data, or local coordinate systems. NOTE that the location specified here should be the same as the schema.org point or contained within the specified bounding box.


#### alternateName

**Cardinality:** Optional, Repeatable

**Content:** string, [DefinedTerm](#sec-definedterm)

**Description:** multiple place names or [DefinedTerm](#sec-definedterm)s that have a place name and URI for the location


### Defined Term {#sec-definedterm}

#### @type

**Cardinality:** Required -- \'DefinedTerm\', Repeatable

**Content:** string.uri


#### name

**Cardinality:** Required if no identifier or termCode

**Content:** string

**Description:** label for the term


#### @identifier

**Cardinality:** Required if no name or termCode

**Content:** string.uri or [PropertyValue-(identifier)](#sec-propertyvalue-id)


#### termCode

**Cardinality:** Required if no name or identifier

**Content:** string

**Description:** A representative code for this keyword in the controlled vocabulary. Analogous to skos:Notation


#### inDefinedTermSet

**Cardinality:** Optional

**Content:** string

**Description:** Name for the controlled vocabulary responsible for this keyword.


## Data types used for CDIF Core

### PropertyValue-(identifier) {#sec-propertyvalue-id}

#### @type

**Cardinality:** Required -- \'PropertyValue\', Repeatable

**Content:** string.uri


#### value

**Cardinality:** Required if no url

**Content:** string

**Description:** the identifier string. E.g. 10.5066/F7VX0DMQ


#### url

**Cardinality:** Required if no value

**Content:** string.url

**Description:** web-resolveable string for the identifier; host name part is location of a resolver that will return some representation for the given identifier value. E.g. https://doi.org/10.5066/F7VX0DMQ


#### propertyID

**Cardinality:** Optional

**Content:** string:uri

**Description:** In this context for the schema:PropertyValue, this field is an identifier for the identifier schema, e.g. DOI, ARK. Get values from https://registry.identifiers.org/registry/ for interoperability


### Labeled Link {#sec-labeledlink}

#### @type

**Cardinality:** Required -- \'CreativeWork\', Repeatable

**Content:** string.uri


#### url

**Cardinality:** Required

**Content:** string:uri

**Description:** URL for web location to GET the resource


#### name

**Cardinality:** Optional

**Content:** string

**Description:** Label for the linked resource


#### description

**Cardinality:** Optional

**Content:** string

**Description:** Text description of the linked resource.


### spdx:Checksum {#sec-spdxchecksum}

#### spdx:algorithm

**Cardinality:** Required

**Content:** string

**Description:** Name or identifier for the algorithm used to calculate the checksum.


#### spdx: checksumValue

**Cardinality:** Required

**Content:** string

**Description:** the checksum string.


### LinkRole {#sec-linkrole}

This is the type used for links that have an associated semantic conveyed by the linkRelationship.

#### @type

**Cardinality:** Required -- \'LinkRole, Repeatable

**Content:** string.uri


#### linkRelationship

**Cardinality:** Required

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** Term that specifies the relationship between the source and target of the link.


#### target

**Cardinality:** Required

**Content:** [EntryPoint](#sec-entrypoint)

**Description:** URL for link target, along with a label and encoding format for the target resource.


### EntryPoint {#sec-entrypoint}

Use to document the URL that is the target for invoking an action, or that is the target object of a link relationship.

#### @type

**Cardinality:** Required -- \"EntryPoint\", Repeatable

**Content:** string. Uri


#### encodingFormat

**Cardinality:** Optional

**Content:** string**,** MIME TYPE**

**Description:** **


#### name

**Cardinality:** Optional

**Content:** string

**Description:** Label for the resource located by the URL


#### url

**Cardinality:** Required

**Content:** string.url

**Description:** Locator that can be used to retrieve the target resource on the Web.


## Data types added by CDIF Discovery profile

### GeoCoordinates {#sec-geocoordinates}

A point location specified with latitude and longitude in decimal degrees, using the WGS84 spatial reference system.

#### @type

\Required -- \' [GeoCoordinates'\] (string:uri)

#### latitude

**Cardinality:** Required

**Content:** number

**Description:** Decimal degrees, value \>=-90 and \<= 90.


#### longitude

**Cardinality:** Required

**Content:** number

**Description:** east-longitude coordinate in decimal degrees. Value must be \>= -180 and \<= 180.


### GeoShape {#sec-geoshape}

CDIF limits schema:GeoShape to a box or line (schema.org includes other options). Point locations are tuples of {latitude east-longitude} (y x). (documentation from [Science on Schema.org](https://github.com/ESIPFed/science-on-schema.org/blob/develop/guides/Dataset.md#spatial-coverage) see details there)

#### @type

**Cardinality:** Required -- \'GeoShape\'

**Content:** string:uri


#### box

**Cardinality:** Required if no line

**Content:** string

**Description:** A rectangular (in lat-long space) extent specified by two points, the first in the lower left (southwest) corner and the second in the upper right (northeast) corner. The schema.org [GeoShape](https://schema.org/GeoShape) documentation states *Either whitespace or commas can be used to separate latitude and longitude; whitespace should be used when writing a list of several such points*.\" Since the box is a list of points, a space should be used to separate the latitude and longitude values. The two corner coordinate points are separated by a space. \'East longitude\' means positive longitude values are east of the prime (Greenwich) meridian.


#### line

**Cardinality:** Required if no box

**Content:** string

**Description:** a series of two or more points. Use for extents like a ship track, flight path, or foot traverse.


### sf:SimpleFeature {#sec-sfsimplefeature}

#### @type

**Cardinality:** Required

**Content:** string:uri

**Description:** Must be MUST be sf:SimpleFeature geometry type (http://www.opengis.net/ont/sf#). See https://opengeospatial.github.io/ogc-geosparql/geosparql11/sf_geometries.ttl


#### geosparql:asWKT

**Cardinality:** Required, Repeatable

**Content:** typed string

**Description:** geosparql specifies that a well known text (WKT) geometry object has an \@value is a string, and an \@type \"geosparql:wktLiteral\"


#### geosparql:crs

**Cardinality:** Optional

**Content:** [object reference](#object-reference)

**Description:** specify the coordinate reference system for the coordinate numbers in the WKT location description.


### time:Proper Interval {#sec-properinterval}

Intervals can be bounded by named ordinal eras (e.g. Jurassic, Tang dynasty, Paleolithic) identified by URI, or by numeric bounds that are time coordinates in a specified reference system (implemented by the TimePosition data type). This implementation is a simplified profile based on the [W3C OWL time specification](https://www.w3.org/TR/owl-time/), using the [http://www.w3.org/2006/time#](http://www.w3.org/2006/time) namespace, which is included in the default context for this profile.

#### @type

**Cardinality:** Required -- \'time:ProperInterval\', repeatable

**Content:** string:uri


#### description

**Cardinality:** optional

**Content:** string

**Description:** Text description of the time interval. If defined by an ISO8601 time interval string, put that here.


Choice:

#### time:startedBy

**Cardinality:** Optional

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** identifier for a named time ordinal era that is older bound of time interval, e.g. \'isc:LowerDevonian\'


#### time:finishedBy

**Cardinality:** Optional

**Content:** string or [DefinedTerm](#sec-definedterm)

**Description:** identifier for a named time ordinal era that is younger bound of time interval, e.g. \'isc:LowerDevonian\'


OR:

#### time:hasBeginning

**Cardinality:** Optional

**Content:** [time:TimePosition](#sec-timeposition)

**Description:** Temporal position for the beginning (older bound) of the interval, located by a numeric value in a temporal reference system


#### time:hasEnd

**Cardinality:** Optional

**Content:** [time:TimePosition](#sec-timeposition)

**Description:** Temporal position for the end (younger bound) of the interval, located by a numeric value in a temporal reference system


### time:TimePosition {#sec-timeposition}

#### @type

**Cardinality:** Required -- \'time:TimePosition\', repeatable

**Content:** string:uri


#### time:hasTRS

**Cardinality:** Required

**Content:** [object reference](#object-reference)

**Description:** identifier for a temporal reference system; default is million years before prsent as a decimal number. Default is http://www.opengis.net/def/crs/OGC/0/ChronometricGeologicTime


#### time:numericPosition

**Cardinality:** Required

**Content:** number

**Description:** Number that locates a temporal position in the reference frame defined by the hasTRS property.


### dqv:QualityMeasurement {#sec-qualitymeasurement}

#### @type

**Cardinality:** Required -- \'dqv:QualityMeasurement\', repeatable


#### dqv:ismeasurementOf

**Cardinality:** Required

**Content:** string, [object reference](#object-reference), or [DefinedTerm](#sec-definedterm)


#### dqv:value

**Cardinality:** Required

**Content:** string or [DefinedTerm](#sec-definedterm)


### PropertyValueSpecification {#sec-propertyvaluespec}

Description of the kind of value expected for a parameter value.

#### @type

**Cardinality:** Required -- \'PropertyValueSpecification\', repeatable


#### valueName

**Cardinality:** Required

**Content:** string

**Description:** This will be used to match the specification to parameters in a template string used to construct a query.


#### description

**Cardinality:** Required

**Content:** string

**Description:** Explanation of the purpose of the parameter, its range of values, datatype, etc.


#### valueRequired

**Cardinality:** optional

**Content:** boolean

**Description:** Default is true. False if the specified parameter is not required to fill the template.


#### valuePattern

**Cardinality:** optional

**Content:** string

**Description:** regular expression to validate values for template parameters.


# Namespaces

Namespace prefixes use in CDIF Discovery schema.org JSON-LD objects are specified by this JSON-LD context, which must be declared in every instance document. Note that the correct namespace URI for schema.org is '**http'**, not '**https'**. The [**https**://schema.org/](https://schema.org/) uri identifies the schema.org context document, not the namespace. This example context includes all the namespaces used in any cdif profile:

\"@context\": {\
\"schema\": \"http://schema.org/\",\
\"dcterms\": \"http://purl.org/dc/terms/\",\
\"geosparql\": \"http://www.opengis.net/ont/geosparql#\",\
\"spdx\": \"http://spdx.org/rdf/terms#\",\
\"cdi\": \"http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/\",\
\"csvw\": \"http://www.w3.org/ns/csvw#\",\
\"prov\": \"http://www.w3.org/ns/prov#\",\
\"time\": \"http://www.w3.org/2006/time#\",\
\"dqv\": \"http://www.w3.org/ns/dqv#\",\
\"sf\": \"http://www.opengis.net/ont/sf#\",\
\"ex\": \"https://example.org/\",\
\"xsd\": \"http://www.w3.org/2001/XMLSchema#\",\
\"dcat\": \"http://www.w3.org/ns/dcat#\" }

# Notes on schema.org implementation

## JSON-LD \@type

JSON-LD every graph node has a \@type property that specifies the rdf:type for the node. This type has implications for the properties expected to be found in the content of the node, and should convey the intention of the kind of thing the node is intended to represent. In the CDIF JSON-LD implementation, most of the \@types are taken from the schema.org vocabulary, but there are a few exceptions for content items that do not map to the schema.org vocabulary. The \@type is always serialized as an array \[JSON list\] to allow for extensions that add additional typing.

## Object reference

Linked data is implemented in rdf using URIs to reference objects that might be located in other parts of a graph, or remotely and accessed online. In the JSON-LD implementation, simply using a URI string as the value of a property does not create such a link---the value is simply a string, not the object reference by the URI. An \"object ref\" is always a string containing the id of the referenced object. Thus

*\"schema:funder\": \"<https://ror.org/021nxhr62>\"*

Does not create a link. Object references are implemented in JSON-LD as object that have a single node identifier as it property.

*\"schema:funder\": { \"@id\": \"https://ror.org/021nxhr62\" }*

Is the correct syntax to implemenat an object reference. Throughout this document, if \'object reference\' is included as a value type for a property, be aware that instance documents might simply have this kind of object as the property value.

## Repeating values

Any property with a 1..\* or 0..\* cardinality has values that are always implemented as arrays. This makes client processing easier because tests for single or array values are not necessary. If a property is 'repeatable', then assume the implementation is an array (JSON list).

## Namespace prefixes and JSON validation. 

Namespace prefixes are explicitly used in the example documents so that the JSON schema can validate instance documents. JSON Schema validates the literal JSON structure \-- property names, nesting, value types. Several features of JSON-LD can cause a semantically correct document to fail JSON Schema checks. The same property can appear as \"schema:name\", \"name\", or \"http://schema.org/name\" depending on the @context. A JSON Schema that checks for \"schema:name\" will reject a document that uses \"name\", even though both mean the same thing. See [Validating CDIF Profile Metadata](https://github.com/Cross-Domain-Interoperability-Framework/validation/blob/main/docs/CDIF-profiles-metadata-validation.md) for a detailed discussion of validation processes for CDIF metadata, and the use of framing to validate JSON-LD instances using different [JSON-LD forms](https://www.w3.org/TR/json-ld11/#forms-of-json-ld) or custom context documents..

## Use of dcat:CatalogRecord

In a harvesting/federated catalog system some metadata about the metadata is useful to keep track of where metadata came from, what format/profile it uses (harvesters need this to process), and update dates. Unambiguous expression of this information requires making statements about a metadata record distinct from the thing in the world that the metadata describes. In an RDF framework, this requires a distinct identifier for the metadata record object that will serve as the subject for these triples.

In the RDF serialization, [Schema.org](http://schema.org/) metadata records are [JSON-LD node objects](https://www.w3.org/TR/json-ld/#node-objects), and include an \"@id\" keyword with a value that identifies the node, analogous to a primary key in a relational database. This identifier can be interpreted to represent a thing in the world that the metadata record (the \'node\') is about, or to represent the metadata record (a JSON object) itself.

To avoid this ambiguity, CDIF adopts the convention that the [schema.org](http://schema.org/) identifier property is used to identify a thing in the world that is the subject of the JSON-LD node. The identified thing might be physical, imaginary, abstract, or a digital object. The JSON-LD \@id property identifies a node in a graph, which is an abstract object. As a URI the \@id URI is expected to dereference to produce a JSON-LD object containing the properties that are attached to the graph node.

Given this convention, when the metadata record is processed, the processor should use the schema:identifier as subject of triples about the subject of the metadata record to avoid ambiguity. In addition, this convention would suggest that if a schema:identifier property is present, the \@id property should be interpreted to identify the JSON object that is the representation of the node in the knowledge graph. In practice JSON-LD processors use the \@id as the subject of triples generated from a JSON-LD object. A \'purist\' approach would require a level of indirection to assert that the \@id is about the thing identified by the schema:identifier. JSON-LD processors don\'t do this, so standard practice is to make the \@id the identifier for the described resource, requiring understanding that it identifies two things---the rdf object at that graph node, and the thing in the world described by the content of that node. This has worked for the most part because metadata providers have been quite lax in providing information about the provenance of the metadata node, and in particular the conformance criteria that were followed in generating the content of that node.

To address this issue, CDIF recommends that statements about the metadata record (the JSON object) as a distinct entity should be made using a separate identified node object. This node object is typed as a schema:Dataset, with additionalType [dcat:CatalogRecord](https://www.w3.org/TR/vocab-dcat-3/#Class:Catalog_Record) recognizing that the DCAT v3 specification uses that element to address this precise issue. This node can be embedded in the Dataset metadata using the subjectOf property, and approach used in the accompanying JSON schema and examples, or implemented as a separate free standing graph node linked to the dataset object via the \'about\' [object reference](#object-reference).

Example instance with dcat catalog record content (mapped to schema.org properties):

```json
{
  "@context": [
    "https://schema.org",
    {
      "dcterms": "http://purl.org/dc/terms/",
      "ex": "https://example.com/99152/"
    }
  ],
  "@id": "ex:URIforNode1",
  "@type": "appropriate schema.org type",
  "identifier": "ex:URIforDescribedResource",
  "name": "unique title for the resource",
  "description": "Description of the resource",
  "subjectOf": {
    "@id": "ex:URIforNode2",
    "@type": "Dataset",
    "additionalType": "dcat:CatalogRecord",
    "sdDatePublished": "2017-05-23",
    "about": {"@id": "ex:URIforNode1"},
    "description": "metadata about documentation for ex:URIforDescribedResource",
    "dcterms:conformsTo": [
      {"@id": "https://w3id.org/cdif/core/1.0"},
      {"@id": "https://w3id.org/cdif/discovery/1.0"}
    ]
  }
}
```

## Polymorphism of PropertyValue

The schema.org PropertyValue type is used in several different contexts in the implementation of CDIF metadata. This is a result of how the expected values for some important properties are defined in schema.org. In the Discovery profile, PropertyValue is an allowed value type for variableMeasured and for identifier. In some more advanced profiles, PropertyValue is also an allowed value for additionalProperty.

The following table compared the properties and requirements for this schema.org type in these different contexts.

| Property | Description | identifier | variableMeasured | additionalProperty |
|---|---|---|---|---|
| @type | Type declaration (must contain schema:PropertyValue) | 1..* required, contains: PropertyValue | 1..* required, contains: PropertyValue | 1..* required, contains: PropertyValue |
| @id | URI identifier for this node | - | 0..1 string | - |
| schema:name | Human-readable label | - | 1 required string | 1 required string |
| schema:description | Textual description | - | 0..1 string, default: "missing" | - |
| schema:alternateName | Alternative names | - | 0..* array of strings | - |
| schema:propertyID | Identifier for the property concept | 0..1 string (identifier scheme name) | 0..* array of: string \| {@id} \| DefinedTerm | 1..* required array of: string \| {@id} \| DefinedTerm; minItems: 1 |
| schema:value | The property value | 0..1 (conditional) string; required if no schema:url | - | 1 required string \| number \| boolean \| object |
| schema:url | Web-resolvable URL | 0..1 (conditional) string (uri format); required if no schema:value | 0..1 string (uri) \| LabeledLink | - |
| schema:unitText | Unit of measurement as text | - | 0..1 string | 0..1 string |
| schema:unitCode | URI or code for unit of measure | - | 0..1 string \| {@id} \| DefinedTerm | 0..1 string \| DefinedTerm |
| schema:measurementTechnique | How values were obtained | - | 0..1 string \| {@id} \| DefinedTerm | - |
| schema:minValue | Minimum numeric value | - | 0..1 number | - |
| schema:maxValue | Maximum numeric value | - | 0..1 number | - |
