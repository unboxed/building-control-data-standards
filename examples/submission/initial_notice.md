# Initial Notice (Exploratory Schema)

This document proposes an illustrative structured data model for an  
**Initial Notice under Section 47 of the Building Act 1984**.

It models the common legal, procedural, and declaration components in a modular, machine-readable form.

This is not a statutory template. It is an exploratory discussion artefact.

---

# Design Intent

An Initial Notice:

- transfers building control responsibility to a Registered Building Control Approver (RBCA)
- is served on the relevant local authority
- must include specific statutory declarations
- must identify parties, work scope, and commencement details
- contains legally prescribed undertakings regarding consultation

Unlike Full Plans:

- It is not an approval application.
- It is a legal notification event.
- It creates a regulatory state change.

This schema treats the Initial Notice as:

- a structured legal notification
- containing declarative statements
- with explicit validation rules
- capable of batch or API submission

---

# Schema Overview

## 1. Notice Metadata Module

| field | required | notes |
|-------|----------|-------|
| notice-reference | MUST | Unique identifier for notice (UUID or scheme reference) |
| notice-date | MUST | ISO 8601 format |
| legislative-basis | MUST | e.g. `section-47-building-act-1984` |
| regulations-version | MUST | e.g. `rbc-approvers-england-2024` |
| local-authority-code | MUST | ONS authority code |

### Validation Rules

- notice-date must not be in the future
- local-authority-code must be valid ONS code
- notice-reference must be unique

---

## 2. Work Description Module

| field | required | notes |
|-------|----------|-------|
| work-summary | MUST | Short description of works |
| work-type | MUST | enum: alteration, extension, refurbishment, new-build, mixed |
| use-of-building | MUST | Use class or free-text |
| site-address | MUST | Structured address object |
| higher-risk-building | MUST | boolean |
| is-new-dwelling | MUST | boolean |
| is-minor-work | MAY | boolean |

### Site Address Component

| field | required |
|-------|----------|
| number-name | MUST |
| street | MUST |
| town-city | MUST |
| postcode | MUST |
| uprn | MAY |

---

## 3. Registered Building Control Approver Module

| field | required | notes |
|-------|----------|-------|
| rbca-reference | MUST | Unique RBCA identifier |
| rbca-name | MUST | Registered company name |
| rbca-address | MUST | Structured address |
| rbca-registration-number | MUST | Registration ID |
| contact-email | MUST | |
| contact-phone | MAY | |
| scope-confirmation | MUST | boolean |
| no-financial-interest-declared | MUST | boolean |

### Validation Rules

- rbca-registration-number must match approved register format
- no-financial-interest-declared must be true

---

## 4. Dutyholders Module

### Person Intending to Carry Out the Work

| field | required |
|-------|----------|
| name | MUST |
| address | MUST |
| email | MAY |
| telephone | MAY |

### Client (if different)

| field | required |
|-------|----------|
| name | MAY |
| address | MAY |
| email | MAY |
| telephone | MAY |

### Principal Contractor

| field | required |
|-------|----------|
| name | MAY |
| address | MAY |
| email | MAY |
| telephone | MAY |

---

## 5. Statutory Statements Module

This module captures legally required declarations.

| field | required | notes |
|-------|----------|-------|
| local-enactments-statement | MUST | text or "none" |
| optional-requirements-applicable | MAY | array |
| planning-permission-status | MAY | enum: granted, not-granted |
| public-electronic-comms-network | MAY | text or boolean |
| exemption-regulation-44ZB | MAY | boolean |
| exemption-regulation-44ZC | MAY | boolean |
| fso-building | MUST | boolean |

---

## 6. Drainage and Sewerage Module

| field | required |
|-------|----------|
| new-drain-connection | MUST |
| sewer-connection-details | MAY |
| discharge-proposals | MAY |
| sewer-consultation-required | MUST |

---

## 7. Commencement Module

| field | required |
|-------|----------|
| proposed-commencement-date | MUST |
| fifteen-percent-work-description | MAY |

### Validation Rules

- proposed-commencement-date must be ≥ notice-date
- fifteen-percent-work-description required unless exempt under regulation 16

---

## 8. Consultation Undertakings Module

| field | required |
|-------|----------|
| fire-authority-consultation-required | MUST |
| fire-authority-consultation-undertaken | MUST |
| sewerage-consultation-required | MUST |
| sewerage-consultation-undertaken | MUST |

---

## 9. Compliance and Registration Declarations

| field | required |
|-------|----------|
| rbca-aware-of-obligations | MUST |
| rbca-registered-for-work-scope | MUST |
| not-higher-risk-building | MUST |
| approval-on-register-confirmed | MUST |

---

## 10. Signatures Module

| field | required |
|-------|----------|
| rbca-signatory-name | MUST |
| rbca-signature-date | MUST |
| client-signatory-name | MAY |
| client-signature-date | MAY |

---

# Example (Anonymised JSON)

```json
{
  "notice-metadata": {
    "notice-reference": "IN-UUID-000001",
    "notice-date": "2025-01-10",
    "legislative-basis": "section-47-building-act-1984",
    "regulations-version": "rbc-approvers-england-2024",
    "local-authority-code": "E09000000"
  },
  "work-description": {
    "work-summary": "Internal refurbishment and formation of new facilities",
    "work-type": "alteration",
    "use-of-building": "office",
    "site-address": {
      "number-name": "1",
      "street": "Example Street",
      "town-city": "London",
      "postcode": "AA1 1AA",
      "uprn": "100000000000"
    },
    "higher-risk-building": false,
    "is-new-dwelling": false,
    "is-minor-work": false
  },
  "rbca": {
    "rbca-reference": "RBCA-0001",
    "rbca-name": "Example Building Control Ltd",
    "rbca-address": "Registered Address Placeholder",
    "rbca-registration-number": "REG-000001",
    "contact-email": "contact@example.com",
    "scope-confirmation": true,
    "no-financial-interest-declared": true
  },
  "dutyholders": {
    "person-intending-to-carry-out-work": {
      "name": "Placeholder Contractor Ltd",
      "address": "Contractor Address Placeholder"
    }
  },
  "commencement": {
    "proposed-commencement-date": "2025-02-01",
    "fifteen-percent-work-description": "Formation of internal partitions"
  },
  "consultations": {
    "fire-authority-consultation-required": true,
    "fire-authority-consultation-undertaken": true,
    "sewerage-consultation-required": false,
    "sewerage-consultation-undertaken": false
  },
  "declarations": {
    "rbca-aware-of-obligations": true,
    "rbca-registered-for-work-scope": true,
    "not-higher-risk-building": true,
    "approval-on-register-confirmed": true
  }
}
```
