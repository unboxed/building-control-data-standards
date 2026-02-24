# Full Plans – Example Submission (Extended + Statutorily Complete)

This example represents a **Full Plans application** structured to meet the
minimum statutory requirements under:

- Building Act 1984
- Building Regulations 2010 (as amended)
- Building (Registered Building Control Approvers etc.) (England) Regulations 2024
- Post–Building Safety Act dutyholder regime

It follows the modular structure defined in this repository and retains all
previously defined MUST and MAY modules.

This is not a statutory template. It is an exploratory discussion artefact.

---

# Application fields

Core building control application structure containing reference information,
application types, submission details, modules, documents, and fees.

## Application fields module

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A unique reference for the data item | MUST |
application-types | Application types[] | A list of building control application types | MUST | Must include `full-plans`
application-sub-type | Application sub type | Further classification of the application type | MAY |
planning-application | Planning application | Reference of related planning application | MAY |
building-control-authority | Building control authority | Authority responsible for the application | MUST |
submission-date | Submission date | Date the application is submitted (YYYY-MM-DD) | MUST |
modules | Modules[] | List of modules included in submission | MUST |
documents | Documents[]{} | List of submitted documents | MUST |
fee | Fee{} | Fee payable for the application | MUST |

Validation rules:

- reference must be a valid UUID format
- application-types must include `full-plans`
- building-control-authority must be a valid organisation reference
- modules must reference existing module definitions
- document references must be unique within the application

---

# Site Location

## Site Location Module

Full Plans applications MUST identify the property to which the works relate.

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

- Exactly one `site-location` must be provided for a Full Plans application.
- `uprns` must contain at least one entry.
- Each UPRN must be a 12-digit numeric string.
- Postcode must conform to UK postcode format.

---

# Proposed work

## Proposed work module

field | name | description | required
-- | -- | -- | --
proposed-work-description | Proposed work description | Description of the building work | MUST
is-material-change-of-use | Material change of use | Indicates whether a material change of use applies | MUST

Validation rules:

- proposed-work-description must not be empty

---

# Building characteristics

## Building characteristics module

field | name | description | required
-- | -- | -- | --
existing-use-class | Existing use class | Current lawful use of the building | MUST
proposed-use-class | Proposed use class | Intended use following works | MUST
existing-number-of-storeys | Existing number of storeys | Total storeys before works | MUST
proposed-number-of-storeys | Proposed number of storeys | Total storeys after works | MUST
existing-height-m | Existing height (m) | Height in metres | MUST
proposed-height-m | Proposed height (m) | Height in metres | MUST
is-higher-risk-building | Higher-risk building | Higher-risk building classification | MUST
is-fso-building | Fire Safety Order building | Indicates whether the building is subject to FSO | MUST

Validation rules:

- storeys must be positive integers
- height must be positive numbers
- use class must reference a valid codelist value

---

# Dutyholders

## Dutyholder module

field | name | description | required
-- | -- | -- | --
client | Client{} | Client under Building Regulations | MUST
principal-designer | Principal designer{} | Appointed principal designer | MUST
principal-contractor | Principal contractor{} | Appointed principal contractor | MUST

Validation rules:

- client must be provided
- principal-designer must be provided
- principal-contractor must be provided

---

# Regulatory declarations

## Regulatory declarations module

field | name | description | required
-- | -- | -- | --
includes-electrical-work | Includes electrical works | Whether Part P applies | MUST
within-3m-of-public-sewer | Within 3m of public sewer | Sewer proximity check | MUST
building-over-public-sewer | Building over public sewer | Indicates if building over sewer | MUST
fronts-onto-private-street | Fronts onto private street | Indicates highway relationship | MUST
party-wall-act-applicable | Party Wall Act applies | Indicates if Party Wall etc. Act 1996 applies | MUST

---

# Drainage and water

## Drainage module

field | name | description | required
-- | -- | -- | --
produce-foul-sewage | Produce foul sewage | Whether foul sewage will be produced | MUST
connect-to-drainage-system | Connect to drainage system | Indicates connection to existing drainage | MUST
foul-sewage-disposal-types | Foul sewage disposal types[] | Disposal methods | MAY
surface-water-disposal-type | Surface water disposal type | Surface water drainage strategy | MUST
water-supply-type | Water supply type | Source of water supply | MUST
drainage-system-details | Drainage system details | Details of drawings/plans showing drainage | MAY

Validation rules:

- if connect-to-drainage-system == true then drainage-system-details is required

---

# Example (anonymised JSON)

```json
{
  "reference": "FP-UUID-000001",
  "application-types": ["full-plans"],
  "application-sub-type": "domestic-extension",
  "planning-application": "PLAN-REF-000001",
  "building-control-authority": "E09000000",
  "submission-date": "2026-01-15",

  "site-details": {
    "site-location": {
      "address": {
        "number-name": "1",
        "street": "Example Street",
        "town-city": "London",
        "postcode": "AA1 1AA"
      },
      "uprns": ["100000000000"]
    }
  },

  "modules": [
    "proposed-work",
    "building-characteristics",
    "dutyholders",
    "regulatory-declarations",
    "drainage",
    "materials",
    "hazardous-substances",
    "site-area",
    "site-visit-details",
    "checklist",
    "conflict-of-interest",
    "commencement",
    "declaration"
  ],

  "proposed-work": {
    "proposed-work-description": "Two-storey rear extension with internal alterations and drainage modifications.",
    "is-material-change-of-use": false
  },

  "building-characteristics": {
    "existing-use-class": "c3",
    "proposed-use-class": "c3",
    "existing-number-of-storeys": 2,
    "proposed-number-of-storeys": 2,
    "existing-height-m": 7.5,
    "proposed-height-m": 7.8,
    "is-higher-risk-building": false,
    "is-fso-building": false
  }
}
