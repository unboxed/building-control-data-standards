# Final Certificate (Exploratory Schema)

This document proposes an illustrative structured data model for a  
**Final Certificate under Section 51 of the Building Act 1984**.

It models the legal confirmations, declarations, completion statements,
and procedural requirements commonly present within Final Certificates
issued by Registered Building Control Approvers.

This is not a statutory template. It is an exploratory discussion artefact.

---

# Design Intent

A Final Certificate:

- confirms completion of work subject to an Initial Notice
- confirms the Registered Building Control Approver has performed its statutory functions
- provides evidence of compliance with applicable Building Regulations
- contains statutory declarations required under the Building (Registered Building Control Approvers etc.) (England) Regulations 2024
- records procedural consultation and declaration requirements
- closes the Initial Notice process

Unlike a Completion Certificate issued by a local authority:

- It relates specifically to work subject to an Initial Notice.
- It is issued by a Registered Building Control Approver.
- It includes statutory declarations regarding conflicts of interest and dutyholder statements.

This schema treats the Final Certificate as:

- a structured regulatory completion notice
- containing declarative legal statements
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
| legislative-basis | MUST | e.g. `section-51-building-act-1984` |
| regulations-version | MUST | e.g. `rbc-approvers-england-2024` |
| initial-notice-reference | MUST | Reference for associated Initial Notice |
| building-control-approver | MUST | Registered Building Control Approver issuing certificate |

### Validation Rules

- certificate-reference must be unique
- certificate-date must not be in the future
- initial-notice-reference must reference an existing Initial Notice
- building-control-approver must be a valid registered organisation

---

## 2. Site Location Module

The Final Certificate MUST identify the property to which the completed work relates.

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

## 3. Work Description Module

| field | required | notes |
|-------|----------|-------|
| work-description | MUST | Description of completed work |
| building-use | MAY | Use classification of building |
| completion-date | MUST | Date work completed |
| higher-risk-building | MUST | Indicates whether work relates to a higher-risk building |

### Validation Rules

- completion-date must not be in the future
- work-description must not be empty
- higher-risk-building must be boolean

---

## 4. Regulatory Confirmation Module

This module records statutory confirmations required of the Registered Building Control Approver.

| field | required | notes |
|-------|----------|-------|
| approver-registered | MUST | Confirms organisation is a registered building control approver |
| work-within-registration-scope | MUST | Confirms work falls within registration scope |
| work-complete | MUST | Confirms work completion |
| statutory-functions-performed | MUST | Confirms statutory functions have been performed |
| fire-safety-information-provided | MUST | Confirmation relating to Regulation 38 |
| final-certificate-issued | MUST | Confirms final certificate issued for all work |
| no-professional-interest | MUST | Declaration of no professional or financial interest |

### Validation Rules

- all confirmation fields must be boolean
- no-professional-interest must be `true` before certificate issuance

---

## 5. Optional Requirements Module

This module records whether optional requirements under the Building Regulations 2010 apply.

| field | required | notes |
|-------|----------|-------|
| optional-water-efficiency | MAY | Regulation 36(2)(b) applies |
| category-2-accessible-dwelling | MAY | Requirement M4(2) applies |
| category-3-wheelchair-dwelling | MAY | Requirement M4(3) applies |

### Validation Rules

- optional requirement fields must be boolean
- optional requirements should only be included where applicable to the work

---

## 6. Dutyholder Declaration Module

This module records receipt of required client and dutyholder statements.

| field | required | notes |
|-------|----------|-------|
| client-statement-received | MUST | Statement received under regulation 18(d) |
| contractor-statements-received | MUST | Statements received from principal contractors |
| designer-statements-received | MUST | Statements received from principal designers |
| fire-authority-consulted | MUST | Consultation under regulation 9 completed |
| registered-building-inspector-advice | MAY | Details of consulted registered building inspector |

### Validation Rules

- mandatory declaration fields must be boolean
- registered-building-inspector-advice should identify a named inspector if provided

---

## 7. Certificate Authorisation Module

| field | required | notes |
|-------|----------|-------|
| authorised-by | MUST | Name of authorised signatory |
| authorised-role | MUST | Role or title of signatory |
| signature-date | MUST | Date certificate signed |
| organisation-name | MUST | Issuing organisation |

### Validation Rules

- signature-date must not precede completion-date
- authorised-by must not be empty

---

# Example JSON

```json
{
  "certificate-reference": "fc-8a3f8f1d-6f0e-4f85-a3c0-1d5e6e5c1001",
  "certificate-date": "2026-05-01",
  "legislative-basis": "section-51-building-act-1984",
  "regulations-version": "rbc-approvers-england-2024",
  "initial-notice-reference": "IN-2026-001245",
  "building-control-approver": {
    "name": "Example Building Control Ltd",
    "registration-number": "RBCA-000123"
  },
  "site-details": {
    "site-location": {
      "address": {
        "number-name": "12",
        "street": "Example Street",
        "town-city": "Exeter",
        "postcode": "EX1 1AA"
      },
      "uprns": [
        "100012345678"
      ]
    }
  },
  "work-description": "Single storey rear extension",
  "building-use": "Residential dwelling",
  "completion-date": "2026-04-28",
  "higher-risk-building": false,
  "regulatory-confirmations": {
    "approver-registered": true,
    "work-within-registration-scope": true,
    "work-complete": true,
    "statutory-functions-performed": true,
    "fire-safety-information-provided": true,
    "final-certificate-issued": true,
    "no-professional-interest": true
  },
  "optional-requirements": {
    "optional-water-efficiency": false,
    "category-2-accessible-dwelling": false,
    "category-3-wheelchair-dwelling": false
  },
  "dutyholder-declarations": {
    "client-statement-received": true,
    "contractor-statements-received": true,
    "designer-statements-received": true,
    "fire-authority-consulted": true,
    "registered-building-inspector-advice": {
      "name": "Jane Smith",
      "registration-class": "Class 3"
    }
  },
  "certificate-authorisation": {
    "authorised-by": "Alex Brown",
    "authorised-role": "Compliance Director",
    "signature-date": "2026-05-01",
    "organisation-name": "Example Building Control Ltd"
  }
}
```
