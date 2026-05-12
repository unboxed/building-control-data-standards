# Certificate of Completion (Exploratory Schema)

This document proposes an illustrative structured data model for a  
**Completion Certificate under Regulation 17 of the Building Regulations 2010**.

It models the procedural, evidential, and compliance-related information
commonly present within Completion Certificates issued by local authorities.

This is not a statutory template. It is an exploratory discussion artefact.

---

# Design Intent

A Completion Certificate:

- records that building work has been completed or partly occupied
- confirms the local authority has taken reasonable steps to assess compliance
- provides evidential confirmation regarding applicable Building Regulations
- identifies the completed work and relevant site
- records limitations of the certificate
- concludes the local authority building control process

Unlike a Final Certificate:

- It is issued by a local authority.
- It does not rely on an Initial Notice process.
- It does not contain Registered Building Control Approver declarations.

This schema treats the Completion Certificate as:

- a structured evidential certificate
- recording compliance assessment outcomes
- with explicit validation rules
- suitable for API or machine-readable exchange

---

# Schema Overview

---

## 1. Certificate Metadata Module

| field | required | notes |
|-------|----------|-------|
| certificate-reference | MUST | Unique certificate identifier |
| certificate-date | MUST | ISO 8601 date format |
| legislative-basis | MUST | e.g. `building-regulations-2010-regulation-17` |
| issuing-authority | MUST | Local authority issuing certificate |
| application-reference | MUST | Building control application reference |

### Validation Rules

- certificate-reference must be unique
- certificate-date must not be in the future
- issuing-authority must be a valid local authority organisation

---

## 2. Site Location Module

The Completion Certificate MUST identify the property to which the completed work relates.

This schema aligns with the canonical `site-details.site-location` structure used across submission types.

| field | required | notes |
|-------|----------|-------|
| site-details | MUST | Container for site information |
| site-location | MUST | Identifies regulated property |
| address | MUST | Structured postal address |
| number-name | MUST | Building number or name |
| street | MUST | Street name |
| town-city | MUST | Post town or city |
| postcode | MUST | Valid UK postcode |
| uprns | MAY | One or more 12-digit UPRNs |

### Validation Rules

- postcode must conform to UK postcode format
- if provided, each UPRN must be a 12-digit numeric string

---

## 3. Work Completion Module

| field | required | notes |
|-------|----------|-------|
| work-description | MUST | Description of completed work |
| completion-date | MUST | Date work completed |
| partial-occupation | MAY | Indicates building partly occupied |
| completion-notice-received | MUST | Confirmation notice received under regulation 16 |

### Validation Rules

- completion-date must not be in the future
- work-description must not be empty
- partial-occupation must be boolean if provided
- completion-notice-received must be boolean

---

## 4. Compliance Assessment Module

This module records the local authority's assessment of compliance.

| field | required | notes |
|-------|----------|-------|
| reasonable-steps-taken | MUST | Local authority took reasonable steps to assess compliance |
| substantive-requirements-satisfied | MUST | Relevant requirements considered satisfied |
| inspections-carried-out | MAY | Indicates inspections undertaken |
| evidence-status | MUST | Certificate is evidential but not conclusive |

### Validation Rules

- mandatory assessment fields must be boolean
- evidence-status must contain the standard evidential wording

---

## 5. Relevant Provisions Module

This module records applicable regulatory provisions considered within the certificate.

| field | required | notes |
|-------|----------|-------|
| regulation-25a | MAY | High-efficiency alternative systems |
| regulation-26 | MAY | Target CO2 emission rates |
| regulation-26a | MAY | Fabric energy efficiency |
| regulation-26c | MAY | Primary energy rates |
| regulation-36 | MAY | Water efficiency |
| regulation-38 | MAY | Fire safety information |
| schedule-1 | MAY | Schedule 1 requirements |
| energy-performance-certificate | MAY | EPC requirements |

### Validation Rules

- provision fields must be boolean
- only applicable provisions should be included

---

## 6. Certificate Limitations Module

This module records standard limitations applying to the certificate.

| field | required | notes |
|-------|----------|-------|
| existing-building-excluded | MUST | Existing unaffected building excluded |
| non-regulated-work-excluded | MUST | Non-regulated work excluded |
| fittings-and-repairs-excluded | MUST | Certain fittings and repair works excluded |

### Validation Rules

- all limitation fields must be boolean

---

## 7. Certificate Authorisation Module

| field | required | notes |
|-------|----------|-------|
| authorised-by | MUST | Name of authorised officer |
| authorised-role | MUST | Role or title |
| signature-date | MUST | Date certificate signed |
| authority-name | MUST | Issuing authority name |

### Validation Rules

- signature-date must not precede completion-date
- authorised-by must not be empty

---

# Example JSON

```json
{
  "certificate-reference": "cc-5f8c47e2-9942-4e3a-8c5f-a0f4a8a00112",
  "certificate-date": "2026-04-08",
  "legislative-basis": "building-regulations-2010-regulation-17",
  "issuing-authority": {
    "name": "Example District Council",
    "ons-code": "E07000001"
  },
  "application-reference": "BC-2025-001245",
  "site-details": {
    "site-location": {
      "address": {
        "number-name": "Rose Cottage",
        "street": "High Street",
        "town-city": "Ottery St Mary",
        "postcode": "EX11 1AA"
      },
      "uprns": [
        "100012345678"
      ]
    }
  },
  "work-completion": {
    "work-description": "Removal of internal loadbearing wall between kitchen and dining room",
    "completion-date": "2026-04-07",
    "partial-occupation": false,
    "completion-notice-received": true
  },
  "compliance-assessment": {
    "reasonable-steps-taken": true,
    "substantive-requirements-satisfied": true,
    "inspections-carried-out": true,
    "evidence-status": "evidence-but-not-conclusive"
  },
  "relevant-provisions": {
    "regulation-25a": false,
    "regulation-26": false,
    "regulation-26a": false,
    "regulation-26c": false,
    "regulation-36": false,
    "regulation-38": true,
    "schedule-1": true,
    "energy-performance-certificate": false
  },
  "certificate-limitations": {
    "existing-building-excluded": true,
    "non-regulated-work-excluded": true,
    "fittings-and-repairs-excluded": true
  },
  "certificate-authorisation": {
    "authorised-by": "Jane Example",
    "authorised-role": "Assistant Director – Planning Strategy & Development Management",
    "signature-date": "2026-04-08",
    "authority-name": "Example District Council"
  }
}
```
