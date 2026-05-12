# Shared Notice Schema (Exploratory Schema)

This document proposes a canonical shared data structure for building control notices.

The schema is designed to support:

* Building Notices
* Initial Notices
* future notice variants
* machine-readable exchange
* API interoperability
* analytical consistency

This is not a statutory template. It is an exploratory discussion artefact.

---

# Design Intent

Although Building Notices and Initial Notices arise from different legal routes, both documents:

* notify commencement of regulated building work
* identify the site to which the work relates
* identify the client and relevant parties
* describe the proposed work
* establish a building control route
* initiate a statutory process

This schema therefore models notices using a shared canonical structure with route-specific declarations and validation rules.

---

# Canonical Notice Structure

```json
{
  "notice-type": "building-notice | initial-notice",

  "notice-metadata": {},

  "parties": {},

  "site-details": {},

  "proposed-work": {},

  "dutyholders": {},

  "building-control-route": {},

  "technical-submission": {},

  "declarations": [],

  "documents": []
}
```

---

# Schema Overview

---

## 1. Notice Type Module

| field       | required | notes                                 |
| ----------- | -------- | ------------------------------------- |
| notice-type | MUST     | `building-notice` or `initial-notice` |

### Validation Rules

* notice-type must use an approved enumeration

---

## 2. Notice Metadata Module

| field             | required | notes                      |
| ----------------- | -------- | -------------------------- |
| notice-reference  | MUST     | Unique notice identifier   |
| notice-date       | MUST     | ISO 8601 date format       |
| submission-date   | MUST     | Date submitted             |
| legislative-basis | MUST     | Relevant legislative basis |

### Validation Rules

* notice-reference must be unique
* notice-date must not be in the future
* submission-date must not be in the future

---

## 3. Parties Module

This module records organisations and individuals associated with the notice.

| field               | required | notes                                |
| ------------------- | -------- | ------------------------------------ |
| client              | MUST     | Person commissioning the work        |
| agent               | MAY      | Agent acting on behalf of client     |
| issuing-body        | MAY      | Registered Building Control Approver |
| receiving-authority | MUST     | Local authority receiving notice     |

### Validation Rules

* receiving-authority must be a valid local authority
* issuing-body should be supplied for Initial Notices
* client must contain contact information

---

## 4. Site Details Module

The notice MUST identify the regulated site.

| field         | required | notes                          |
| ------------- | -------- | ------------------------------ |
| site-details  | MUST     | Container for site information |
| site-location | MUST     | Identifies regulated property  |
| address       | MUST     | Structured postal address      |
| uprns         | MAY      | One or more UPRNs              |

### Validation Rules

* postcode must conform to UK postcode format
* UPRNs must be 12-digit numeric strings

---

## 5. Proposed Work Module

This module records the intended building work.

| field                | required | notes                               |
| -------------------- | -------- | ----------------------------------- |
| work-description     | MUST     | Description of proposed work        |
| proposed-start-date  | MAY      | Intended commencement date          |
| building-use         | MAY      | Intended building use               |
| work-category        | MAY      | Classification of work              |
| higher-risk-building | MAY      | Indicates higher-risk building work |

### Validation Rules

* work-description must not be empty
* proposed-start-date must not be in the past when initially submitted

---

## 6. Dutyholders Module

This module records relevant dutyholders associated with the work.

| field                | required | notes                             |
| -------------------- | -------- | --------------------------------- |
| client-dutyholder    | MUST     | Client under building regulations |
| principal-designer   | MAY      | Principal designer                |
| principal-contractor | MAY      | Principal contractor              |
| designers            | MAY      | Additional designers              |
| contractors          | MAY      | Additional contractors            |

### Validation Rules

* dutyholder contact information should be provided where applicable
* principal dutyholders should be supplied for applicable work types

---

## 7. Building Control Route Module

This module identifies the regulatory route under which the work will proceed.

