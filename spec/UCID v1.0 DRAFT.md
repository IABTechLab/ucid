![](https://app.extremereach.com/content/images/logo_header.gif)

# Universal Creative Identification Framework Specification v1.0

**September 2021**

**About Extreme Reach**

Extreme Reach (ER) is the global leader in creative logistics. Its end-to-end technology platform moves creative at the speed of media, simplifying the activation and optimization of omnichannel campaigns for brands and agencies with unparalleled control, visibility and insights. 

One global creative-to-media supply chain answers the challenges of a complex marketing landscape and an equally complicated infrastructure under the global advertising ecosystem. The company's groundbreaking solution integrates all forms of linear TV and non-linear video workflow seamlessly with talent payments and rights management. Now, brands and agencies can optimize campaigns as fast as consumer consumption shifts across linear TV, CTV, OTT, addressable TV, mobile, desktop, and video-on-demand. 

Extreme Reach connects brand content with consumers across media types and markets, fully illuminating the marketing supply chain for a clear view of creative usage, waste, performance and ROI. 
 
Extreme Reach operates in 140 countries and 45 languages, with 1,100 team members serving 90 of the top 100 global advertisers and enabling $150 billion in video ad spend around the world. More than half a billion creative brand assets are managed in ER’s creative logistics platform.

Learn more about Extreme Reach at [www.extremereach.com](https://www.extremereach.com).

**License**

Universal Creative Identification Framework Specification is licensed under a Creative Commons Attribution 3.0 License. To view a copy of this license, visit[ creativecommons.org/licenses/by/3.0/](http://creativecommons.org/licenses/by/3.0/) or write to Creative Commons, 171 Second Street, Suite 300, San Francisco, CA 94105, USA.

![](https://drive.google.com/uc?id=1cbwEGlb8S69SndIDoHnvc5_3TfmkGM7R)

**TABLE OF CONTENTS**

- [OVERVIEW](#overview)
  - [Universal Creative Identification Mission](#ucidmission)
  - [Universal Creative Identification Framework Executive Summary](#execsummary)
  - [History of Universal Creative Identification Framework](#historyofucid)
- [REGULATORY GUIDANCE](#guidance)
- [ARCHITECTURE](#architecture)
  - [Terminology](#terminology)
  - [Universal Creative Identification Framework Principles](#ucid_principles)
  - [Reference Model](#referencemodel)
    - [Universal Creative ID Structure - Default](#ucid-default)
    - [Universal Creative ID Structure - Example Custom Schema](#ucid-custom)
    - [Universal Creative ID Elements](#ucid-elements)
  - [Protocol Layers](#protocollayers)
    - [Layer-1:  Transport](#layer1_transport)
      - [Communications](#communications)
      - [Version Headers](#versionheaders)
      - [Transport Security](#transportsecurity)
    - [Layer-2:  Format](#layer2_format)
    - [Layer-3:  Transaction](#layer3_transaction)
    - [Layer-4:  Domain](#layer4_domain)
- [SPECIFICATION](#specification)
  - [Object Model](#objectmodel)
    - [Object:  RegistrationAuthority](#object_ra)
    - [Object:  Domain](#object_domain)
    - [Object:  Ucid](#object_ucid)
  - [Detailed Implementation Guide](#detailedimplementationguide)
- [EXAMPLES](#examples)
  - [UCID Creation](#examples_ucid_creation)
  - [UCID Validation](#examples_ucid_validation)
  - [RA Peer Discovery](#examples_peer_discovery)
- [Appendix A:  Additional Resources](#appendixa_additionalresources)
- [Appendix B:  Change Log](#appendixb_changelog)
- [Appendix C:  Errata](#appendixc_errata)
- [Appendix C:  Versioning Policy](#appendixd_versioning)


# OVERVIEW <a name="overview"></a>

## Universal Creative Identification Mission <a name="ucidmission"></a>

The mission of the Universal Creative Identification project is to provide an open, interoperable mechanism for creating and validating globally unique identifiers for disinguishing unique creative assets as they are produced, managed, aggregated, distributed and measured througout the marketing supply chain. By assigning a each creative its own, unique identifier parties throught the supply chain can use a consistent identifier when referencing a creative, regardless of where or how it is used.

## Universal Creative Identification Framework Executive Summary <a name="execsummary"></a>

This document specifies an open-standard framework for the creation and validation of globally unique identifers for creative assets. This specification aims to standardize and simplify the process for generating unique creative identifiers by multiple parties in the ecosystem. This results in an open ecosystem whereby multiple providers can participate instead of relying on a single, proprietary ID scheme, or on loosely defined formats and "honor system" uniqueness. The framework is patterned loosely after the [Internet Domain Name System](https://en.wikipedia.org/wiki/Domain_Name_System) (DNS), since Internet interoperability also depends on a clear system for ensuring uniqueness of resources. While DNS operates at a lower level of the networking stack, the Universal Creative Identification Framework operates on top of the HTTP protocol and uses standard HTTP REST API semantics to manage generation and access to creative identifiers. 

The overall goal of Universal Creative Identification is and has been to create a *uniform standard* for idenfiying creative assets used in marketing. The intent is not to specify exactly how each participating party generates unique identifiers. As a project, we aim to establish a basic standard format and facilitate interoperability between parties in the ecosystem, while ensuring global uniqueness of identifiers so that execution and reporting can be performed with greater accuracy.

## History of Universal Creative Identification Framework <a name="historyofucid"></a>

A critical element of accurate campaign measurement is the ability to differentiate which ad creative was seen by which people. Basic KPIs, such as unique reach, frequency, creative performance, etc. rely on uniquely and unambiguously identifying each ad creative and then correctly associating that identifier with individual ad exposure metrics. As simple and obvious as this may seem, it remains quite elusive to the vast majority of the measurement ecosystem. The lack of standardization in ad identification across linear, CTV, web, mobile, social and out-of-home marketing channels results in highly fragmented measurement data sets that, at best, require significant manual effort to reconcile and, at worst, lead to wildly inaccurate measurement results. Universal Creative Identification (UCID) was launched as a pilot project by Extreme Reach in September 2021 to address this lack of standardization for creative identification across the global marketing ecosystem. 

# REGULATORY GUIDANCE <a name="guidance"></a>

Universal Creative Identification implementations will need to ensure compliance with all applicable regional legislation.

# ARCHITECTURE <a name="architecture"></a>

This section describes the underlying model of the ecosystem to which creative identifiers apply and the overall organization of Universal Creative Identification so that specification details have proper context.

## Terminology <a name="terminology"></a>

The following terms are used throughout this document specifically in the context of Universal Creative Identification and this specification.

<table>
  <tr>
    <td><strong>Term</strong></td>
    <td><strong>Definition</strong></td>
  </tr>
  <tr>
    <td>Universal Creative Identifier (UCID)</td>
    <td>A globally unique identification code that distinguishes a specific piece of creative.</td>
  </tr>
  <tr>
    <td>Registration Authority (RA)</td>
    <td>An organization/entity that implements the Universal Creative Identification Specification and produces unique creative identifiers.</td>
  </tr>
  <tr>
    <td>Registration Authority ID (RAID)</td>
    <td>The unique, one-character identifer that uniquely identifies a Registration Authority.</td>
  </tr>
  <tr>
    <td>Client</td>
    <td>An organization/entity that wishes to create and/or validate UCIDs.</td>
  </tr>
  <tr>
    <td>Domain</td>
    <td>A 4-character prefix that contains a set of unique codes created for a specific client.</td>
  </tr>
  <tr>
    <td>Creative Metadata</td>
    <td>A set of additional attributes associated with a specific creative.</td>
  </tr>
</table>


## Universal Creative Identification Principles <a name="ucid_principles"></a>

The following points define the guiding principles underlying the Universal Creative Identification Framework Specification, some of its basic rules, and its evolution.

* The UCID framework involves designation of **Registration Authorities** that are recognized suppliers of trusted, unique creative identifiers within the creative ecosystem**. The list of recognized Registration Authorities is maintained on the [Universal Creative Identification Wikipedia page](https://www.wikipedia.org/wiki/ucid).
* Each Registration Authority (RA) is responsible for the following primary functions:
  * Issuing unique **Domains** for individual advertisers, agencies, etc. (clients), similar to how a company can register one or more Internet domains. The combination of RA and Domain must be globally unique.
  * Issuing unique creative identifiers, or **UCIDs** on behalf of authorized clients and ensuring that all issued identifiers are unique within each RA/Domain. Within a given RA/Domain a client may generate as many creative identifiers as they choose.
  * Validating UCIDs to confirm that they were previously issued by the RA, or one of its peers.
  * Each RA should implement all API operations and objects defined in this specification. This allows each RA to communicate with its peers by calling the defined operations against the *ApiBaseUrl* for each RA.
* An important element of the UCID framework is the inclusion of a **Registration Authority Identifier** (RAID) prefix on every generated identifier. This ensures that all creative identifiers are universally unique, even if two or more registries issue the same domain or even full code. This is similar to the “top level domain” (TLD) used with Internet domains (e.g. .com, .org, .edu, etc.)
* The RAID is represented by a single alpha-numeric character and can quickly distinguish which registry issued the code, similar to the first digit of a credit card designating VISA, MasterCard, etc.
* Another important aspect of the UCID framework is the *open validation* mechanism. Any creative identifier can be validated by simply calling a public API operation against any recognized RA. This allows parties throughout the marketing supply chain to confirm the uniqueness and source for creative identifiers.
* The framework allows “Legacy” creative codes that do not conform to the UCID structure (do not include a valid RAID prefix code) to be validated by querying peer RAs to verify that the code is valid and was generated by one of the peer RAs.
* Clients should ensure that a UCID is assigned to each unique creative asset they produce and that the same UCID is used for all renditions and uses for that creative. For example, a video ad creative used across linear TV, CTV, web and social channels should use the same UCID for all usages to ensure that measurement data from various sources can be aligned to the same creative message. 

** Note : An RA may elect to charge a fee for registration and maintenance of ID Domains, or may elect to offer the service for free as a value-add to other services.

Universal Creative Identification is a living specification. New objects and attributes may be added and enumerated lists may be extended at any time and thus implementers must accept these types of changes without breakage within a version number. See [Appendix D: Versioning Policy](#appendixd_versioning)

## Reference Model <a name="referencemodel"></a>

### Universal Creative ID Structure - Default <a name="ucid-default"></a>
The standard UCID structure for a creative identifier consists simply of an RAID, a Domain Code, a unique 6-character sequence and a Format Code, allowing up to 2,176,782,336 possible unique IDs per RA/Domain.
![UCID01](https://user-images.githubusercontent.com/7672719/136828415-5436ceec-ad0c-4dbc-a290-f9c16c2673db.png)

### Universal Creative ID Structure - Example Custom Schemas <a name="ucid-custom"></a>
If supported by the RA, a client may also elect to apply a custom schema to their identifiers, such that individual positions in the generated ad code can represent creative attributes that have specific meaning to the client. This flexibility in the schema also allows the UCID to incorporate formatting used in various "legacy" creative identifiers used across different global markets. In most cases, a legacy identifier can be converted to a UCID simply by adding an RAID prefix;'.

#### Encoding campaign and production year information within a UCID
![UCID02](https://user-images.githubusercontent.com/7672719/136828446-f40e2332-e823-4fee-a59a-6c13fc0cdee7.png)

#### Implementing Clearcast Clock-ID format within a UCID
![image](https://user-images.githubusercontent.com/7672719/137186878-6df34a98-5004-4095-aa9b-462f202a7b21.png)



### Default Universal Creative ID Elements <a name="ucid-elements"></a>

![image](https://user-images.githubusercontent.com/7672719/136845835-9ce3ffbb-e2cf-4887-91ab-a8d69ca7d896.png)


## Protocol Layers <a name="protocollayers"></a>

To assist in interoperability between Registration Authorities and to enable various aspects of the specification to evolve at different paces, a layered approach is being adopted as of Universal Creative Identification v1.0.  The following illustrates this model. Expressed informally, Layer-1 moves bytes between parties, Layer-2 expresses the language of these bytes, Layer-3 specifies a transaction using this language, and Layer-4 describes the goods being transacted.

The following subsections specify these layers as they pertain to the Universal Creative Identification Framework Specification. Unless explicitly specified otherwise, annotated as optional, or called out as a best practice, all material aspects of these subsections are required for Universal Creative Identification compliance.

### Layer-1:  Transport <a name="layer1_transport"></a>

#### Communications <a name="communications"></a>

The base protocol between an exchange and its demand sources is HTTP. Specifically, HTTP POST is required for Domain and UCID creation requests to and HTTP GET is used to query Registration Authorities and validate UCIDs.

Calls returning content (e.g., a UCID or UCID metadata) should return HTTP code 200.  Calls returning no content in response to valid requests should return HTTP 204. Invalid calls (e.g., a request containing a malformed or corrupt payload) should return HTTP 400 with no content.

#### Version Headers <a name="versionheaders"></a>

The Universal Creative Identification version must be passed in the header of each UCID API request with a custom header parameter. This will allow Registration Authorities and their Clients to recognize the version of the message contained before attempting to parse the request. See [Versioning Policy](#appendixd_versioning) and [Universal Creative Identification Principles](#ucid_principles) for more regarding versioning.

`x-ucid-version: 1.0`

Additionally, while optional, it is recommended that responses include an identically formatted HTTP header with the protocol version implemented by the responder.  It is assumed, however, that any response will be compatible with the version of the request and that version support is discussed beforehand between the parties. If the version header is omitted from a request the assumed version will be 1.0.

#### Transport Security <a name="transportsecurity"></a>

As of Universal Creative Identification v1.0, HTTPS and Transport Layer Security (TLS) version 1.2+ are required for compliance and thus all connections over which the Universal Creative Identification protocol operates must be HTTPS. 

### Layer-2:  Format <a name="layer2_format"></a>

JSON (JavaScript Object Notation) is the format used for all UCID request and response data payloads. JSON was chosen for its combination of human readability and relative compactness. Given the limited data payloads used and to maintain simplicity of implementations, no other formats are included in the specification.

### Layer-3:  Transaction <a name="layer3_transaction"></a>

The Transaction Layer defines the specific API operations used for creative identifier creation and validation protocol between a client and a Registration Authority, as well as the operations used for peer-to-peer communication between Registration Authorities.

Refer to the Specification section of this document for full details.

### Layer-4:  Domain <a name="layer4_domain"></a>

The Domain Layer defines the objects on which the Transaction Layer operates. In a typical UCID creation transaction the client would issue a request to a Registration Authority to generate a new UCID for a specific client domain and the newly created UCID object would be returned.

# SPECIFICATION <a name="specification"></a>

This section contains the detailed Universal Creative Identification transaction layer specification. Unless explicitly specified otherwise, annotated as optional, or called out as a best practice, all material aspects of this section are required for Universal Creative Identification compliance.

## Object Model <a name="objectmodel"></a>

The sections that follow describe the payload structures for the UCID request and response objects. Payloads are simple data structures that carry a small number of attributes.

Throughout the object model subsections, attributes may be indicated as “Required” or “Recommended”.  Attributes are deemed *required* only if their omission would break the protocol and is not necessarily an indicator of business value otherwise.  Attributes are *recommended* when their omission would not break the protocol but would dramatically diminish business value.

From a specification compliance perspective, any attribute not denoted *required* is optional, whether *recommended* or not. An optional attribute may have a default value to be assumed if omitted.  If no default is indicated, then by convention its absence should be interpreted as *unknown*, unless otherwise specified. Empty strings or null values should be interpreted the same as omitted (i.e., the default if one is specified or *unknown* otherwise).

**Note:** As a convention in this document, objects being defined are denoted with uppercase first letter in deference to the common convention for class names in programming languages such as Java, whereas actual instances of objects and references thereto in payloads are lowercase.

### Object:  RegistrationAuthority <a name="object_ra"></a>

The <code>RegistrationAuthority</code> object represents the information for a specific Registration Authority. It includes the Name, RAID, API Base URL, etc.

<table>
  <tr>
    <td><strong>Attribute&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Definition</strong></td>
  </tr>
  <tr>
    <td><code>Name</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>Name of the Registration Authority</td>
  </tr>
  <tr>
    <td><code>RAID</code></td>
    <td>string;&nbsp;required</td>
    <td>1-character unique Registration Authority Identifier</td>
  </tr>
  <tr>
    <td><code>ApiBaseUrl</code></td>
    <td>string;&nbsp;required&nbsp;*</td>
    <td>The URL root used to call the Registration Authority's UCID API, e.g. <code>https://ucid.extremereach.com</code></td>
  </tr>
  <tr>
    <td><code>Organization</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>The name of the organization hosting the Registration Authority.</td>
  </tr>
  <tr>
    <td><code>ContactEmail</code></td>
    <td>string</td>
    <td>The email address for contacting the organization hosting the Registration Authority.</td>
  </tr>
  <tr>
    <td><code>UcidVersion</code></td>
    <td>string</td>
    <td>Supported Version of the Universal Creative Identification Framework Specification (e.g., "1.0"). Default is <code>1.0</code></td>
  </tr>
  <tr>
    <td><code>RegionsCovered</code></td>
    <td>string</td>
    <td>Listing of region(s) where the Registration Authority operates.</td>
  </tr>
</table>

Some of these attributes are optional. The `RegionsCovered` attribute, for example, indicates the regions, markets, countries, etc. where this Registration Authority operates. Its utility here is more to assist in diagnostics by making the payload more self-documenting outside the context of a runtime transaction.

Most attributes are required. The `ApiBaseUrl` attribute does have runtime utility since each Registration Authority communicates with its peers via the standard protocol and must know how to connect.


### Object:  Domain <a name="object_domain"></a>

The <code>Domain</code> object represents a unique 4-character prefix assigned to a Client by a Registration Authority. It simply includes the 4-character DomainCode.

<table>
  <tr>
    <td><strong>Attribute&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Definition</strong></td>
  </tr>
  <tr>
    <td><code>DomainCode</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>Unique 4-character prefix that represents the domain</td>
  </tr>
</table>

### Object:  UCID <a name="object_ucid"></a>

The <code>UCID</code> object represents a unique creative identifier created for a Client by a Registration Authority.

<table>
  <tr>
    <td><strong>Attribute&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Definition</strong></td>
  </tr>
  <tr>
    <td><code>UCID</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>Unique creative identifier code</td>
  </tr>
  <tr>
    <td><code>Owner</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>The name of the organization/entity that owns the UCID and the Domain it belongs to</td>
  </tr>
</table>




# Detailed Implementation Guide <a name="detailedimplementationguide">

To put these concepts into practice, Registration Authorities must know how to communicate with one another, how to discover peer RAs and how to validate UCIDs. Clients of RAs must know how to authenticate, request new domains and UCIDs and how to obtain UCID metadata. Each of these operations is implemented using a REST API operation that follows a standard URI, request and response format. This section describes each of the operations that must be implemented by a RA and the behavior that each should implement. For documentation purposes, each example URI uses the host name https://ucid.io.

## Public Operations <a name="public_operations"></a>

 ### Peer-to-Peer Discovery <a name="operations-discovery"></a>
 The UCID framework is designed to allow Registration Authorities to communicate with each other in a peer-to-peer manner for validating UCIDs and to allow for discovery of new RAs.

| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.io/ra | GET | Returns the basic identification metadata for a Registration Authority, similar to a WHOIS record for an Internet domain. | [RegistrationAuthority](#object_ra) |
| https://ucid.io/ra/peers | GET | Returns the directory of peer Registration Authorities known by the RA. Each RA should periodically call the /ra-peers endpoint of each of its peers to discover any newly created Registration Authorities and update it's local peer directory. | List of [RegistrationAuthority](#object_ra) | 

 ### UCID Validation <a name="ucid_validation"></a>
 ID validation is a very important aspect of the UCID framework. Each creative identifier must be able to be validated as having been issued by one of the available Registration Authorities. This provides traceability for creative identifiers and helps ensure clobal uniqueness.
 
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.io/ucids/{creative_identifier} | GET | Validates a submitted creative_dentifier as being a UCID that was issued by the Registration Authority and returns the validated ID and basic metadata.** | [UCID](#object_ucid) or 302 Redirect |   
 
**If the submitted creative identifier was NOT issued by the RA, the RA may optionally forward the request on to its peer RAs. If one of the peers successfully validates the UCID then a HTTP 302 Redirect can be returned to the client to redirect them to the correct RA. For cases where a “legacy” creative_dentifier (e.g. an ID that is missing the RAID prefix) is submitted and validated, the returned metadata must include the fully-qualified UCID, effectively providing an automatic translation from the legacy ID format to the new UCID format.
 
## Private Operations <a name="private-operations"></a>

 ### Authentication <a name="authentication"></a>
 Private operations provided by a Registration Authority must require authentication to ensure only authorized clients are able to access the operations and to properly establish the identity of the connecting client. The UCID framework does not specify the authentication protocol and each Registration Authority may select the authentication mechanism for their API.
 
 ### Domain Operations <a name="domain_operations"></a>
 Domain operations allow a client to request creation of a new domain and obtain the list of domains registered to them.
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.io/domains | GET |  Returns the list of domains registered to the client. | List of [Domain](#object_domain) |   
| https://ucid.io/domains/{domain_code} | GET |  Checks existence of a domain registered to the client. | [Domain](#object_domain) |   
| https://ucid.io/domains | POST | Request the creation of a new domain for the client. Optionally, a specific 4-character code can be submitted to allow for “vanity domains”. If not previously used, the domain will be created, otherwise an error will be returned. If domain_code is not supplied, a new unique 4-character code will be assigned and returned. | [Domain](#object_domain) |   

 ### UCID Operations <a name="ucid_operations"></a>
 The UCID creation operation is the core operation used to request the creation of a new UCID by a RA. This operation must generate a new, unique identifier using the RA Code, the client Domain and a unique character sequence based upon either the UCID standard format, or a custom schema implemented by the RA.
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.io/ucids | POST |  Requests the creation of a new UCID. The domain parameter is optional and only needed for clients that register more than one domain. If a domain is not supplied, then the UCID is created under the client’s default domain. If successful, the newly created UCID is returned. | [UCID](#object_ucid) |   

 ### Metadata Operations <a name="metadata_operations"></a>
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.io/ucids/{creative_identifier}/metadata | GET |  Requests the metadata associated with the specified UCID. | [UCIDMetadata](#object_ucid_metadata) |   

 ## EXAMPLES <a name="examples"></a>

The following are examples of Layer-3 request/response operations and payloads expressed using JSON.

### UCID Creation <a name="examples_ucid_creation"></a>

The following is an example of a UCID creation operation.

```
Request:  POST https://ucid.io/ucids
Body:
 {
  "Domain": "ACME",
  "CustomPrefix": "",
  "CustomSuffix": "",
  "FormatCode": "H"
}
Response: 
{
  "UCID": "EACME004723H",
  "Owner": "Acme International",
  "RAID": "E"
}
```

### UCID Validation <a name="examples_ucid_validation"></a>

The following is an example of a UCID validation operation.

```
Request:  GET https://ucid.io/ucids/EACME000123H
Response: 
{
  "UCID": "EACME000123H",
  "Owner": "Acme International",
  "RAID": "E"
}
```

 ### RA Peer Discovery <a name="examples_peer_discovery"></a>

The following is an example of a RA peer discovery operation.

```
Request:  GET https://ucid.io/ra/peers
Response: 
[
  {
    "Name": "Ad-ID",
    "RAID": "A",
    "ApiBaseUrl": "https://ucid.ad-id.org"
  },
  {
    "Name": "Clearcast",
    "RAID": "C",
    "ApiBaseUrl": "https://ucid.clearcast.uk"
  },
  {
    "Name": "ISCI",
    "RAID": "I",
    "ApiBaseUrl": "https://isci.extremereach.com"
  }
]
```
 
# Appendix A:  Additional Resources <a name="appendixa_additionalresources"></a>

Reference 1
[www.extremereach.com](https://www.extremereach.com)

Creative Commons / Attribution License
[creativecommons.org/licenses/by/3.0](https://creativecommons.org/licenses/by/3.0)


Universal Creative Identification v1.0 Specification
[https://github.com/ExtremeReach/ucid](https://github.com/ExtremeReach/ucid)

JavaScript Object Notation (JSON)
[www.json.org](https://www.json.org/)

# Appendix B:  Change Log <a name="appendixb_changelog"></a>

This appendix serves as a brief summary of changes to the specification. These changes pertain only to the substance of the specification and not routine document formatting, information organization, or content without technical impact. For that, see [Appendix C: Errata](#appendixc_errata).

<table>
  <tr>
    <td><strong>Version</strong></td>
    <td><strong>Release</strong></td>
    <td><strong>Changes</strong></td>
  </tr>
  <tr>
    <td>1.0</td>
    <td>N/A</td>
    <td>Original release of Universal Creative Identification specification.</td>
  </tr>
</table>

# Appendix C:  Errata <a name="appendixc_errata"></a>

This appendix catalogues any error corrections which have been made to this document after its versioned release. The body of the document has been updated accordingly.

Only minor fixes, such as clarifications or corrections to descriptions, may be treated as errata. Improvements or material changes are summarized in the change log.

Granular details of the changes can be seen by reviewing the commit history of the document.

# Appendix D:  Versioning Policy <a name="appendixd_versioning"></a>

As of Universal Creative Identification 1.0, Universal Creative Identification's version number is only incremented on breaking changes. In other words, Universal Creative Identification 1.1 should be considered a distinct version from Universal Creative Identification 1.0 when there is a need for distinguishing versions. For example, a Registration Authority might regard the [version header](#versionheaders) when parsing a UCID request received from a client. See [Universal Creative Identification Principles](#ucid_principles).

The current version of the Universal Creative Identification Framework Specification is updated when there are non-breaking improvements to be released such as new fields, objects, or values in enumerated lists. Errata, such as clarifications or corrections to descriptions not materially impacting the specification itself, are also addressed during periodic updates. See [Errata](#appendixc_errata).

Release branches are created for each new release and the history of these can be reviewed on GitHub. The main branch for the repository will always reflect the most recent release, whereas ongoing development work occurs in the 'develop' branch.
