## Application fields

Core planning application structure containing reference information,
application types, submission details, modules, documents, and fees

**Application fields module**

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A unique reference for the data item | MUST |
application-types | Application types[] | A list of planning application types that define the nature of the planning application | MUST | Select from the **application-type** enum
application-sub-type | Application sub type | Further classification of the application type for specific variations within the main application type | MAY | Select from the **application-subtype** enum
planning-authority | Planning authority | A reference of the planning authority the application has been submitted to, e.g. local-authority:CMD for London borough of Camden | MUST | Select from the **planning-authority** enum. Currently built by combining local-authority, development-corporation and national-park-authority datasets from planning.data.gov.uk
submission-date | Submission date | Date the application is submitted in YYYY-MM-DD format | MUST |
modules | Modules[] | List of required modules for this application that can be used to validate the application | MUST |
documents | Documents[]{} | List of submitted documents with references and details | MUST |
fee | Fee{} | The fee payable for the application including amounts and transaction details | MAY |


**Document component**

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A reference for the document | MUST |
name | Name | The name or title of the document | MUST |
description | Description | Brief description of what the document contains | MAY |
document-types | Document types[] | List of codelist references that the document covers | MUST | Select from the **planning-requirement** enum
file | File{} | The digital file or a reference to where the file is stored | MUST |


**Fee component**

field | name | description | required | notes
-- | -- | -- | -- | --
amount | Amount | The total amount due for the application fee | MUST |
amount-paid | Amount paid | The amount paid towards the application fee | MUST |
transactions | Transactions[] | References to payments or financial transactions related to this application | MAY |


**File component**

field | name | description | required | notes
-- | -- | -- | -- | --
url | URL | A URL pointing to the stored file | MAY |
base64-content | Base64 | Base64-encoded content of the file for inline file uploads | MAY |
filename | Filename | Name of the file being uploaded | MUST |
mime-type | MIME type | The file's MIME type such as application/pdf or image/jpeg | MAY |
checksum | Checksum | Hash of the file contents used for file validation and checking files have not been tampered with | MAY |
file-size | File size | Size of the file in bytes that can be used to enforce limits | MAY |

**Validation rules**

- reference must be a valid UUID format
- application-types must reference valid application type codelist values
- application-sub-type must reference valid application sub-type codelist values
- planning-authority must be a valid organisation reference
- modules must reference existing module definitions
- document references must be unique within the application
- file must contain either url or base64, but not both
- document-types must reference valid planning requirement codelist values

**Supporting document component**

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A unique reference for the data item | MUST |
name | Name | A name for the document. For example, The Site Plan | MUST |

**Validation rules**

- All fields must use values from rights-of-way-answers codelist
- If new-altered-vehicle is yes, details must be provided in highways module
- If change-right-of-way is yes, separate rights of way order may be needed
- If temp-right-of-way is yes, details of temporary diversions must be provided
- each document in supporting-documents must have a `reference` that matches a document in application.documents

## Agent contact details

Name and contact information if an agent is being used.

**Agent contact details module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| agent-reference | Agent reference | A reference to an agent object | MUST |  |
| contact-details | Contact details{} | A structured object containing contact information for an individual. This component is required for planning in principle (PiP) applications and optional for other application types. Contains email and phone contact information. | MUST |  |


**Contact details component**

field | name | description | required | notes
-- | -- | -- | -- | --
email | Email | The email address that can be used for electronic correspondence with the individual | MUST |
phone-numbers | Phone number(s)[]{} | One or more telephone numbers to contact individual | MUST |


**Phone number component**

field | name | description | required | notes
-- | -- | -- | -- | --
number | Phone number | A phone number | MAY |
contact-priority | Contact priority | The priority of a number | MAY | Select from the **contact-priority** enum


## Agent details

Name and contact information if an agent is being used.

**Agent details module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| agent | agent{} | Details of the agent | MAY |  |


