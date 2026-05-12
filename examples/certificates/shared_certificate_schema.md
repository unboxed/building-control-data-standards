# Shared Certificate Schema (Exploratory Schema)

This document proposes a canonical shared data structure for building control completion certificates.

The schema is designed to support:

- Final Certificates
- Completion Certificates
- future certificate variants
- machine-readable exchange
- API interoperability
- analytical consistency

This is not a statutory template. It is an exploratory discussion artefact.

---

# Design Intent

Although Final Certificates and Completion Certificates arise from different legal routes, both documents:

- identify completed regulated work
- identify the site to which the work relates
- record compliance assessment outcomes
- provide evidential certification
- contain declarations and limitations
- conclude a building control process

This schema therefore models certificates using a shared canonical structure with route-specific declarations.

---

# Canonical Certificate Structure

```json
{
  "certificate-type": "final-certificate | completion-certificate",
  "certificate-metadata": {},
  "issuing-body": {},
  "building-control-route": {},
  "site-details": {},
  "work-details": {},
  "compliance-assessment": {},
  "applicable-regulations": [],
  "declarations": [],
  "certificate-limitations": {},
  "authorisation": {}
}
```

---

# Schema Overview

---

## 1. Certificate Type Module

| field | required | notes |
|-------|----------|-------|
| certificate-type | MUST | `final-certificate` or `completion-certificate` |

### Validation Rules

- certificate-type must use an approved enumeration

---

## 2. Certificate Metadata Module

| field | required | notes |
|-------|----------|-------|
| certificate-reference | MUST | Unique certificate identifier |
| certificate-date | MUST | ISO 8601 date format |
| legislative-basis | MUST | Relevant legislative basis |
| application-reference | MUST | Associated building control application reference |

### Validation Rules

- certificate-reference must be unique
- certificate-date must not be in the future

---

## 3. Issuing Body Module

This module normalises local authority and Registered Building Control Approver issuers.

| field | required | notes |
|-------|----------|-------|
| issuing-body | MUST | Certificate issuer |
| issuer-type | MUST | `local-authority` or `registered-building-control-approver` |
| organisation-name | MUST | Issuing organisation |
| registration-number | MAY | RBCA registration identifier |
| ons-code | MAY | Local authority ONS code |

### Validation Rules

- issuer-type must use an approved enumeration
- registration-number should be supplied for registered building control approvers
- ons-code should be supplied for local authorities

---

## 4. Building Control Route Module

This module identifies the regulatory route under which the work was controlled.

| field | required | notes |
|-------|----------|-------|
| route-type | MUST | `initial-notice` or `local-authority` |
| initial-notice-reference | MAY | Required for Initial Notice route |
| initial-notice-date | MAY | Date Initial Notice given |

### Validation Rules

- initial-notice-reference must be supplied where route-type is `initial-notice`

---

## 5. Site Details Module

The certificate MUST identify the regulated site.

| field | required | notes |
|-------|----------|-------|
| site-details | MUST | Container for site information |
| site-location | MUST | Identifies regulated property |
| address | MUST | Structured postal address |
| uprns | MAY | One or more UPRNs |

### Validation Rules

- postcode must conform to UK postcode format
- UPRNs must be 12-digit numeric strings

---

## 6. Work Details Module

| field | required | notes |
|-------|----------|-------|
| work-description | MUST | Description of completed work |
| building-use | MAY | Building use classification |
| completion-date | MUST | Date work completed |
| partial-occupation | MAY | Indicates partial occupation |
| higher-risk-building | MAY | Indicates higher-risk building work |

### Validation Rules

- completion-date must not be in the future
- work-description must not be empty

---

## 7. Compliance Assessment Module

This module records the compliance conclusions reached by the issuing body.

| field | required | notes |
|-------|----------|-------|
| work-complete | MUST | Work completed |
| compliance-assessed | MUST | Compliance assessment undertaken |
| reasonable-steps-taken | MAY | Local authority reasonable steps confirmation |
| statutory-functions-performed | MAY | RBCA statutory functions confirmation |
| fire-safety-information-provided | MAY | Regulation 38 confirmation |
| evidence-status | MUST | Evidential status wording |

### Validation Rules

- mandatory fields must be boolean
- evidence-status must use approved wording

---

