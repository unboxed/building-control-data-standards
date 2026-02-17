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
- work details
- installer registration
- explicit validation rules

---

# Schema Overview

## Global Details Module

| field | required |
|-------|----------|
| sender-type | MUST |
| sender-code | MUST |
| local-authority-code | MUST |
| response-email | MUST |
| response-phone | MAY |
| submission-date | MUST |

---

## Building Record Component

| field | required |
|-------|----------|
| status | MUST |
| record-type | MUST |
| work-address | MUST |
| property-type | MUST |
| work-details | MUST |
| work-description | MUST |
| installer-details | MUST |

---

## Example Anonymised JSON

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
      "work-address": {
        "number-name": "10",
        "street": "Example Street",
        "town-city": "London",
        "postcode": "AA1 1AA",
        "uprn": "100000000000"
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