**Agent obj component**

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A unique reference for the data item | MUST |
person | Person{} | Detail to help identify a person | MUST |
company | Company | The name of a company (that the agent works for) | MAY |
user-role | User role | The role of the named individual. Agent or proxy | MAY | Select from the **user-role-type** enum


**Person obj component**

field | name | description | required | notes
-- | -- | -- | -- | --
title | Title | The title of the individual | MAY |
first-name | First Name | The first name of the individual | MUST |
last-name | Last Name | The last name of the individual | MUST |
address-text | Address Text | Flexible field for capturing addresses | MUST |
postcode | Postcode | The postal code | MAY |


## Applicant contact details

Telephone number and email address of the applicant.

**Applicant contact details module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| applicant-reference | Applicant reference | Reference to match contact details to a named individual from the applicant details component | MUST |  |
| contact-details | Contact details{} | A structured object containing contact information for an individual. This component is required for planning in principle (PiP) applications and optional for other application types. Contains email and phone contact information. | MUST |  |


**Contact details component**

field | name | description | required | notes
-- | -- | -- | -- | --
email | Email | The email address that can be used for electronic correspondence with the individual | MUST |
phone-numbers | Phone number(s)[]{} | One or more telephone numbers to contact individual | MUST |


**Phone number component**

field | name | description | required | notes
-- | -- | -- | -- | --
number | Phone number | A phone number | MAY |
contact-priority | Contact priority | The priority of a number | MAY | Select from the **contact-priority** enum

**Validation rules**

- applicant-reference must match a reference from the applicant details component
- At least one phone number must have contact-priority set to primary

## Applicant details

Name and contact information for the parties making the application.

**Applicant details module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| applicants | Applicants[]{} |  | MUST |  |


**Applicant component**

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A unique reference for the data item | MUST |
person | Person{} | Detail to help identify a person | MUST |


**Person obj component**

field | name | description | required | notes
-- | -- | -- | -- | --
title | Title | The title of the individual | MAY |
first-name | First Name | The first name of the individual | MUST |
last-name | Last Name | The last name of the individual | MUST |
address-text | Address Text | Flexible field for capturing addresses | MUST |
postcode | Postcode | The postal code | MAY |

**Validation rules**

- At least one applicant must be provided
- Each applicant reference must be unique within the application

## Checklist

Checking whether all the requirements of the form have been met, such as proof of payment or supporting documentation.

**Checklist module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| national-req-types | National requirement types[] | List of the document types required for the given application type | MUST |  |

**Validation rules**

- All values must be from the national-requirement-type codelist
- Values must be valid for the current application type

## Conflict of interest

Details of any conflict of interest that may exist between the applicant and relevant authority.

**Conflict of interest module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| conflict-to-declare | Conflict to declare | Indicates whether any named applicant or agent has a relationship to the planning authority that must be declared | MUST |  |
| conflict-person-name | Conflict person name | Name of the individual with the conflict of interest that matches one of the names provided in applicants/agent section | MAY | Rule: is a MUST if `conflict-to-declare` is `True` |
| conflict-details | Conflict details | Details of the conflict of interest including name, role and how the individual is related to the planning authority | MAY | Rule: is a MUST if `conflict-to-declare` is `True` |

**Validation rules**

- conflict-person-name must match a name provided in applicants or agent sections

## Declaration

Signed and dated verification of the application's accuracy.

**Declaration module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| name | Name | A name of a person | MUST |  |
| declaration-confirmed | Declaration confirmed | Confirms the applicant or agent has reviewed and validated the information provided in the application | MUST |  |
| declaration-date | Declaration date | The date the declaration was made | MUST |  |

**Validation rules**

- name must match one of the named individuals in the application
- declaration-date must be in YYYY-MM-DD format
- declaration-date must not be in the future

## Foul sewage disposal

How waste water will leave the property as part of the proposed development

