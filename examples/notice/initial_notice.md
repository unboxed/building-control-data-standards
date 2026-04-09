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

---

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

## 2. Site Location Module

The Initial Notice MUST identify the property to which it applies.

This schema aligns with the canonical `site-details.site-location` structure used across submission types.

| Field         | Type          | Required | Description                              |
|---------------|--------------|----------|------------------------------------------|
| site-details  | object        | MUST     | Container for site information           |
| site-location | object        | MUST     | Identifies the regulated property        |
| address       | object        | MUST     | Structured postal address                |
| number-name   | string        | MUST     | Building number or name                  |
| street        | string        | MUST     | Street name                              |
| town-city     | string        | MUST     | Post town or city                        |
| postcode      | string        | MUST     | Valid UK postcode                        |
| uprns         | array[string] | MUST     | One or more 12-digit UPRNs               |

### Validation Rules

- `uprns` MUST contain at least one entry.
- Each UPRN MUST be a 12-digit numeric string.
- `postcode` MUST conform to UK postcode format.

---

## 3. Work Description Module

| field | required | notes |
|-------|----------|-------|
| work-summary | MUST | Short description of works |
| work-type | MUST | enum: alteration, extension, refurbishment, new-build, mixed |
| use-of-building | MUST | Use class or free-text |
| higher-risk-building | MUST | boolean |
| is-new-dwelling | MUST | boolean |
| is-minor-work | MAY | boolean |

---

## 4. Registered Building Control Approver Module

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

## 5. Dutyholders Module

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

## 6. Statutory Statements Module

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

## 7. Drainage and Sewerage Module

| field | required |
|-------|----------|
| new-drain-connection | MUST |
| sewer-connection-details | MAY |
| discharge-proposals | MAY |
| sewer-consultation-required | MUST |

---

## 8. Commencement Module

| field | required |
|-------|----------|
| proposed-commencement-date | MUST |
| fifteen-percent-work-description | MAY |

### Validation Rules

- proposed-commencement-date must be ≥ notice-date
- fifteen-percent-work-description required unless exempt under regulation 16

---

## 9. Consultation Undertakings Module

| field | required |
|-------|----------|
| fire-authority-consultation-required | MUST |
| fire-authority-consultation-undertaken | MUST |
| sewerage-consultation-required | MUST |
| sewerage-consultation-undertaken | MUST |

---

## 10. Compliance and Registration Declarations

| field | required |
|-------|----------|
| rbca-aware-of-obligations | MUST |
| rbca-registered-for-work-scope | MUST |
| not-higher-risk-building | MUST |
| approval-on-register-confirmed | MUST |

---

## 11. Signatures Module

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

  "work-description": {
    "work-summary": "Internal refurbishment and formation of new facilities",
    "work-type": "alteration",
    "use-of-building": "office",
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
