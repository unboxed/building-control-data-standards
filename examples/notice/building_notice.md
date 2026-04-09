# Building Notice (Exploratory Schema)

This example represents a **Building Notice application** structured to meet the
minimum statutory requirements under:

- Building Act 1984
- Building Regulations 2010 (as amended)
- Building (Registered Building Control Approvers etc.) (England) Regulations 2024

It reflects the simplified submission model permitted for Building Notices,
where detailed plans are not required at submission stage.

This is not a statutory template. It is an exploratory discussion artefact.

---

# Application fields

Core building control application structure containing reference information,
application types, submission details, modules, documents, and fees.

## Application fields module

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A unique reference for the data item | MUST |
application-types | Application types[] | A list of building control application types | MUST | Must include `building-notice`
application-sub-type | Application sub type | Further classification of the application type | MAY |
building-control-authority | Building control authority | Authority responsible for the application | MUST |
submission-date | Submission date | Date the application is submitted (YYYY-MM-DD) | MUST |
modules | Modules[] | List of modules included in submission | MUST |
documents | Documents[]{} | List of submitted documents | MAY |
fee | Fee{} | Fee payable for the application | MUST |

Validation rules:

- reference must be a valid UUID format
- application-types must include `building-notice`
- building-control-authority must be a valid organisation reference
- modules must reference existing module definitions
- document references must be unique within the application (if provided)

---

# Applicant

## Applicant module

Building Notice submissions MUST identify the applicant responsible for the work.

field | name | description | required
-- | -- | -- | --
applicant-type | Applicant type | Individual or business | MUST
applicant-name | Applicant name | Full name or organisation | MUST
contact-details | Contact details | Email and/or telephone | MUST
address | Address | Correspondence address | MUST
agent-used | Agent used | Indicates if an agent is acting | MUST

Validation rules:

- applicant-name must not be empty
- at least one contact method (email or phone) must be provided

---

# Site Location

## Site Location Module

Building Notices MUST identify the property to which the works relate.

This aligns with the canonical `site-details.site-location` component used across submission types.

field | name | description | required
-- | -- | -- | --
site-details | Site details | Container for site information | MUST
site-location | Site location | Identifies regulated property | MUST
address | Address | Structured postal address | MUST
number-name | Number or name | Building number or name | MUST
street | Street | Street name | MUST
town-city | Town or city | Post town or city | MUST
postcode | Postcode | Valid UK postcode | MUST
uprns | UPRNs[] | One or more 12-digit UPRNs | MUST

Validation rules:

- Exactly one `site-location` must be provided
- `uprns` must contain at least one entry
- Each UPRN must be a 12-digit numeric string
- Postcode must conform to UK postcode format

---

# Proposed work

## Proposed work module

field | name | description | required
-- | -- | -- | --
proposed-work-description | Proposed work description | Description of the building work | MUST
is-new-dwelling | New dwelling | Indicates whether work includes a new dwelling | MUST
proposed-start-date | Proposed start date | Intended date of commencement | MUST

Validation rules:

- proposed-work-description must not be empty
- proposed-start-date must be on or after submission-date

---

# Building summary

## Building summary module

A simplified representation of building characteristics appropriate for Building Notice submissions.

field | name | description | required
-- | -- | -- | --
existing-use | Existing use | Current use of the building or land | MUST
proposed-use | Proposed use | Intended use following works | MUST
number-of-storeys | Number of storeys | Total number of storeys (including basements) | MUST

Validation rules:

- number-of-storeys must be a positive integer
- use values should align with a recognised codelist where possible

---

# Dutyholders

## Dutyholder module

Building Notices MAY include dutyholder information at submission stage.

field | name | description | required
-- | -- | -- | --
client | Client{} | Client under Building Regulations | MUST
principal-designer | Principal designer{} | Appointed principal designer | MAY
principal-contractor | Principal contractor{} | Appointed principal contractor | MAY

Validation rules:

- client must be provided
- principal-designer and principal-contractor MAY be null or incomplete

---

# Services

## Services module

field | name | description | required
-- | -- | -- | --
water-supply-type | Water supply type | Source of water supply (e.g. mains) | MUST
drainage-type | Drainage type | Method of foul drainage | MUST

Validation rules:

- values should align with recognised codelists where possible

---

# Charges

## Charges module

field | name | description | required
-- | -- | -- | --
additional-floor-area-m2 | Additional floor area | Additional floor area in square metres | MUST
estimated-cost | Estimated cost | Estimated cost of works (if no floor area) | MUST
charge-unknown | Charge unknown | Indicates authority should calculate fee | MAY

Validation rules:

- at least one of `additional-floor-area-m2` or `estimated-cost` must be provided
- values must be zero or positive numbers

---

# Declaration

## Declaration module

field | name | description | required
-- | -- | -- | --
confirm-statement | Confirmation statement | Applicant confirms accuracy of submission | MUST

---

# Example (anonymised JSON)

```json
{
  "reference": "BN-UUID-000001",
  "application-types": ["building-notice"],
  "application-sub-type": "internal-alterations",
  "building-control-authority": "E09000000",
  "submission-date": "2026-04-01",

  "modules": [
    "applicant",
    "site-details",
    "proposed-work",
    "building-summary",
    "dutyholders",
    "services",
    "charges",
    "declaration"
  ],

  "applicant": {
    "applicant-type": "individual",
    "applicant-name": "Jane Example",
    "contact-details": {
      "email": "jane@example.com",
      "phone": "07123456789"
    },
    "address": {
      "number-name": "1",
      "street": "Example Street",
      "town-city": "Exampletown",
      "postcode": "AA1 1AA"
    },
    "agent-used": false
  },

  "site-details": {
    "site-location": {
      "address": {
        "number-name": "1",
        "street": "Example Street",
        "town-city": "Exampletown",
        "postcode": "AA1 1AA"
      },
      "uprns": ["100000000000"]
    }
  },

  "proposed-work": {
    "proposed-work-description": "Internal alterations including widening of doorway in a stud wall.",
    "is-new-dwelling": false,
    "proposed-start-date": "2026-04-14"
  },

  "building-summary": {
    "existing-use": "residential",
    "proposed-use": "residential",
    "number-of-storeys": 3
  },

  "dutyholders": {
    "client": {
      "name": "Jane Example"
    },
    "principal-designer": null,
    "principal-contractor": null
  },

  "services": {
    "water-supply-type": "mains",
    "drainage-type": "mains"
  },

  "charges": {
    "additional-floor-area-m2": 0,
    "estimated-cost": 800,
    "charge-unknown": true
  },

  "declaration": {
    "confirm-statement": true
  }
}