**Foul sewage disposal module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| has-new-disposal-arrangements | Has new disposal arrangements | Does the proposal include any new foul sewage disposal arrangments | MUST |  |
| foul-sewage-disposal-types | Foul sewage disposal types[] | List of ways foul sewage will be disposed of | MAY | Select from the **foul-sewage-disposal-type** enum |
| produce-foul-sewage | Produce foul sewage | Whether the proposed development will produce any foul sewage | MUST |  |
| connect-to-drainage-system | Connect to drainage system | Whether the proposal needs to connect to the existing drainage system | MUST |  |
| drainage-system-details | Drainage system details | Details of the drawings/plans that show the existing drainage system | MAY |  |

**Validation rules**

- if connect-to-drainage-system == true then drainage-system-details is required
- if application-type includes 'extraction-oil-gas' then drainage-system-details is required

## Hazardous substances

What hazardous substances may be used as part of the development

**Hazardous substances module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| involves-hazardous-substances | Involves hazardous substances | Indicates if hazardous substances are involved in the proposal | MUST | Select from the **yes-no-not-applicable** enum |
| substance-types | Substance types[]{} | List of hazardous substances and their quantities | MAY | Rule: is a MUST if `involves-hazardous-substances` is `yes` |
| hazardous-sub-consent-req | Hazardous substance consent required | Does the proposal involve the use or storage of any substances requiring hazardous substances consent | MUST |  |
| hazardous-sub-consent-details | Hazardous substance consent details | Details of hazardous substance consent requirements | MAY | Rule: is a MUST if `hazardous-sub-consent-req` is `True` |


**Hazardous substance component**

field | name | description | required | notes
-- | -- | -- | -- | --
hazardous-substance-type | Hazardous substance type | Reference of hazardous substance type from predefined list | MUST | Select from the **hazardous-sub-type** enum
hazardous-substance-other | Hazardous substance other | The specific name of the hazardous substance if other is selected | MAY | Rule: is a MUST if `hazardous-substance-type` is `other`
amount | Amount | The total amount due for the application fee | MUST |

**Validation rules**

- if involves-hazardous-substances == 'yes' then substance-types is required
- if hazardous-sub-consent-req == true then hazardous-sub-consent-details is required
- if hazardous-substance-type == 'other' then name is required
- amount > 0

## Materials

What materials are being used for the proposed development

**Materials module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| building-elements | Building elements[]{} | Details of materials for a specific building element such as walls, roof, windows or doors | MUST |  |
| providing-additional-material-information | Providing additional material information | Is the applicant providing additional materials information on submitted plan(s)/drawing(s)/design and access statement? | MUST |  |
| supporting-documents | Supporting documents[]{} | Supporting documents that provide additional information about the materials to be used | MAY | Rule: is a MUST if `providing-additional-material-information` is `True` |


**Building element component**

field | name | description | required | notes
-- | -- | -- | -- | --
building-element-type | Building element type | The part of building the materials relate to, such as walls, roofs, windows, or doors | MUST | Select from the **building-element-type** enum
existing-materials | Existing materials | Description of the materials currently used for this building element | MAY |
proposed-materials | Proposed materials | Description of the materials proposed for this building element as part of the development | MAY |
materials-not-applicable | Materials not applicable | Indicates this building element is not relevant to the application | MAY |
materials-not-known | Materials not known | Indicates the materials for this building element are not yet known | MAY |


**Supporting document component**

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A unique reference for the data item | MUST |
name | Name | A name for the document. For example, The Site Plan | MUST |

**Validation rules**

- Each building-element must have a unique building-element-type
- At least one of: existing-materials, proposed-materials, materials-not-applicable or materials-not-known must be provided for each building-element
- materials-not-applicable cannot be true if existing-materials or proposed-materials is provided
- materials-not-known cannot be true if existing-materials or proposed-materials is provided
- supporting-documents must reference valid documents in the application

**Notified person component**

field | name | description | required | notes
-- | -- | -- | -- | --
person | Person{} | details of the owner (or tenant when not a listed building consent application) | MAY |
notice-date | Notice date | Date when notice was served | MAY |