| field                    | required | notes                                 |
| ------------------------ | -------- | ------------------------------------- |
| route-type               | MUST     | `local-authority` or `initial-notice` |
| initial-notice-reference | MAY      | Required for Initial Notice route     |
| initial-notice-date      | MAY      | Date Initial Notice given             |
| rbca-registration-number | MAY      | RBCA registration identifier          |

### Validation Rules

* initial-notice-reference must be supplied where route-type is `initial-notice`
* rbca-registration-number should be supplied for Initial Notices

---

## 8. Technical Submission Module

This module records technical information accompanying the notice.

| field                   | required | notes                              |
| ----------------------- | -------- | ---------------------------------- |
| estimated-cost          | MAY      | Estimated project cost             |
| drainage-details        | MAY      | Drainage proposals                 |
| energy-strategy         | MAY      | Energy compliance information      |
| fire-safety-information | MAY      | Fire safety information            |
| plans-provided          | MAY      | Indicates plans submitted          |
| specifications-provided | MAY      | Indicates specifications submitted |

### Validation Rules

* estimated-cost should be numeric
* technical fields should reflect submitted information

---

## 9. Declarations Module

This module records statutory and procedural declarations.

| field        | required | notes                |
| ------------ | -------- | -------------------- |
| declarations | MAY      | List of declarations |

Example structure:

```json
{
  "type": "rbca-registered",
  "status": true
}
```

Potential declaration types include:

* rbca-registered
* work-within-registration-scope
* joint-client-approval
* no-professional-interest
* higher-risk-building-declared

### Validation Rules

* declaration status values must be boolean
* declaration types should use standardised enumerations

---

## 10. Documents Module

This module records associated submitted documents.

| field     | required | notes                          |
| --------- | -------- | ------------------------------ |
| documents | MAY      | Submitted supporting documents |

Example structure:

```json
{
  "document-type": "existing-plan",
  "document-reference": "doc-001245",
  "file-format": "pdf"
}
```

### Validation Rules

* document references must be unique
* file formats should use approved enumerations

---

# Example JSON

```json
{
  "notice-type": "initial-notice",

  "notice-metadata": {
    "notice-reference": "in-2026-001245",
    "notice-date": "2026-05-01",
    "submission-date": "2026-05-01",
    "legislative-basis": "building-act-1984-section-47"
  },

  "parties": {
    "client": {
      "name": "Jane Example",
      "email": "jane@example.com"
    },
    "agent": {
      "name": "Example Architects Ltd"
    },
    "issuing-body": {
      "organisation-name": "Example Building Control Ltd"
    },
    "receiving-authority": {
      "organisation-name": "Example District Council"
    }
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

  "proposed-work": {
    "work-description": "Single storey rear extension",
    "proposed-start-date": "2026-06-01",
    "building-use": "Residential dwelling",
    "higher-risk-building": false
  },

  "dutyholders": {
    "client-dutyholder": {
      "name": "Jane Example"
    },
    "principal-designer": {
      "name": "Example Architects Ltd"
    },
    "principal-contractor": {
      "name": "Example Construction Ltd"
    }
  },

  "building-control-route": {
    "route-type": "initial-notice",
    "initial-notice-reference": "IN-2026-001245",
    "initial-notice-date": "2026-05-01",
    "rbca-registration-number": "RBCA-000123"
  },

  "technical-submission": {
    "estimated-cost": 85000,
    "plans-provided": true,
    "specifications-provided": true
  },

  "declarations": [
    {
      "type": "rbca-registered",
      "status": true
    },
    {
      "type": "work-within-registration-scope",
      "status": true
    }
  ],

  "documents": [
    {
      "document-type": "proposed-plan",
      "document-reference": "doc-001",
      "file-format": "pdf"
    }
  ]
}
```

---

# Relationship Between Documents

The repository structure intentionally separates:

* legal/procedural discussion
* canonical data structure

This allows:

| document                | purpose                                        |
| ----------------------- | ---------------------------------------------- |
| building_notice.md      | explains Building Notice legal/process context |
| initial_notice.md       | explains Initial Notice legal/process context  |
| shared_notice_schema.md | defines interoperable canonical structure      |

This approach avoids duplication while preserving clarity around the differing legal routes.