## 8. Applicable Regulations Module

This module records regulations relevant to the completed work.

| field | required | notes |
|-------|----------|-------|
| applicable-regulations | MAY | List of applicable regulations |

Example structure:

```json
{
  "regulation": "M4(2)",
  "description": "Accessible and adaptable dwellings",
  "applicable": true
}
```

### Validation Rules

- applicable must be boolean
- regulation identifiers should use standardised naming

---

## 9. Declarations Module

This module records route-specific statutory declarations.

| field | required | notes |
|-------|----------|-------|
| declarations | MAY | List of declarations |

Example structure:

```json
{
  "type": "client-statement-received",
  "status": true,
  "legislation": "regulation-18d"
}
```

Potential declaration types include:

- no-professional-interest
- client-statement-received
- contractor-statements-received
- designer-statements-received
- fire-authority-consulted
- registered-building-inspector-advice-obtained

### Validation Rules

- declaration status values must be boolean
- declaration types should use standardised enumerations

---

## 10. Certificate Limitations Module

This module records standard evidential limitations.

| field | required | notes |
|-------|----------|-------|
| existing-building-excluded | MAY | Existing unaffected building excluded |
| non-regulated-work-excluded | MAY | Non-regulated work excluded |
| fittings-and-repairs-excluded | MAY | Certain repairs and fittings excluded |

### Validation Rules

- limitation fields must be boolean

---

## 11. Authorisation Module

| field | required | notes |
|-------|----------|-------|
| authorised-by | MUST | Name of authorised signatory |
| authorised-role | MUST | Job title or role |
| signature-date | MUST | Date signed |
| organisation-name | MUST | Signing organisation |

### Validation Rules

- signature-date must not precede completion-date
- authorised-by must not be empty

---

# Example JSON

```json
{
  "certificate-type": "final-certificate",
  "certificate-metadata": {
    "certificate-reference": "fc-001245",
    "certificate-date": "2026-05-01",
    "legislative-basis": "section-51-building-act-1984",
    "application-reference": "BC-2025-000245"
  },
  "issuing-body": {
    "issuer-type": "registered-building-control-approver",
    "organisation-name": "Example Building Control Ltd",
    "registration-number": "RBCA-000123"
  },
  "building-control-route": {
    "route-type": "initial-notice",
    "initial-notice-reference": "IN-2025-001122",
    "initial-notice-date": "2025-01-06"
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
  "work-details": {
    "work-description": "Single storey rear extension",
    "building-use": "Residential dwelling",
    "completion-date": "2026-04-28",
    "partial-occupation": false,
    "higher-risk-building": false
  },
  "compliance-assessment": {
    "work-complete": true,
    "compliance-assessed": true,
    "statutory-functions-performed": true,
    "fire-safety-information-provided": true,
    "evidence-status": "evidence-but-not-conclusive"
  },
  "applicable-regulations": [
    {
      "regulation": "regulation-38",
      "description": "Fire safety information",
      "applicable": true
    },
    {
      "regulation": "M4(2)",
      "description": "Accessible and adaptable dwellings",
      "applicable": false
    }
  ],
  "declarations": [
    {
      "type": "client-statement-received",
      "status": true,
      "legislation": "regulation-18d"
    },
    {
      "type": "no-professional-interest",
      "status": true
    }
  ],
  "certificate-limitations": {
    "existing-building-excluded": true,
    "non-regulated-work-excluded": true,
    "fittings-and-repairs-excluded": true
  },
  "authorisation": {
    "authorised-by": "Alex Brown",
    "authorised-role": "Compliance Director",
    "signature-date": "2026-05-01",
    "organisation-name": "Example Building Control Ltd"
  }
}
```

---

# Relationship Between Documents

The repository structure intentionally separates:

- legal/procedural discussion
- canonical data structure

This allows:

| document | purpose |
|---|---|
| final_certificate.md | explains Final Certificate legal/process context |
| certificate_of_completion.md | explains Completion Certificate legal/process context |
| shared_certificate_schema.md | defines interoperable canonical structure |

This approach avoids duplication while preserving clarity around the differing legal routes.

---

These schemas were informed by common structures and statutory wording found in contemporary Final Certificates and Completion Certificates issued under the Building Act 1984 and Building Regulations 2010. 