**Newspaper notice component**

field | name | description | required | notes
-- | -- | -- | -- | --
newspaper-name | Newspaper name | Name of the newspaper where notice was published | MUST |
publication-date | Publication date | Date when the notice was published | MUST |


**Person obj component**

field | name | description | required | notes
-- | -- | -- | -- | --
title | Title | The title of the individual | MAY |
first-name | First Name | The first name of the individual | MUST |
last-name | Last Name | The last name of the individual | MUST |
address-text | Address Text | Flexible field for capturing addresses | MUST |
postcode | Postcode | The postal code | MAY |


## Site area

How big the site is including relevant measurements

**Site area module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| site-area-in-hectares | Site area in hectares | The size of the site in hectares | MUST |  |
| site-area-provided-by | Site area provided by | Who provided the site area value | MAY | Select from the **provided-by** enum |

**Validation rules**

- site-area-in-hectares must be a positive number
- Authority can use site-area-provided-by to determine if calculation verification is needed
- site-area-in-hectares is measured in hectares

## Site details

Where the proposed development will be built.

**Site details module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| site-locations | Site locations[]{} | Details of the sites where development or works are proposed | MUST |  |


**Site location component**

field | name | description | required | notes
-- | -- | -- | -- | --
site-boundary | Site boundary | Geometry of the site of the development, typically in GeoJSON format | MAY |
address-text | Address Text | Flexible field for capturing addresses | MAY |
postcode | Postcode | The postal code | MAY |
easting | Easting | Easting coordinate in British National Grid (EPSG:27700) | MAY |
northing | Northing | Northing coordinate in British National Grid (EPSG:27700) | MAY |
latitude | Latitude | Latitude coordinate in WGS84 (EPSG:4326) | MAY |
longitude | Longitude | Longitude coordinate in WGS84 (EPSG:4326) | MAY |
description | Description | A text description providing details about the subject. For parking changes, this describes how the proposed works affect existing car parking arrangements. | MAY |
uprns | UPRNs[] | Unique Property Reference Numbers (UPRNs) for properties within the site boundary | MAY |

**Validation rules**

- {'description': 'At least one site-location must be provided for tree works applications', 'field': 'site-locations', 'require': {'min': 1}, 'type': 'count-constraint', 'when': {'application-type': {'in': ['tree-works']}}}
- {'description': 'Exactly one site-location for all other applications types', 'field': 'site-locations', 'require': {'exact': 1}, 'type': 'count-constraint', 'when': {'application-type': {'not': ['tree-works']}}}
- If easting is provided, northing must also be provided and vice versa
- If latitude is provided, longitude must also be provided and vice versa
- Site boundary must be valid GeoJSON
- UPRNs must be valid format
- Post code must be valid UK format

## Site Visit Details

Information to help the planning authority arrange a site visit

**Site Visit Details module**

| reference | name | description | requirement | notes |
| --- | --- | --- | --- | --- |
| can-be-seen-from | Site seen from public area | Can site be seen from a public road, public footpath, bridleway or other public land | MUST |  |
| contact-type | Site visit contact type | Indicates who the authority should contact to arrange a site visit | MUST | Select from the **site-visit-contact-type** enum |
| contact-reference | Contact reference | The reference of the applicant or agent who should be contacted for site visits | MAY |  |
| other-contact | Other site visit contact{} | Details of specifically named contact for site visits | MAY | Rule: is a MUST if `contact-type` is `other` |


**Other contact component**

field | name | description | required | notes
-- | -- | -- | -- | --
fullname | Full name | The complete name of a person | MUST |
number | Phone number | A phone number | MUST |
email | Email | The email address that can be used for electronic correspondence with the individual | MUST |

**Validation rules**

- contact-reference must match agent-details.agent.reference details if contact-type is agent
- contact-reference must match one of the references in applicant-details.applicants if contact-type is applicant
- When contact-type is other, full contact details must be provided

