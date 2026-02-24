# Competent Persons Scheme Notifications (Exploratory Schema)

This document proposes an **illustrative data structure** for modelling  
**Competent Persons Scheme (CPS) notifications** as structured Building Control data.

All identifiers and references are anonymised.

This is not a formal standard — it is a discussion artefact.

---

## Design Intent

CPS notifications are:

- certificate-based
- post-completion
- installer-led
- often submitted in batches
- event-driven rather than approval-driven

This structure separates:

- global submission metadata
- individual building records
- site identification
- work details
- installer registration
- explicit validation rules

In CPS notifications, `site-location` refers to the **individual property to which the certificate applies**, rather than a wider development site.

---

# Schema Overview

---

## Global Details Module

Applies to the entire submission batch.

| field | required |
|-------|----------|
| sender-type | MUST |
| sender-code | MUST |
| local-authority-code | MUST |
| response-email | MUST |
| response-phone | MAY |
| submission-date | MUST |

---

## Building Records Container

| field | required |
|-------|----------|
| building-records | MUST |

- `building-records` MUST contain at least one record.
- Each record represents a single certified property.

---

## Building Record Component

Applies per property notification.

| field | required |
|-------|----------|
| status | MUST |
| record-type | MUST |
| site-details | MUST |
| property-type | MUST |
| work-details | MUST |
| work-description | MUST |
| installer-details | MUST |

---

## Site Location Component (per Building Record)

Each `building-record` MUST contain a `site-details.site-location` object.

| Field         | Type          | Required | Description                              |
|---------------|--------------|----------|------------------------------------------|
| site-details  | object        | MUST     | Container for site information           |
| site-location | object        | MUST     | Identifies the certified property        |
| address       | object        | MUST     | Structured postal address                |
| number-name   | string        | MUST     | Building number or name                  |
| street        | string        | MUST     | Street name                              |
| town-city     | string        | MUST     | Post town or city                        |
| postcode      | string        | MUST     | Valid UK postcode                        |
| uprns         | array[string] | MUST     | One or more 12-digit UPRNs               |

---

# Validation Rules

## MUST

- `building-records` MUST contain at least one record.
- Each `building-record` MUST include `site-details.site-location`.
- `uprns` MUST contain at least one entry.
- Each UPRN MUST be a 12-digit numeric string.
- `postcode` MUST conform to UK postcode format.
- `status` MUST be one of:
    - `new`
    - `update`
    - `cancelled`
- `date-work-completed` MUST be provided.
- `certificate-reference` MUST be unique per sender.

## MAY

- `uprns` MAY contain multiple entries where work spans multiple properties.
- `response-phone` MAY be omitted.
- `commissioning-required` MAY be false where not applicable.
- `work-description` MAY contain multiple controlled values.
- Future versions MAY include geometry or boundary data.

---

# Non-Goals

This exploratory schema does not:

- Model inspection workflows
- Represent approval states
- Replace certificate PDF artefacts
- Represent enforcement action
- Represent fee or payment status

---

# Example Anonymised JSON

```json
{
  "global-details": {
    "sender-type": "competent-persons-scheme",
    "sender-code": "SCHEME001",
    "local-authority-code": "E09000000",
    "response-email": "contact@scheme.example",
    "response-phone": "02070000000",
    "submission-date": "2025-12-01"
  },
  "building-records": [
    {
      "status": "new",
      "record-type": "competent-persons-scheme-notification",

      "site-details": {
        "site-location": {
          "address": {
            "number-name": "10",
            "street": "Example Street",
            "town-city": "London",
            "postcode": "AA1 1AA"
          },
          "uprns": ["100000000000"]
        }
      },

      "property-type": "dwelling",

      "work-details": {
        "sender-record-id": "SCHEME001_00001",
        "certificate-reference": "CERT_00001",
        "commissioning-required": false,
        "commissioning-carried-out": false,
        "green-deal-finance": false,
        "date-work-commenced": "2025-11-01",
        "date-work-completed": "2025-11-15"
      },

      "work-description": [
        "roof-covering-installation",
        "slating"
      ],

      "installer-details": {
        "installer-name": "Registered Installer",
        "registration-id": "REG12345",
        "telephone": "07123456789",
        "scheme-code": "SCHEME001"
      }
    }
  ]
}
