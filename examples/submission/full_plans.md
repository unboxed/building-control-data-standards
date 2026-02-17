# Full Plans – Example Submission (Statutorily Complete)

This example represents a **Full Plans application** structured to meet the
minimum statutory requirements under:

- Building Act 1984
- Building Regulations 2010 (as amended)
- Building (Registered Building Control Approvers etc.) (England) Regulations 2024
- Post–Building Safety Act dutyholder regime

It follows the modular structure defined in the main schema and uses
**MUST / MAY** requirements in line with the repository standard.

---

# Application fields

Core building control application structure containing reference information,
application types, submission details, modules, documents, and fees.

## Application fields module

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | A unique reference for the data item | MUST | Valid UUID format
application-types | Application types[] | Type of building control application | MUST | Must include `full-plans`
building-control-authority | Building control authority | Authority responsible for the application | MUST | Valid organisation reference
submission-date | Submission date | Date application submitted (YYYY-MM-DD) | MUST |
modules | Modules[] | List of modules included in submission | MUST |
documents | Documents[]{} | List of submitted documents | MUST |
fee | Fee{} | Fee payable for application | MUST |

---

# Proposed work

## Proposed work module

field | name | description | required | notes
-- | -- | -- | -- | --
proposed-work-description | Proposed work description | Description of the building work | MUST |
is-material-change-of-use | Material change of use | Indicates if change of use applies | MUST |

---

# Building characteristics

## Building characteristics module

field | name | description | required | notes
-- | -- | -- | -- | --
existing-use-class | Existing use class | Current lawful use | MUST |
proposed-use-class | Proposed use class | Intended use following works | MUST |
existing-number-of-storeys | Existing number of storeys | Total storeys before works | MUST |
proposed-number-of-storeys | Proposed number of storeys | Total storeys after works | MUST |
existing-height-m | Existing height (m) | Height in metres | MUST |
proposed-height-m | Proposed height (m) | Height in metres | MUST |
is-higher-risk-building | Higher-risk building | HRB classification | MUST |
is-fso-building | Fire Safety Order building | FSO applicability | MUST |

---

# Dutyholders

## Dutyholder module

field | name | description | required | notes
-- | -- | -- | -- | --
client | Client{} | Client under Building Regulations | MUST |
principal-designer | Principal designer{} | Appointed principal designer | MUST |
principal-contractor | Principal contractor{} | Appointed principal contractor | MUST |

---

# Regulatory declarations

## Regulatory declarations module

field | name | description | required | notes
-- | -- | -- | -- | --
includes-electrical-work | Includes electrical works | Whether Part P applies | MUST |
within-3m-of-public-sewer | Within 3m of public sewer | Sewer proximity | MUST |
building-over-public-sewer | Building over sewer | Over or near sewer | MUST |
fronts-onto-private-street | Fronts onto private street | Highway relationship | MUST |
party-wall-act-applicable | Party Wall Act applies | Party Wall etc. Act 1996 | MUST |

---

# Drainage and water

## Drainage module

field | name | description | required | notes
-- | -- | -- | -- | --
produce-foul-sewage | Produces foul sewage | Whether foul discharge exists | MUST |
connect-to-drainage-system | Connects to mains drainage | Connection to system | MUST |
foul-sewage-disposal-types | Foul disposal types[] | Disposal method | MUST |
surface-water-disposal-type | Surface water disposal | Surface water strategy | MUST |
water-supply-type | Water supply type | Source of water supply | MUST |
drainage-system-details | Drainage drawings reference | Reference to plans | MUST |

---

# Commencement

## Commencement module

field | name | description | required | notes
-- | -- | -- | -- | --
proposed-commencement-date | Proposed commencement date | Intended start date | MUST |
consent-to-extend-decision-period | Consent to extend time | Extension of statutory period | MUST |

---

# Documents

## Document component

field | name | description | required | notes
-- | -- | -- | -- | --
reference | Reference | Unique document reference | MUST |
name | Name | Document title | MUST |
document-types | Document types[] | Type classification | MUST |
file | File{} | File object | MUST |

---

## Required document types for Full Plans

The following `document-types` MUST be included for a valid Full Plans submission:

- site-location-plan
- existing-floor-plans
- proposed-floor-plans
- elevations
- sections
- structural-calculations
- drainage-layout

---

# Declaration

## Declaration module

field | name | description | required | notes
-- | -- | -- | -- | --
name | Name | Name of declarant | MUST |
declaration-confirmed | Declaration confirmed | Confirmation of accuracy | MUST |
declaration-date | Declaration date | Date of declaration | MUST |

---

# Example JSON payload

```json
{
  "reference": "FP-UUID-000001",
  "application-types": ["full-plans"],
  "building-control-authority": "E09000000",
  "submission-date": "2026-01-15",
  "modules": [
    "proposed-work",
    "building-characteristics",
    "dutyholders",
    "regulatory-declarations",
    "drainage",
    "commencement"
  ],
  "proposed-work": {
    "proposed-work-description": "Two-storey rear extension with internal alterations.",
    "is-material-change-of-use": false
  },
  "building-characteristics": {
    "existing-use-class": "c3",
    "proposed-use-class": "c3",
    "existing-number-of-storeys": 2,
    "proposed-number-of-storeys": 2,
    "existing-height-m": 7.5,
    "proposed-height-m": 7.5,
    "is-higher-risk-building": false,
    "is-fso-building": false
  },
  "dutyholders": {
    "client": {"name": "Client Placeholder"},
    "principal-designer": {"name": "PD Placeholder"},
    "principal-contractor": {"name": "PC Placeholder"}
  },
  "regulatory-declarations": {
    "includes-electrical-work": true,
    "within-3m-of-public-sewer": false,
    "building-over-public-sewer": false,
    "fronts-onto-private-street": false,
    "party-wall-act-applicable": true
  },
  "drainage": {
    "produce-foul-sewage": true,
    "connect-to-drainage-system": true,
    "foul-sewage-disposal-types": ["mains-sewer"],
    "surface-water-disposal-type": "soakaway",
    "water-supply-type": "mains",
    "drainage-system-details": "See DR-001"
  },
  "commencement": {
    "proposed-commencement-date": "2026-03-01",
    "consent-to-extend-decision-period": true
  },
  "declaration": {
    "name": "Applicant Placeholder",
    "declaration-confirmed": true,
    "declaration-date": "2026-01-15"
  }
}
```