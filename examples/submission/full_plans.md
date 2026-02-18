# Full Plans – Example Submission (Extended + Statutorily Complete)

This example represents a **Full Plans application** structured to meet the
minimum statutory requirements under:

- Building Act 1984
- Building Regulations 2010 (as amended)
- Building (Registered Building Control Approvers etc.) (England) Regulations 2024
- Post–Building Safety Act dutyholder regime

It follows the modular structure defined in this repository and retains all
previously defined MUST and MAY modules.

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

# Proposed work

## Proposed work module

field | name | description | required | notes
-- | -- | -- | -- | --
proposed-work-description | Proposed work description | Description of the building work | MUST |
is-material-change-of-use | Material change of use | Indicates whether a material change of use applies | MUST |

Validation rules:

- proposed-work-description must not be empty

---

# Building characteristics

## Building characteristics module

field | name | description | required | notes
-- | -- | -- | -- | --
existing-use-class | Existing use class | Current lawful use of the building | MUST |
proposed-use-class | Proposed use class | Intended use following works | MUST |
existing-number-of-storeys | Existing number of storeys | Total storeys before works | MUST |
proposed-number-of-storeys | Proposed number of storeys | Total storeys after works | MUST |
existing-height-m | Existing height (m) | Height in metres | MUST |
proposed-height-m | Proposed height (m) | Height in metres | MUST |
is-higher-risk-building | Higher-risk building | Higher-risk building classification | MUST |
is-fso-building | Fire Safety Order building | Indicates whether the building is subject to FSO | MUST |

Validation rules:

- storeys must be positive integers
- height must be positive numbers
- use class must reference a valid codelist value

---

# Dutyholders

## Dutyholder module

field | name | description | required | notes
-- | -- | -- | -- | --
client | Client{} | Client under Building Regulations | MUST |
principal-designer | Principal designer{} | Appointed principal designer | MUST |
principal-contractor | Principal contractor{} | Appointed principal contractor | MUST |

Validation rules:

- client must be provided
- principal-designer must be provided
- principal-contractor must be provided

---

# Regulatory declarations

## Regulatory declarations module

field | name | description | required | notes
-- | -- | -- | -- | --
includes-electrical-work | Includes electrical works | Whether Part P applies | MUST |
within-3m-of-public-sewer | Within 3m of public sewer | Sewer proximity check | MUST |
building-over-public-sewer | Building over public sewer | Indicates if building over sewer | MUST |
fronts-onto-private-street | Fronts onto private street | Indicates highway relationship | MUST |
party-wall-act-applicable | Party Wall Act applies | Indicates if Party Wall etc. Act 1996 applies | MUST |

---

# Drainage and water

## Drainage module

field | name | description | required | notes
-- | -- | -- | -- | --
produce-foul-sewage | Produce foul sewage | Whether foul sewage will be produced | MUST |
connect-to-drainage-system | Connect to drainage system | Indicates connection to existing drainage | MUST |
foul-sewage-disposal-types | Foul sewage disposal types[] | Disposal methods | MAY |
surface-water-disposal-type | Surface water disposal type | Surface water drainage strategy | MUST |
water-supply-type | Water supply type | Source of water supply | MUST |
drainage-system-details | Drainage system details | Details of drawings/plans showing drainage | MAY |

Validation rules:

- if connect-to-drainage-system == true then drainage-system-details is required

---

# Materials

## Materials module

field | name | description | required | notes
-- | -- | -- | -- | --
building-elements | Building elements[]{} | Details of materials for building elements | MUST |
providing-additional-material-information | Providing additional material information | Indicates if additional material details provided in plans | MUST |
supporting-documents | Supporting documents[]{} | Supporting documents relating to materials | MAY |

Validation rules:

- Each building-element must have a unique building-element-type
- At least one of existing-materials, proposed-materials, materials-not-applicable or materials-not-known must be provided

---

# Hazardous substances

## Hazardous substances module

field | name | description | required | notes
-- | -- | -- | -- | --
involves-hazardous-substances | Involves hazardous substances | Indicates if hazardous substances are involved | MUST |
substance-types | Substance types[]{} | List of hazardous substances and quantities | MAY |
hazardous-sub-consent-req | Hazardous substance consent required | Indicates if consent required | MUST |
hazardous-sub-consent-details | Hazardous substance consent details | Details of consent requirements | MAY |

Validation rules:

- if involves-hazardous-substances == 'yes' then substance-types is required
- if hazardous-sub-consent-req == true then hazardous-sub-consent-details is required

---

# Site area

## Site area module

field | name | description | required | notes
-- | -- | -- | -- | --
site-area-in-hectares | Site area in hectares | Size of site in hectares | MUST |
site-area-provided-by | Site area provided by | Source of measurement | MAY |

Validation rules:

- site-area-in-hectares must be a positive number

---

# Site details

## Site details module

field | name | description | required | notes
-- | -- | -- | -- | --
site-locations | Site locations[]{} | Details of sites where works are proposed | MUST |

Validation rules:

- Exactly one site-location must be provided for full plans applications

---

# Site Visit Details

## Site Visit Details module

field | name | description | required | notes
-- | -- | -- | -- | --
can-be-seen-from | Site seen from public area | Can site be seen from public land | MUST |
contact-type | Site visit contact type | Who authority should contact | MUST |
contact-reference | Contact reference | Reference of applicant or agent | MAY |
other-contact | Other site visit contact{} | Specifically named contact | MAY |

Validation rules:

- When contact-type is other, full contact details must be provided

---

# Checklist

## Checklist module

