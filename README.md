# NDE requirements

## Introduction

This document helps vendors of software - like collection management systems, and linked data publication platforms - to make their systems NDE compliant and ready for use in the Dutch heritage sector.  

The guidelines in this document are required for any institution in the Dutch cultural sector that receives funding through the NDE *Versnellingsprogramma* (Acceleration Program). Software vendors help these organisations to meet the requirements by implementing them as features in their systems.

Organisations who provide datasets or collections according to these guidelines are considered  [*Bronhouders*](https://dera.netwerkdigitaalerfgoed.nl/index.php/Rollen#Bronhouder) (Data Providers) as defined by the *Digitaal Erfgoed Referentie Architectuur* - [DERA](https://dera.netwerkdigitaalerfgoed.nl)) (Digital Heritage Reference Architecture).

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

Please refer to our [Implementation Guidelines](https://netwerk-digitaal-erfgoed.github.io/cm-implementation-guidelines/#publishing-collection-information) for further detailed technical information.

## Compatibility requirements

### 1. Persistent Identifiers

Every information resource needs a [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) that identifies it to the world. Users must be able to trust these identifiers, they should be unique and reliably serve the same resource. URIs must be resolvable and point the user to valid linked data. 

Persistent Identifiers (PIDs) are the part of a URI that uniquely identifies a resource. Heritage institutions must use Persistent Identifiers (PIDs) for their resources to guarantee reliability. With a PID the link to a resource keeps working even when the organisation, or the object's location, has changed.

#### 1.1 PID Implementations

There are multiple PID systems such as [Archival Resource Keys](https://arks.org) (ARKs), [Digital Object Identifiers](https://www.doi.org) (DOIs), [Handle](http://handle.net), and others. You may choose any implementation, even your own system based on the internet domain of your organisation. Each system has its own particular properties, strengths, and weaknesses. Our [PID Guide](https://www.pidwijzer.nl/en) helps you make a choice. 

As a heritage institution you must choose a PID system. You may be limited by the PID systems supported by your software vendor. But when you change vendor, your PID system migrates with you. 

Regardless of your choice for a PID system, you are required to structurally guarantee the system in your organisation. You must document the formal processes and policies that describe how PIDs are sustainably implemented and commit to them over time.

As a vendor you must implement at least one PID system in your software. You may support more. When implementing the PID-system you must link resources to your customer (the cultural heritage institution) and not to you as a vendor. Persistency must be guaranteed when resources are migrated to another vendor in the future.

#### 1.2 Linking to removed resources

Removed resources are all objects that have been deleted, made invisible, or that for any other reason are no longer structurally available . You must maintain persistency for removed resources. To handle such requests you must use [standard HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages). You may use 'HTTP 301 Moved Permanently' to redirect the URI of a removed resource to a new resource that replaces it, or mark a removed resource as 'obsolete' by throwing 'HTTP 410 Gone' and serve its last incarnation, or choose another solution. 

Unless an actual error occurs, you may not throw a client error response (category 400) or a server error (category 500) response to point to removed resources.

### 2.  Publication of Linked Open Data

#### 2.1 Data modelling

We distinguish between the publication of data in "generic data models" and "domain data models". The purpose of generic data models is to integrate the data in the cultural heritage network and make it more visible. Domain models are usually more richly populated and provide users with more possibilities for further processing in [*dienstplatformen*](https://netwerkdigitaalerfgoed.nl/nieuws/maak-jij-erfgoedsites-en-apps-volg-de-afspraken-uit-de-architectuurblauwdruk-voor-dienstplatformen/) (Service Platforms).

You must use the generic model [schema.org](https://schema.org) when you choose to serve linked data in only one model. 

We recommend you also serve data in one or more domain specific models. Domain models make reuse of data much more likely and may contain a wealth of information. Depending on the type of data, you may consider the following examples:
- Museum collections and catalogues: [CIDOC-CRM](https://cidoc-crm.org) and its derivative [Linked-Art](https://linked.art/model/).
- Data from archives: [RiC-O](https://www.ica.org/standards/RiC/RiC-O_v0-2.html)
- Libraries: [RDA Elements](https://www.rdaregistry.info/Elements/) following the recommendations of the [RDA application profile Dutch bibliography](https://netwerk-digitaal-erfgoed.github.io/rdanl/) (only in Dutch). When using other library domain standards like [BIBFRAME](https://www.loc.gov/bibframe/) you should document the implementation choices made and relation to the RDA application profile.

You should publish documentation how your data model maps to other wellknown models like the [Europeana Data Model (EDM)](https://pro.europeana.eu/page/edm-documentation). We recommend you publish this documentation in a machine readable format. NDE can support you when creating these mappings.

#### 2.2 Serving data

The [Implementation Guidelines](https://netwerk-digitaal-erfgoed.github.io/cm-implementation-guidelines/#publishing-collection-information) mentioned above consider three levels when [publishing linked data](https://netwerk-digitaal-erfgoed.github.io/cm-implementation-guidelines/#publishing-linked-data):
1. Web compliance level: resolvable URIs
2. Basic level: data dumps
3. Advanced level: queryable Linked Data

You must publish linked data both as level 1 and 2. 

To publish web compliant linked data (level 1) you must use URIs (see: [persistent identifiers](###%201.%20Persistent%20Identifiers)) to resolve the individual linked data resources contained in your data. To handle requests you must support the [HTTP content negotation](https://datatracker.ietf.org/doc/html/rfc7231#section-5.3). Content negotiation offers users the choice to consume the data in the serialisation of their choice, e.g. as HTML, RDF, or other formats you support.

To publish linked data as a data dump (level 2) you should use [RDF](https://www.w3.org/RDF/). We recommend the use of the N-Triples serialisation for flexibility but any other RDF serialisation is valid (N-Triples, Turtle, JSON-LD, RDF/XML). You must serve the data dump through a URI with a persistent identifier. 

We strongly recommend that vendors also implement level 3 in their products. This allows users to also query linked data collections. For compliance with level 3 you must use a [SPARQL](https://www.w3.org/TR/rdf-sparql-query/) endpoint.

> Note, when you comply with levels 1 and 2 you allow users to access the individual linked data objects and the full dataset, while you avoid maintaining a linked data infrastructure that you would need to provide for querying. The drawback of this approach is that you move the burden to facilitate querying to the user. For this reason we view compliance with level 1 and 2 as a minimal requirement. We strongly urge you to go further and implement level 3. Compliancy with level 3 will very likely play a large role in the allocation of future NDE funding.

### 3. Implementation of the NDE *termennetwerk* (Network)

To connect the linked data that you publish with other data sources heritage institutions should use terms. Terms are descriptions of concepts or entities, such as subjects, people or places, and are managed in terminology sources, such as thesauri, reference lists or classification systems. Examples are [AAT](https://www.getty.edu/research/tools/vocabularies/aat/), [GeoNames](https://www.geonames.org/) or [WO2 Thesaurus](https://data.niod.nl/WO2_Thesaurus.html) (in Dutch).

Heritage institutions may use their own terminology sources. However, this reduces their ability to connect to sources from different institutions when using terms that are not part of internationally standardised terminology sources. We recommend using as many standardised terminology sources as possible. We developed the Network-of-Terms service to give you easy access to many different terminology sources.

If a heritage institution is considered an authority in a domain, vendors should publish their terminology sources for use by others and make it available through the Network-of-Terms. Please refer to our [Implementation Guidelines on publishing terminology sources](https://netwerk-digitaal-erfgoed.github.io/cm-implementation-guidelines/#publishing-terminology-sources). 

For vendors we require the implementation of the [NDE Network-of-Terms API](https://termennetwerk.netwerkdigitaalerfgoed.nl/faq) in the collection management system. This will give your customers easy access to the terminology sources made available through the Network-of-Terms. You must implement configurable terms for fields in your heritage application. You may choose the specific fields. If you allow users of your application to configure the data type of a field, you should add a data type for terms. That way users are not limited in the (number of) fields that they want to configure for certain sets of terms. 

### 4. Registration with the NDE dataset register

To increase the findability of datasets of heritage institutions, you must publish the metadata of any dataset you serve. The metadata contains the description of the dataset and must be modelled in [schema.org](https://schema.org). We created a data model for the description of datasets and adopted the class https://schema.org/Dataset. You must comply with the model as documented in the [NDE Requirements for Datasets](https://netwerk-digitaal-erfgoed.github.io/requirements-datasets) and specifically the [section Dataset information](https://netwerk-digitaal-erfgoed.github.io/requirements-datasets/#dataset-information).

You are required to serve the metadata through a URI with a [persistent identifier](###%201.%20Persistent%20Identifiers). 

We have created the [NDE dataset register](https://datasetregister.netwerkdigitaalerfgoed.nl/?lang=en) to help users in the cultural heritage sector find relevant datasets to link with. You must use this to register the metadata of datasets you serve. 

Although the register provides a manual registration option for users, as a vendor you are required to use the [Dataset Register API](https://datasetregister.netwerkdigitaalerfgoed.nl/apidoc.php?lang=en) to implement an automated registration feature in your software. The API description is available based on an [Open API](https://www.openapis.org) specification via the [Swagger UI](https://datasetregister.netwerkdigitaalerfgoed.nl/api/static/index.html). The manual service may be used for publishing updates and changes. 

You must publish the metadata with an open data license. You may choose the most suitable license.

The actual access to digital objects, such as images, audio files, or linked data resources, may be restricted. You must specify additional license statements for use and reuse of the objects in the metadata. Use the [schema:license](https://schema.org/license) property to relate a license to an information resource. 

Remember that you must relate a license to the dataset, the object metadata, and the reproduction. These licenses may differ.

### 5. Serving images through IIIF

You must implement the [IIIF Image API](https://iiif.io/api/image/3.0/) to publish images contained in your system. The Image API supports requests for specific parts, sizes and orientations of images based on a URL scheme. It gives viewers the ability to request only those tiles needed for the current viewport and zoom level, instead of forcing them to load the complete high resolution image. You may choose the highest zoom level, size, and number of parts of any image you serve, since these are directly related to the available storage in your system, the results of the digitisation process at the institution, and other customer wishes. 

We use the [IIIF Image API Validator](https://iiif.io/api/image/validator/) to test compliance.

While the Image API provides access to individual images, it does not describe the relationship between groups of images, such as the individual scans that make up the pages of a book. Besides the IIIF Image API you must also publish an IIIF manifest through the [IIIF Presentation API](https://iiif.io/api/presentation/3.0/). An IIIF manifest is a JSON-LD document that describes the structure and metadata of a digital object or collection of objects, such as images, audio, or video. It comprises several key components, including pages, canvases, table of contents (ranges), and annotations. You must serve the IIIF manifest through a URI with a [persistent identifier](###%201.%20Persistent%20Identifiers). To improve the readability of the media the metadata of the object must be readable for both humans and machines.

We use the [IIIF Presentation API Validator](https://presentation-validator.iiif.io) to test compliance.