### Building element type

| reference | name | description | application-types | notes |
| --- | --- | --- | --- | --- |
| walls | Walls | A vertical construction that bounds or subdivides spaces | advertising;demolition-con-area;full;hh;outline | Referring to same thing as IfcWall |
| roof | Roof | A covering of the top part of a building, it protects the building against the effects of weather | advertising;demolition-con-area;full;hh;outline | Referring to same thing as IfcRoof |
| windows | Windows |  | advertising;demolition-con-area;full;hh;outline |  |
| doors | Doors |  | advertising;demolition-con-area;full;hh;outline |  |
| boundary-treatments | Boundary treatments |  | advertising;demolition-con-area;full;hh;lbc;outline |  |
| vehicle-access-hard-standings | Vehicle access and hard-standings |  | advertising;demolition-con-area;full;hh;lbc;outline |  |
| lighting | Lighting |  | advertising;demolition-con-area;full;hh;lbc;outline |  |
| external-walls | External walls |  | lbc |  |
| roof-covering | Roof covering |  | lbc |  |
| chimney | Chimney |  | lbc |  |
| external-doors | External doors |  | lbc |  |
| ceilings | Ceilings |  | lbc |  |
| internal-walls | Internal walls |  | lbc |  |
| floors | Floors |  | lbc |  |
| internal-doors | Internal doors |  | lbc |  |
| rainwater-goods | Rainwater goods |  | lbc |  |
| other | Other |  | lbc |  |

### Contact priority

| reference | name | description |
| --- | --- | --- |
| primary | Primary | The preferred item to use |
| secondary | Secondary | The option to use if primary is not working |

### Day type

| reference | name | description |
| --- | --- | --- |
| monday-friday | Monday to Friday |  |
| saturday | Saturday |  |
| sunday | Sunday |  |
| bank-holiday | Bank holiday |  |

### Hazardous substance type

| reference | name | description |
| --- | --- | --- |
| acrylonitrile | Acrylonitrile |  |
| ammonia | Ammonia |  |
| bromine | Bromine |  |
| chlorine | Chlorine |  |
| ethylene-oxide | Ethylene oxide |  |
| flour | Flour |  |
| hydrogen-cyanide | Hydrogen cyanide |  |
| liquid-oxygen | Liquid oxygen |  |
| liquid-petroleum-gas | Liquid petroleum gas |  |
| phosgene | Phosgene |  |
| refined-white-sugar | Refined white sugar |  |
| sulphur-dioxide | Sulphur dioxide |  |

### Housing type

| reference | name | description |
| --- | --- | --- |
| houses | Houses | Detached |
| flats-maisonettes | Flats/Maisonettes | Self-contained apartments or maisonettes. |
| sheltered-housing | Sheltered Housing | Housing with support for older or disabled people. |
| bedsit-studio | Bedsit/Studio | Single-room living spaces. |
| cluster-flats | Cluster Flats | Flats with shared communal areas. |
| other | Other | Any other housing type not listed. |
| live-work-units | Live-Work Units | Properties combining residential and workspace. |
| unknown | Unknown | When the type of housing is uncertain. |

### Tenure type

| reference | name | description | application-types |
| --- | --- | --- | --- |
| market-housing | Market Housing | Private housing for sale or rent. | ldc;full;outline |
| social-rented | Social Rented Housing | Public/social housing at below-market rents. | ldc |
| intermediate-housing | Intermediate Housing | Housing with rents or ownership costs between social housing and market housing. | ldc |
| key-worker-housing | Key Worker Housing | Housing for essential workers (e.g. teachers |  NHS staff). |
| affordable-rent | Social |  Affordable |  or Intermediate Rent |
| home-ownership | Affordable Home Ownership | Shared ownership or similar schemes. | full;outline |
| starter-homes | Starter Homes | Discounted homes for first-time buyers. | full;outline |
| custom-build | Self-Build and Custom Build | Homes built or commissioned by individuals. | full;outline |