field | name | description | required | notes
-- | -- | -- | -- | --
national-req-types | National requirement types[] | List of required document types | MUST |

Validation rules:

- Values must be valid for full-plans application type

---

# Conflict of interest

## Conflict of interest module

field | name | description | required | notes
-- | -- | -- | -- | --
conflict-to-declare | Conflict to declare | Indicates whether conflict exists | MUST |
conflict-person-name | Conflict person name | Name of individual with conflict | MAY |
conflict-details | Conflict details | Details of conflict | MAY |

Validation rules:

- conflict-person-name must match applicant or agent if provided

---

# Commencement

## Commencement module

field | name | description | required | notes
-- | -- | -- | -- | --
proposed-commencement-date | Proposed commencement date | Intended start date | MUST |
consent-to-extend-decision-period | Consent to extend time | Agreement to extend statutory period | MUST |

---

# Declaration

## Declaration module

field | name | description | required | notes
-- | -- | -- | -- | --
name | Name | Name of declarant | MUST |
declaration-confirmed | Declaration confirmed | Confirmation of accuracy | MUST |
declaration-date | Declaration date | Date declaration made | MUST |

Validation rules:

- declaration-date must not be in the future
- name must match applicant or agent

# Example (anonymised JSON)

```
{
  "reference": "FP-UUID-000001",
  "application-types": ["full-plans"],
  "application-sub-type": "domestic-extension",
  "planning-application": "PLAN-REF-000001",
  "building-control-authority": "E09000000",
  "submission-date": "2026-01-15",
  "modules": [
    "proposed-work",
    "building-characteristics",
    "dutyholders",
    "regulatory-declarations",
    "drainage",
    "materials",
    "hazardous-substances",
    "site-area",
    "site-details",
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
  },

  "dutyholders": {
    "client": {
      "name": "Client Placeholder",
      "address-text": "1 Example Street, London, AA1 1AA"
    },
    "principal-designer": {
      "name": "Principal Designer Placeholder",
      "company": "Design Practice Ltd"
    },
    "principal-contractor": {
      "name": "Principal Contractor Placeholder",
      "company": "Construction Company Ltd"
    }
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
    "drainage-system-details": "Refer to drawing DR-001."
  },

  "materials": {
    "building-elements": [
      {
        "building-element-type": "walls",
        "existing-materials": "Brick",
        "proposed-materials": "Facing brick to match existing"
      },
      {
        "building-element-type": "roof",
        "existing-materials": "Concrete tiles",
        "proposed-materials": "Concrete tiles to match"
      }
    ],
    "providing-additional-material-information": true,
    "supporting-documents": [
      {
        "reference": "DOC-005",
        "name": "Materials Specification"
      }
    ]
  },

  "hazardous-substances": {
    "involves-hazardous-substances": "no",
    "hazardous-sub-consent-req": false
  },

  "site-area": {
    "site-area-in-hectares": 0.015,
    "site-area-provided-by": "measured"
  },

  "site-details": {
    "site-locations": [
      {
        "address-text": "1 Example Street, London",
        "postcode": "AA1 1AA",
        "uprns": ["100000000000"]
      }
    ]
  },

  "site-visit-details": {
    "can-be-seen-from": true,
    "contact-type": "applicant",
    "contact-reference": "APPL-001"
  },

  "checklist": {
    "national-req-types": [
      "site-location-plan",
      "existing-floor-plans",
      "proposed-floor-plans",
      "elevations",
      "sections",
      "structural-calculations",
      "drainage-layout"
    ]
  },

  "conflict-of-interest": {
    "conflict-to-declare": false
  },

  "commencement": {
    "proposed-commencement-date": "2026-03-01",
    "consent-to-extend-decision-period": true
  },

  "declaration": {
    "name": "Applicant Placeholder",
    "declaration-confirmed": true,
    "declaration-date": "2026-01-15"
  },

  "documents": [
    {
      "reference": "DOC-001",
      "name": "Site Location Plan",
      "document-types": ["site-location-plan"],
      "file": {
        "filename": "site_location_plan.pdf",
        "url": "https://example-storage.com/docs/site_location_plan.pdf"
      }
    },
    {
      "reference": "DOC-002",
      "name": "Existing and Proposed Floor Plans",
      "document-types": ["existing-floor-plans", "proposed-floor-plans"],
      "file": {
        "filename": "floor_plans.pdf",
        "url": "https://example-storage.com/docs/floor_plans.pdf"
      }
    },
    {
      "reference": "DOC-003",
      "name": "Elevations and Sections",
      "document-types": ["elevations", "sections"],
      "file": {
        "filename": "elevations_sections.pdf",
        "url": "https://example-storage.com/docs/elevations_sections.pdf"
      }
    },
    {
      "reference": "DOC-004",
      "name": "Structural Calculations",
      "document-types": ["structural-calculations"],
      "file": {
        "filename": "structural_calculations.pdf",
        "url": "https://example-storage.com/docs/structural_calculations.pdf"
      }
    },
    {
      "reference": "DOC-005",
      "name": "Materials Specification",
      "document-types": ["materials-specification"],
      "file": {
        "filename": "materials_specification.pdf",
        "url": "https://example-storage.com/docs/materials_specification.pdf"
      }
    },
    {
      "reference": "DOC-006",
      "name": "Drainage Layout",
      "document-types": ["drainage-layout"],
      "file": {
        "filename": "drainage_layout.pdf",
        "url": "https://example-storage.com/docs/drainage_layout.pdf"
      }
    }
  ],

  "fee": {
    "amount": 950.00,
    "amount-paid": 950.00,
    "transactions": []
  }
}
```