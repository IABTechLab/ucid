![](https://app.extremereach.com/content/images/logo_header.gif)

# Universal Creative Identification Framework Specification 1.0


**TABLE OF CONTENTS**

- [Overview](#overview)
  - [Universal Creative Identification Mission](#ucidmission)
  - [Universal Creative Identification Framework Executive Summary](#execsummary)
  - [History of Universal Creative Identification Framework](#historyofucid)
- [Regulatory Guidance](#guidance)
- [Architecture](#architecture)
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
- [Specification](#specification)
  - [Object Model](#objectmodel)
    - [Object:  RegistrationAuthority](#object_ra)
    - [Object:  Domain](#object_domain)
    - [Object:  Ucid](#object_ucid)
    - [Object:  Relationship](#object_relationship)
- [Implementation Guide](#implementation-guide)
  - [URL Character Escaping](#escaping)
  - [Public Operations](#public-operations)
    - [UCID Verification](#ucid-verification)
    - [Peer UCID Verification](#peer-verification)
    - [RA Peer Discovery](#peer-discovery)
- [Examples](#examples)
  - [Domain Creation](#examples_domain_creation)
  - [UCID Creation](#examples_ucid_creation)
  - [UCID Verification](#examples_ucid_verification)
  - [UCID Verification of "Legacy" Identifier](#examples_ucid_verification_legacy)
  - [UCID Verification of Peer-Issued Identifier](#examples_ucid_verification_peer)
  - [RA Identification](#examples_ra_identification)
- [License](#license)
- [Appendix A:  Additional Resources](#appendixa_additionalresources)
- [Appendix B:  Change Log](#appendixb_changelog)
- [Appendix C:  Errata](#appendixc_errata)
- [Appendix C:  Versioning Policy](#appendixd_versioning)


# Overview <a name="overview"></a>

## Universal Creative Identification Mission <a name="ucidmission"></a>

The mission of the Universal Creative Identification project is to provide an open, interoperable mechanism for creating and verifying globally unique identifiers used for disinguishing unique creative assets as they are produced, managed, aggregated, distributed and measured througout the marketing supply chain. By assigning each creative its own, globally unique identifier parties throughout the supply chain can use a consistent identifier when referencing a creative, regardless of where or how it is sourced or used.

## Universal Creative Identification Framework Executive Summary <a name="execsummary"></a>

This document specifies an open-standard framework for the creation and verification of globally unique identifers for creative assets. This specification aims to standardize and simplify the process for generating unique creative identifiers by multiple parties in the ecosystem. This results in an open ecosystem whereby multiple providers can participate instead of relying on a single, proprietary ID scheme, or on loosely defined formats and "honor system" uniqueness. The framework is patterned loosely after the [Internet Domain Name System](https://en.wikipedia.org/wiki/Domain_Name_System) (DNS), since Internet interoperability also depends on a clear system for ensuring uniqueness of resources. While DNS operates at a lower level of the networking stack, the Universal Creative Identification Framework operates on top of the HTTP protocol and uses standard HTTP REST API semantics to manage generation and access to creative identifiers. 

The overall goal of Universal Creative Identification is and has been to create a *uniform standard* for idenfiying creative assets used in marketing. The intent is not to specify exactly how each participating party generates unique identifiers. As a project, we aim to establish a basic standard format and facilitate interoperability between parties in the ecosystem, while ensuring global uniqueness of identifiers so that campaign execution and reporting can be performed with greater accuracy and efficiency.

## History of Universal Creative Identification Framework <a name="historyofucid"></a>

A critical element of accurate campaign measurement is the ability to differentiate which ad creative was seen by which people. Basic KPIs, such as unique reach, frequency, creative performance, etc. rely on uniquely and unambiguously identifying each ad creative and then correctly associating that identifier with individual ad exposure metrics. As simple and obvious as this may seem, it remains quite elusive to the vast majority of the marketing and measurement ecosystem. The lack of standardization in ad identification across linear, CTV, web, mobile, social and out-of-home marketing channels can cause the same ad creative to be recorded and measured as different ads. This results in highly fragmented measurement data sets that, at best, require significant manual effort to reconcile and, at worst, lead to wildly inaccurate measurement results. Universal Creative Identification (UCID) was launched as a pilot project by Extreme Reach in September 2021 to address this lack of standardization for creative identification across the global marketing ecosystem. 

# Regulatory Guidance <a name="guidance"></a>

Universal Creative Identification implementations will need to ensure compliance with all applicable regional legislation.

# Architecture <a name="architecture"></a>

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
    <td>The unique, two-character identifer that uniquely identifies a Registration Authority.</td>
  </tr>
  <tr>
    <td>RA Peer</td>
    <td>A dictionary maintained by an RA of other known RA implementations, such that verification requests can be redirected for UCID's that were generated by another RA.</td>
  </tr>
  <tr>
    <td>Client</td>
    <td>An organization/entity that wishes to create and/or verify UCIDs.</td>
  </tr>
  <tr>
    <td>Domain</td>
    <td>A 4-character prefix that contains a set of unique codes created for a specific client.</td>
  </tr>
  <tr>
    <td>Creative Metadata</td>
    <td>A set of additional attributes associated with a specific creative.</td>
  </tr>
  <tr>
    <td>Relataionship</td>
    <td>The representation of a linkage between two UCIDs that are related to each other.</td>
  </tr>
</table>


## Universal Creative Identification Principles <a name="ucid_principles"></a>

The following points define the guiding principles underlying the Universal Creative Identification Framework Specification, some of its basic rules, and its evolution.

### Registration Authorities
* The UCID framework involves designation of **Registration Authorities** that are recognized suppliers of trusted, unique creative identifiers within the creative ecosystem. The list of recognized Registration Authorities is maintained on the [Universal Creative Identification Github page]([https://www.wikipedia.org/wiki/ucid](https://github.com/IABTechLab/ucid) in the [registration-authorities.json](https://github.com/IABTechLab/ucid/blob/main/registration-authorities.json) file.
* Each Registration Authority (RA) is responsible for the following primary functions:
  * Issuing unique **Domains** for individual advertisers, agencies, etc. (clients), similar to how a company can register one or more Internet domains**.
  * Issuing **Universal Creative Identifiers**, or **UCIDs**, on behalf of authorized clients and ensuring that all issued identifiers are unique within each RA+Domain. Within a given RA+Domain a client may generate as many creative identifiers as they choose.
  * Verifying UCIDs to confirm that they were previously issued by the RA, or one of its peers.
  * Providing basic public metadata for a UCID to indicate domain owner, adverstiser, brand, creative content type, length, etc.
  * Each RA should implement all API operations and objects defined in this specification. This allows each RA to communicate with its peers by calling well-known operations against the *ApiBaseUrl* for each RA.
### RA+Domain Scoping of Creative Identifiers 
* An important element of the UCID framework is the inclusion of a **Registration Authority Identifier** (RAID) prefix on every generated identifier. This ensures that all creative identifiers are universally unique, even if two or more registries issue the same domain or ID value. This is similar to the “top level domain” (TLD) used with Internet domains (e.g. .com, .org, .edu, etc.)
* The RAID is represented by a unique three-character prefix, consisting of two alpha-numeric characters followed by a period "." delimiter. This prefix can quickly distinguish a UCID from other identifier types and indicate which registry issued the UCID, similar to the prefix of a credit card designating VISA, MasterCard, etc.
* The combination of RAID + Domain must be globally unique.
### Open Verification
* Another important feature of the UCID framework is the *open verification* mechanism. Any creative identifier can be verified by simply calling a public API operation against any recognized RA. This allows parties throughout the marketing supply chain to confirm the uniqueness and source for creative identifiers.
* The framework allows “Legacy” creative codes that do not conform to the UCID structure (do not include a valid RAID prefix code) to be verified and resolved to their UCID-compliant equivalent by querying peer RAs to verify that the code is valid and was generated by a known peer RAs.
* Clients should ensure that a UCID is assigned to *each unique creative asset* they produce and that the same UCID is used for all renditions and uses for that creative. For example, a video ad creative used across linear TV, CTV, web and social channels should use the same UCID for all usages to ensure that measurement data from various sources can be aligned to the same creative message.
* Relationships between 2 or more UCIDs may be expressed to indicate parent-child associations, aliases, derivatives, or format translations. This allows for UCID "graphs" to be established that promote discovery and interoperability. 

** Note : The UCID framework does not prescribe any commercial model to the operation of a Registration Authority. An RA provider may elect to charge a fee for registration and maintenance of ID Domains, fees for creation of individual codes, or may elect to offer the service for free as a value-add to other services.

Universal Creative Identification is a living specification. New objects and attributes may be added and enumerated lists may be extended at any time and thus implementers must accept these types of changes without breakage within a version number. See [Appendix D: Versioning Policy](#appendixd_versioning)

## Reference Model <a name="referencemodel"></a>

### Universal Creative ID Structure - Default <a name="ucid-default"></a>
The standard UCID structure for a creative identifier is an 13-character code consisting simply of an 2-character RAID + delimiter, a 4-character Domain Code and a unique sequence of 6 alpha-numeric characters, allowing up to 2,176,782,336 possible unique IDs per RA+Domain.

<center>
<table>
  <tr>
    <td colspan="3"><strong>RAID Prefix</strong></td>
    <td colspan="4"><strong>Domain Code</strong></td>
    <td colspan="6"><strong>Unique ID Value</strong></td>
  </tr>
  <tr>
    <td><pre>E</pre></td>
    <td><pre>X</pre></td>
    <td><pre>.</pre></td>
    <td><pre>A</pre></td>
    <td><pre>C</pre></td>
    <td><pre>M</pre></td>
    <td><pre>E</pre></td>
    <td><pre>0</pre></td>
    <td><pre>1</pre></td>
    <td><pre>2</pre></td>
    <td><pre>3</pre></td>
    <td><pre>4</pre></td>
    <td><pre>5</pre></td>
  </tr>
</table>
</center>

### Default Universal Creative ID Elements <a name="ucid-elements"></a>

<table>
  <tr>
    <td><strong>ID Element&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Values&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Purpose</strong></td>
  </tr>
  <tr>
    <td>RAID</td>
    <td>2 alpha-numeric characters, from 0 to Z, followed by a period "." delimiter</td>
    <td>Designates the Registration Authority that generated the UCID. Some initial RAID values could be: AD = Ad-ID, CC = Clearcast, XR = Extreme Reach, etc. This allows for up to 1296 total possible Registration Authorities.</td>
  </tr>
  <tr>
    <td>ID Domain</td>
    <td>4 alpha-numeric characters, from 0 to Z</td>
    <td>The unique ID domain issued to the client by the RA.</td>
  </tr>
  <tr>
    <td>Unique ID Value</td>
    <td>6 alpha-numeric characters, from 0 to Z</td>
    <td>A character sequence that is unique within the domain.</td>
  </tr>
</table>
 
### Universal Creative ID Structure - Example Custom Schemas <a name="ucid-custom"></a>
If supported by the RA, a client may also elect to apply a custom schema to their identifiers, such that individual positions in the generated ad code can represent creative attributes that have specific meaning to the client and other consumers of the code. This includes using extended characters, such as dash, slash, underscore, etc. within the domain or code. 

This flexibility in the schema also allows the UCID to incorporate formatting found in various "legacy" creative identifiers used across different global markets. In most cases, a legacy identifier can be converted to a UCID simply by adding an RAID prefix.

#### Encoding client-specific campaign, production year and creative format information within a UCID

<center>
<table>
  <tr>
    <td colspan="3"><strong>RAID Prefix</strong></td>
    <td colspan="4"><strong>Domain Code</strong></td>
    <td colspan="2"><strong>Campaign Year</strong></td>
    <td colspan="2"><strong>Campaign Code</strong></td>
    <td colspan="4"><strong>Unique ID Value</strong></td>
    <td><strong>Format</strong></td>
  </tr>
  <tr>
    <td><pre>E</pre></td>
    <td><pre>X</pre></td>
    <td><pre>.</pre></td>
    <td><pre>A</pre></td>
    <td><pre>C</pre></td>
    <td><pre>M</pre></td>
    <td><pre>E</pre></td>
    <td><pre>2</pre></td>
    <td><pre>3</pre></td>
    <td><pre>R</pre></td>
    <td><pre>X</pre></td>
    <td><pre>0</pre></td>
    <td><pre>9</pre></td>
    <td><pre>4</pre></td>
    <td><pre>1</pre></td>
    <td><pre>V</pre></td>
  </tr>
</table>
</center>

#### Encoding Ad-ID format within a UCID

<center>
<table>
  <tr>
    <td colspan="3"><strong>RAID Prefix</strong></td>
    <td colspan="4"><strong>Ad-ID Prefix</strong></td>
    <td colspan="7"><strong>Unique ID Value</strong></td>
    <td><strong>HD</strong></td>
  </tr>
  <tr>
    <td><pre>A</pre></td>
    <td><pre>D</pre></td>
    <td><pre>.</pre></td>
    <td><pre>A</pre></td>
    <td><pre>C</pre></td>
    <td><pre>M</pre></td>
    <td><pre>E</pre></td>
    <td><pre>0</pre></td>
    <td><pre>1</pre></td>
    <td><pre>2</pre></td>
    <td><pre>3</pre></td>
    <td><pre>4</pre></td>
    <td><pre>5</pre></td>
    <td><pre>6</pre></td>
    <td><pre>H</pre></td>
  </tr>
</table>
</center>

#### Encoding Clearcast Clock-ID format within a UCID

<center>
<table>
  <tr>
    <td colspan="3"><strong>RAID Prefix</strong></td>
    <td colspan="4"><strong>Clock-ID Prefix</strong></td>
    <td colspan="6"><strong>Clock Value</strong></td>
    <td colspan="4"><strong>Duration</strong></td>
  </tr>
  <tr>
    <td><pre>C</pre></td>
    <td><pre>C</pre></td>
    <td><pre>.</pre></td>
    <td><pre>A</pre></td>
    <td><pre>B</pre></td>
    <td><pre>C</pre></td>
    <td><pre>/</pre></td>
    <td><pre>1</pre></td>
    <td><pre>2</pre></td>
    <td><pre>3</pre></td>
    <td><pre>4</pre></td>
    <td><pre>5</pre></td>
    <td><pre>6</pre></td>
    <td><pre>/</pre></td>
    <td><pre>0</pre></td>
    <td><pre>3</pre></td>
    <td><pre>0</pre></td>
  </tr>
</table>
</center>


## Protocol Layers <a name="protocollayers"></a>

To assist in interoperability between Registration Authorities and to enable various aspects of the specification to evolve at different paces, a layered approach is being adopted as of Universal Creative Identification v1.0.  The following illustrates this model. Expressed informally, Layer-1 moves bytes between parties, Layer-2 expresses the language of these bytes, Layer-3 specifies a transaction using this language, and Layer-4 describes the goods being transacted.

The following subsections specify these layers as they pertain to the Universal Creative Identification Framework Specification. Unless explicitly specified otherwise, annotated as optional, or called out as a best practice, all material aspects of these subsections are required for Universal Creative Identification compliance.

### Layer-1:  Transport <a name="layer1_transport"></a>

#### Communications <a name="communications"></a>

The base protocol between an exchange and its demand sources is HTTP. Specifically, HTTP POST is required for Domain and UCID creation requests to and HTTP GET is used to query Registration Authorities and verify UCIDs.

Calls returning content (e.g., RA, Domain, UCID, etc. object) should return HTTP code 200.  Calls returning no content in response to valid requests should return HTTP 204. Invalid calls (e.g., a request containing a malformed or corrupt payload) should return HTTP 400 with no content.

#### Version Headers <a name="versionheaders"></a>

The Universal Creative Identification version must be passed in the header of each UCID API request with a custom header parameter. This will allow Registration Authorities and their Clients to recognize the version of the message contained before attempting to parse the request. See [Versioning Policy](#appendixd_versioning) and [Universal Creative Identification Principles](#ucid_principles) for more regarding versioning.

`X-UCID-Version: 1.0`

Additionally, while optional, it is recommended that responses include an identically formatted HTTP header with the protocol version implemented by the responder.  It is assumed, however, that any response will be compatible with the version of the request and that version support is discussed beforehand between the parties. If the version header is omitted from a request the assumed version will be 1.0.

#### Peer Headers <a name="peerheaders"></a>
When an RA forwards a request to a Peer it must include a `X-Peer-RAID` header containing its own RAID. This allows the RA recieving the request to understand that the request came from within the RA peer network and therefore should not be forwarded further, acting as a circuit breaker to recursive calls.

`X-Peer-RAID: EX`

#### Transport Security <a name="transportsecurity"></a>

As of Universal Creative Identification v1.0, HTTPS and Transport Layer Security (TLS) version 1.2+ are required for compliance and thus all connections over which the Universal Creative Identification protocol operates must be HTTPS. 

### Layer-2:  Format <a name="layer2_format"></a>

JSON (JavaScript Object Notation) is the format used for all UCID request and response data payloads. JSON was chosen for its combination of human readability and relative compactness. Given the limited data payloads used and to maintain simplicity of implementations, no other formats are included in the specification.

### Layer-3:  Transaction <a name="layer3_transaction"></a>

The Transaction Layer defines the specific API operations used for creative identifier creation and verification protocol between a client and a Registration Authority, as well as the operations used for peer-to-peer communication between Registration Authorities.

Refer to the Specification section of this document for full details.

### Layer-4:  Domain <a name="layer4_domain"></a>

The Domain Layer defines the objects on which the Transaction Layer operates. In a typical UCID creation transaction the client would issue a request to a Registration Authority to create a new UCID for a specific client domain and the newly created UCID object would be returned.

# Specification <a name="specification"></a>

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
    <td><code>name</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>Name of the Registration Authority</td>
  </tr>
  <tr>
    <td><code>RAID</code></td>
    <td>string;&nbsp;required</td>
    <td>1-character unique Registration Authority Identifier</td>
  </tr>
  <tr>
    <td><code>apiBaseUrl</code></td>
    <td>string;&nbsp;required&nbsp;*</td>
    <td>The URL root used to call the Registration Authority's UCID API, e.g. <code>https://ucid.extremereach.com</code></td>
  </tr>
  <tr>
    <td><code>organization</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>The name of the organization hosting the Registration Authority.</td>
  </tr>
  <tr>
    <td><code>contactEmail</code></td>
    <td>string</td>
    <td>The email address for contacting the organization hosting the Registration Authority.</td>
  </tr>
  <tr>
    <td><code>ucidVersion</code></td>
    <td>string</td>
    <td>Supported Version of the Universal Creative Identification Framework Specification (e.g., "1.0"). Default is <code>1.0</code></td>
  </tr>
  <tr>
    <td><code>regionsCovered</code></td>
    <td>string</td>
    <td>Listing of region(s) where the Registration Authority operates.</td>
  </tr>
  <tr>
    <td><code>legacyCodeFormat</code></td>
    <td>string</td>
    <td>Regular expression defining non UCID code formats that can be verified. Enables an RA to search for peer that can potentially verify a code with a RAID that is not recognized.</td>
  </tr>
</table>

Some of these attributes are optional. The `regionsCovered` attribute, for example, indicates the regions, markets, countries, etc. where this Registration Authority operates. Its utility here is more to assist in diagnostics by making the payload more self-documenting outside the context of a runtime transaction.

Most attributes are required. The `apiBaseUrl` attribute does have runtime utility since each Registration Authority communicates with its peers via the standard protocol and must know how to connect.


### Object:  Domain <a name="object_domain"></a>

The <code>Domain</code> object represents a unique 4-character prefix assigned to a Client by a Registration Authority. It simply includes the 4-character DomainCode.

<table>
  <tr>
    <td><strong>Attribute&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Definition</strong></td>
  </tr>
  <tr>
    <td><code>domainCode</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>Unique 4-character prefix that represents the domain</td>
  </tr>
  <tr>
    <td><code>owner</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>The name of the organization/entity that owns the domain</td>
  </tr>
  <tr>
    <td><code>domainName</code></td>
    <td>string</td>
    <td>"Friendly" name associated with domain, e.g. associated advertiser, brand, internet domain, etc.</td>
  </tr>
</table>

### Object:  UCID <a name="object_ucid"></a>

The <code>UCID</code> object represents a unique creative identifier created for a Client by a Registration Authority. Optionally, a set of related UCID objects can be represented as well.

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
    <td><code>owner</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>The name of the organization/entity that owns the UCID and the Domain it belongs to</td>
  </tr>
  <tr>
    <td><code>uri</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>The fully qualified URI that returns this UCID from the RA that issued it</td>
  </tr>
  <tr>
    <td><code>advertiser</code></td>
    <td>string</td>
    <td>The advertiser associated with the creative identified by the UCID</td>
  </tr>
  <tr>
    <td><code>brand</code></td>
    <td>string</td>
    <td>The brand associated with the creative identified by the UCID</td>
  </tr>
  <tr>
    <td><code>product</code></td>
    <td>string</td>
    <td>The product associated with the creative identified by the UCID</td>
  </tr>
  <tr>
    <td><code>creativeType</code></td>
    <td>string</td>
    <td>The primary mime-type category of the creative identified by the UCID (e.g. video, audio, image, text, etc.)</td>
  </tr>
  <tr>
    <td><code>creativeDuration</code></td>
    <td>string</td>
    <td>The duration in seconds of the creative identified by the UCID (for static content a duration of 0 is used)</td>
  </tr>
  <tr>
    <td><code>relationships</code></td>
    <td>List of <code>Relationship</code></td>
    <td>The optional set of related UCIDs that this UCID is related to</td>
  </tr>
</table>

### Object:  Relationship <a name="object_relationship"></a>

The <code>Relationship</code> object represents another identifier that is related to a UCID. It also allows for the expression of various types of relationships, such as a parent, child, sibling or alias. The <code>alias</code> relationship allows a UCID to be linked to other creative identifiers, such as legacy codes, house numbers, watermarks, fingerprints, etc. The <code>parent</code>, <code>child</code> and <code>sibling</code> relationships can be used to indicate hierarchies or associations with different versions of a creative. 
When the identifier specified in a <code>Relationship</code> is another UCID, then the corresponding <code>uri</code> property would be the fully qualified RA API URL that returns the associated UCID metadata. If the identifer represents a non-UCID identifier, then the <code>uri</code> property would hold the URL to return the associated object representation.

The relationship construct provides a very flexible means of associating other identifiers, objects and metadata sets with a UCID.

<table>
  <tr>
    <td><strong>Attribute&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong></td>
    <td><strong>Definition</strong></td>
  </tr>
  <tr>
    <td><code>identifier</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>Creative identifier code</td>
  </tr>
  <tr>
    <td><code>uri</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>The fully qualified URI that returns an representation of the object referenced by the identifier</td>
  </tr>
  <tr>
    <td><code>type</code></td>
    <td>string;&nbsp;required&nbsp;</td>
    <td>One of the following: 'Parent', 'Child', 'Sibling' or 'Alias'</td>
  </tr>
</table>


# Implementation Guide <a name="implementation-guide"></a>

To put these concepts into practice, Registration Authorities must know how to communicate with one another, how to discover peer RAs and how to verify UCIDs. Clients of RAs must know how to authenticate, request new domains and UCIDs and how to obtain UCID metadata. Each of these operations is implemented using a REST API operation that follows a standard URI, request and response format. This section describes each of the operations that must be implemented by a RA and the behavior that each should implement. For documentation purposes, each example below uses the example RA host name https://ucid.example.com, which uses the RAID "EX".

## URL Character Escaping <a name="escaping"></a>
For API GET operations that include the UCID in the URL path or query string, the UCID value must be URL-escaped to encode any reserved non-alphanumeric characters, such as colon, forward slash, etc. For example, a verification GET operation for the UCID code **EX.ABC/12345/030** would be escaped as [https://ucid.example.com/ucids/EX.ABC%2F12345%2F030](https://ucid.example.com/ucids/EX.ABC%2F12345%2F030) 

## Public Operations <a name="public-operations"></a>

 ### UCID Verification <a name="ucid-verification"></a>
 Identifer verification is a key feature of the UCID framework. Each creative identifier must be able to be verified as having been issued by one of the available Registration Authorities. This provides traceability for creative identifiers and helps ensure clobal uniqueness.
 
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.example.com/ucids/{creative_identifier} | GET | Verifies a submitted, [URL-escaped](#escaping) creative_dentifier as being a UCID that was issued by the Registration Authority and returns the verified [UCID](#object_ucid) object and associated metadata.** | [UCID](#object_ucid), 404 Not Found or 301 Redirect (see [Peer UCID Verification](#peer_verification)) |   
 
 ### Peer UCID Verification <a name="peer-verification"></a>
If a UCID cannot be verified within the receiving RA, the request should be forwarded to one or more peer RAs. The initial RA should first attempt to resolve the appropriate peer RA from the RAID prefix of the requested UCID. It will then reformat the request to target the `apiBaseUri` of the resolved peer RA and add the `X-Peer-RAID` HTTP Header. The reformatted request is then forwarded to the peer RA. If a valid UCID response is received from the peer RA the initial RA should return a `301 Redirect` response to the caller with the `Location` attribute set to the `Uri` property of the peer UCID response.
If the peer RA returns a `404 Not Found`, or for cases where a “legacy” creative_dentifier is submitted that does not match a known, registered RAID prefix, then the above process is executed against all known peer RAs where the code matches the `legacyCodeFormat`. The first valid peer response should trigger the `301 Redirect` back to the caller.
If no UCID can be resolved from either direct verification or peer verification, then a `404 Not Found` response should be returned.

### Peer-to-Peer Discovery <a name="peer-discovery"></a>
 The UCID framework is designed to operate as a federated model, allowing Registration Authorities to communicate with each other in a peer-to-peer manner for verifying UCIDs and to allow for discovery of new RAs.

| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.example.com/ra | GET | Returns the basic identification metadata for a Registration Authority, similar to a WHOIS record for an Internet domain. | [RegistrationAuthority](#object_ra) |
| https://ucid.example.com/ra/peers | GET | Returns the directory of peer Registration Authorities known by the RA. Each RA should periodically call the /ra-peers endpoint of each of its peers to discover any newly created Registration Authorities and update it's local peer directory. Such checks also enable a RA to remove/mark offline peers that no longer respond,  | List of [RegistrationAuthority](#object_ra) | 
 
## Private Operations <a name="private-operations"></a>

 ### Authentication <a name="authentication"></a>
 Private operations provided by a Registration Authority must require authentication to ensure only authorized clients are able to access the operations and to properly establish the identity of the connecting client. The UCID framework specifies that HTTP Authorization header is used to present a [Bearer]() token to the service, but does not specify how that token is generated and/or maintained meaning that each Registration Authority may select the authentication mechanism for their API.

 ### Registry Authority Operations <a name="ra_operations"></a>
 Registry Authority operations allow a entiy/organization to request registration of a new RA against an existing RA in order to build out the peer netwrok.
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.example.com/ra | POST |  Submits basic metadata for an RA to be added to the network. Upon submission the RA will be in pending status. Pending RA's are automatically checked for conformity to the specification before being given a status of approved. An RA can elect not to implement the automatic approval mechanism and wait for another peer in the network to carry out the check. Once approved it is up to the RA organisation to move to a status of online (following appropriate possibly manual checks deemed necessary by the RA) meaning the RA is accessible and ready to verify codes within the network. | [RegistrationAuthority](#object_ra) 

 ### Domain Operations <a name="domain_operations"></a>
 Domain operations allow a client to request creation of a new domain and obtain the list of domains registered to them.
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.example.com/domains | GET |  Returns the list of domains registered to the client. | List of [Domain](#object_domain) |   
| https://ucid.example.com/domains/{domain_code} | GET |  Checks existence of a domain registered to the client. | [Domain](#object_domain) |   
| https://ucid.example.com/domains | POST | Request the creation of a new domain for the client. Optionally, a specific 4-character code can be submitted to allow for “vanity domains”. If not previously used, the domain will be created, otherwise an error will be returned. If domain_code is not supplied, a new unique 4-character code will be auto-generated and returned. | [Domain](#object_domain) |   

 ### UCID Operations <a name="ucid_operations"></a>
 The UCID creation operation is the core operation used to request the creation of a new UCID by a RA. This operation must always generate a new, unique identifier using the RAID, the client Domain and a unique character sequence based upon either the UCID standard format, or a custom schema implemented by the RA. If a "customIdentifier" value supplied in the request, it is checked for uniqueness and, optionally, conformance with the schema enforced by the RA. If the supplied "customIdentifier" value would result in a non-unique, or non-conformant, UCID value, the request should return an error. If no "customIdentifier" value is supplied in the request, a new, generic unique alpha-numeric code is auto-generated by the RA. If a "customPrefix" and/or "customSuffix" is supplied in the request, these are prepended and/or appended to the generic unique code.
 
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.example.com/ucids | POST |  Requests the creation of a new UCID. The domain parameter is optional and only needed for clients that register more than one domain. If a domain is not supplied, then the UCID is created under the client’s default domain. If successful, the newly created UCID is returned. | [UCID](#object_ucid) |   

 ### Metadata Operations <a name="metadata_operations"></a>
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.example.com/ucids/{creative_identifier}/metadata | GET |  Requests the metadata associated with the specified UCID. | [UCIDMetadata](#object_ucid_metadata) |   

 ### Relationship Operations <a name="relationship_operations"></a>
| Operation | HTTP Verb | Desription | Return Type|
| --------- |-----------| -----------| ---------- |
| https://ucid.example.com/ucids/{creative_identifier}/relationships | POST |  Adds a new UCID relationship to the specified UCID. | [UCID](#object_ucid) |   
| https://ucid.example.com/ucids/{creative_identifier}/relationships/{related_creative_identifier} | DELETE |  Removes a UCID relationship from the specified UCID. | [UCID](#object_ucid) |   


 
## EXAMPLES <a name="examples"></a>

The following are examples of Layer-3 request/response operations and payloads expressed using JSON.

### Domain Creation <a name="examples_domain_creation"></a>

The following is an example of a Domain creation operation.

```
Request:  POST https://ucid.example.com/domains
Body:
 {
  "domainCode": "ACME",
  "domainName": "acme.com",
  "owner": "Acme International"
}
Response: 
{
  "domainCode": "ACME",
  "domainName": "acme.com",
  "owner": "Acme International"
}
```

### UCID Creation <a name="examples_ucid_creation"></a>

The following is an example of a UCID creation operation.

```
Request:  POST https://ucid.example.com/ucids
Body:
 {
  "domain": "ACME",
  "customIdentifier": "",
  "customPrefix": "",
  "customSuffix": "",
  "advertiser": "Acme",
  "brand": "Coyote Brands",
  "product": "Invisible Paint",
  "creativeType": "video",
  "creativeDuration": "30"
}
Response: 
{
  "UCID": "EX.ACME004723",
  "owner": "Acme International",
  "uri": "https://ucid.example.com/ucids/EX.ACME004723",
  "advertiser": "Acme",
  "brand": "Coyote Brands",
  "product": "Invisible Paint",
  "creativeType": "video",
  "creativeDuration": "30",
  "relationships": []
}
```

### UCID Verification <a name="examples_ucid_verification"></a>

The following is an example of a UCID verification operation.

```
Request:  GET https://ucid.example.com/ucids/EX.ACME000123
Response: 
{
  "UCID": "EX.ACME000123H",
  "owner": "Acme International",
  "uri": "https://ucid.example.com/ucids/EX.ACME000123",
  "advertiser": "Acme",
  "brand": "Coyote Brands",
  "product": "Invisible Paint",
  "creativeType": "video",
  "creativeDuration": "30",
  "relationships": []
}
```

### UCID Verification of "Legacy" Identifier <a name="examples_ucid_verification_legacy"></a>

The following is an example of a UCID verification operation where a "legacy" identifer was passed in request. The RA translates the identifer to its correct UCID format and returns response with legacy code as an "alias" relationship.

```
Request:  GET https://ucid.example.com/ucids/ACME000123
Response: 
{
  "UCID": "EX.ACME000123",
  "owner": "Acme International",
  "uri": "https://ucid.example.com/ucids/EX.ACME000123",
  "advertiser": "Acme",
  "brand": "Coyote Brands",
  "product": "Invisible Paint",
  "creativeType": "video",
  "creativeDuration": "30",
  "relationships": 
  [
    {
      "UCID": "ACME000123",
      "relationshipType": "Alias",
      "uri": "https://ucid.example.com/ucids/ACME000123"
    }
  ]
}
```

### UCID Verification of Peer-Issued Identifier <a name="examples_ucid_verification_peer"></a>

The following is an example of a UCID verification operation where an identifer issued by an RA peer was passed in the request. The RA resolves the issuing RA from RAID, proxies to call to the Peer RA, and returns the response.

```
Request:  GET https://ucid.example.com/ucids/PX.ABCD000123H
Response: 
{
  "UCID": "PX.ABCD000123H",
  "owner": "ABCD Inc.",
  "uri": "https://ucid.peer-ra.com/ucids/PX.ABCD000123H",
  "relationships": []
}
```
Note: The response contains the uri for the peer RA.

### RA Identification <a name="examples_ra_identification"></a>

The following is an example of a RA identification operation.

```
Request:  GET https://ucid.example.com/ra
Response: 
{
  "Name": "Example Registration Authority",
  "RAID": "EX",
  "ApiBaseUrl": "https://ucid.example.com",
  "Organization": "Example Organization",
  "ContactEmail": "ucid@example.com",
  "RegionsCovered": "*"
}
```
### RA Peer Discovery <a name="examples_peer_discovery"></a>

The following is an example of a RA peer discovery operation.

```
Request:  GET https://ucid.example.com/ra/peers
Response: 
[
  {
    "name": "Extreme Reach",
    "RAID": "EX",
    "apiBaseUrl": "https://ucid.extremereach.com"
  },
  {
    "name": "Ad-ID",
    "RAID": "AD",
    "apiBaseUrl": "https://ucid.ad-id.org"
  },
  {
    "name": "Clearcast",
    "RAID": "CC",
    "apiBaseUrl": "https://ucid.clearcast.uk"
  },
]
```

# License <a name="license"></a>

Universal Creative Identification Framework Specification is licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.txt).


# Appendix A:  Additional Resources <a name="appendixa_additionalresources"></a>

Extreme Reach
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
    <td><strong>Date</strong></td>
    <td><strong>Changes</strong></td>
  </tr>
  <tr>
    <td>1.0</td>
    <td>September, 2021</td>
    <td>Original release of Universal Creative Identification specification.</td>
  </tr>
  <tr>
    <td>1.0</td>
    <td>January, 2024</td>
    <td>Revisions to specification to modify RAID delimiter and UCID metadata payload.</td>
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