### Use class

| reference | name | description | notes | entry-date | start-date | end-date |
| --- | --- | --- | --- | --- | --- | --- |
| b2 | B2 – General Industrial | Industrial uses not falling within Class E. |  | 2025-10-27 |  |  |
| b8 | B8 – Storage and Distribution | Warehousing and storage. |  | 2025-10-27 |  |  |
| c1 | C1 – Hotels | Includes hotels, boarding houses, and guest houses. |  | 2025-10-27 |  |  |
| c2 | C2 – Residential Institutions | Care homes, hospitals, and boarding schools. |  | 2025-10-27 |  |  |
| c2a | C2A – Secure Residential Institutions | Prisons, young offender institutions. |  | 2025-10-27 |  |  |
| c3 | C3 – Dwellinghouses | Sole or main residence used by people forming a single household. |  | 2025-10-27 |  |  |
| c4 | C4 – Houses in multiple occupation | Defined in the Housing Act 2004 (with the exclusion of converted block of flats). |  | 2025-10-27 |  |  |
| e-a | E(a) – Retail (other than hot food) | Shops and other retail services. |  | 2025-10-27 |  |  |
| e-b | E(b) – Food and Drink | Premises mostly for on-site consumption. |  | 2025-10-27 |  |  |
| e-c-i | E(c)(i) – Financial Services | Banks, building societies, and credit unions. |  | 2025-10-27 |  |  |
| e-c-ii | E(c)(ii) – Professional Services | Non-health/medical services (e.g., accountants, solicitors). |  | 2025-10-27 |  |  |
| e-c-iii | E(c)(iii) – Any Other Service | Non-retail services to the public. |  | 2025-10-27 |  |  |
| e-d | E(d) – Indoor Sports and Recreation | Excludes motorised or firearms activities. |  | 2025-10-27 |  |  |
| e-e | E(e) – Medical or Health Services | Clinics and health centres. |  | 2025-10-27 |  |  |
| e-f | E(f) – Creche, Day Nursery | Facilities for childcare. |  | 2025-10-27 |  |  |
| e-g-i | E(g)(i) – Office | For operational or administrative functions. |  | 2025-10-27 |  |  |
| e-g-ii | E(g)(ii) – Research and Development | Development of products or processes. |  | 2025-10-27 |  |  |
| e-g-iii | E(g)(iii) – Industrial Process | Processes that can operate within a residential area. |  | 2025-10-27 |  |  |
| f1-a | F1(a) – Education | Schools, colleges, and training centres. |  | 2025-10-27 |  |  |
| f1-b | F1(b) – Display of Works of Art | Galleries (excluding commercial galleries). |  | 2025-10-27 |  |  |
| f1-c | F1(c) – Museum | Non-commercial museums. |  | 2025-10-27 |  |  |
| f1-d | F1(d) – Public Library | Libraries open to the public. |  | 2025-10-27 |  |  |
| f1-e | F1(e) – Public Hall/Exhibition Hall | Community spaces for events. |  | 2025-10-27 |  |  |
| f1-f | F1(f) – Public Worship/Religious Instruction | Churches, mosques, synagogues. |  | 2025-10-27 |  |  |
| f1-g | F1(g) – Law Court | Court facilities. |  | 2025-10-27 |  |  |
| f2-a | F2(a) – Local Community Shop | Shop under 280 sqm with no similar facility nearby. |  | 2025-10-27 |  |  |
| f2-b | F2(b) – Community Hall | Halls for local community use. |  | 2025-10-27 |  |  |
| f2-c | F2(c) – Outdoor Sport/Recreation | Excludes motorised sports. |  | 2025-10-27 |  |  |
| f2-d | F2(d) – Indoor/Outdoor Swimming Pool | Includes skating rinks. |  | 2025-10-27 |  |  |
| sui | Sui generis | Uses that do not fall within any defined use class and are considered unique. For example, theatres, nightclubs, scrap yards and mineral extraction |  | 2025-10-27 |  |  |
| other | Other (Please Specify) | Free text required if selected. |  | 2025-10-27 |  |  |

### Use class for accommodation

| reference | name | description | notes | entry-date | start-date | end-date |
| --- | --- | --- | --- | --- | --- | --- |
| c1 | C1 – Hotels | Includes hotels, boarding houses, and guest houses. |  | 2025-10-27 |  |  |
| c2 | C2 – Residential Institutions | Care homes, hospitals, and boarding schools. |  | 2025-10-27 |  |  |
| c2a | C2A – Secure Residential Institutions | Prisons, young offender institutions. |  | 2025-10-27 |  |  |
| other | Other (Please Specify) | Free text required if selected. |  | 2025-10-27 |  |  |

### User role type

| reference | name | description |
| --- | --- | --- |
| agent | Agent | A professional agent working for the applicant |
| proxy | Proxy | An individual working on behalf of the applicant but not in a professional capacity |

## The fields above are a subset of those recommended by [MHCLG](https://github.com/digital-land/planning-application-data-specification)

## Client vs Applicant vs Contractor vs Designer roles

The Full Plans form requires identifying not only the applicant but also, if different:

the client (on whose behalf the application is made)

the principal/sole contractor

the principal/lead designer

This schema currently includes only an applicant and optionally an agent, but there is no explicit notion of client, contractor, or designer.

## Existing building use, height, and storeys; and proposed building use, height, and storeys

The Full Plans form requests:

current building use

current number of storeys

current height

proposed new use

proposed new number of storeys

proposed new height

This schema covers use class, site location, site area, and materials, but does not explicitly include building height, number of storeys, or existing vs. proposed building use.

## Detailed work description

The Full Plans form includes a section titled Proposed Work, which asks for:

a description of the proposed works

material changes or change of use

drainage and sewer details

whether building over a public sewer

compliance with local enactments

This schema has modules for Foul sewage disposal, Materials, and Site details, but does not include:

a general “description of proposed work”

building height

number of storeys

current vs proposed use

## Drainage, sewers, water supply, sewer proximity, private street/highway checks

The Full Plans process asks for:

drainage and water supply details

whether the development is near or over a public sewer

whether building fronts onto a private street or highway

This schema includes Foul sewage disposal, but does not capture:

water supply details

surface water drainage

sewer proximity checks

private street / highway relationships

## Scope of drawings, plans, and supporting technical documents

Full Plans applications require submission of:

site location (block) plan

existing and proposed floor plans

elevations

sections

structural calculations

specifications (e.g., insulation, glazing, ventilation)

drainage layouts

foundation and structural details

This schema includes:

a generic documents[] module

a materials module

But it does not specifically require:

structural calculations

insulation specifications

ventilation/glazing specifications

a block plan

detailed technical plans needed for building control

## Regulatory and safety declarations

The Full Plans form includes checks for:

whether work is subject to the Fire Safety Order

whether a public sewer is within 3 metres

whether building fronts a private street

electrical installation works (Part P compliance)

replacement of windows/doors

extension of statutory time limits

presence of trees within 30m

Party Wall Act considerations

This schema does not include dedicated fields for:

electrical works

fire safety

sewer proximity

private street checks

tree constraints

Party Wall matters

safety / compliance declarations

## Commencement date, expiry, conditions, consent for conditional approval

The Full Plans form asks for:

the date work is considered commenced

agreement to conditional approval

whether applicant consents to an extension of the statutory decision period

This schema includes only a submission-date, and lacks:

commencement date

expected start / completion date

consent to conditions

extension-of-time consent

## Client consent statement (where applicant is not the client)

The Full Plans form requires:

a Statement of Consent signed by the client, if someone else is submitting the application on their behalf.

This schema:

includes agent and applicant,

but does not include a separate client,

nor a signed consent statement from that client.
