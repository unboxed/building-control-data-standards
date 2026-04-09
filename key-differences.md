# Building Control Submission Matrix

This document provides a matrix between the three primary building control submission types in England:

- Full Plans
- Building Notice
- Initial Notice

It identifies which modules are:

- ✅ MUST — required for submission
- ◑ MAY — optional or situational
- ❌ NOT APPLICABLE — not typically part of the submission

This is an exploratory standardisation artefact intended to support consistent data modelling.

---

# Core application structure

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
application-fields | ✅ MUST | ✅ MUST | ✅ MUST | Common across all submissions
documents | ✅ MUST | ◑ MAY | ❌ | Initial Notices are notice-based, not plan-based
fee | ✅ MUST | ✅ MUST | ❌ | Fees handled outside local authority process for Initial Notice

---

# Applicant & identity

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
applicant | ◑ MAY | ✅ MUST | ◑ MAY | Building Notice always requires explicit applicant
agent | ◑ MAY | ◑ MAY | ◑ MAY | Present across all, not always required

---

# Site & location

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
site-details (site-location + UPRN) | ✅ MUST | ✅ MUST | ✅ MUST | Canonical component across all submission types
site-area | ◑ MAY | ❌ | ◑ MAY | More relevant for Full Plans and complex projects

---

# Proposed work & timing

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
proposed-work | ✅ MUST | ✅ MUST | ✅ MUST | Present in all submissions
proposed-start-date / commencement | ◑ MAY | ✅ MUST | ✅ MUST | Required for Building Notice and Initial Notice
planning-reference | ◑ MAY | ❌ | ◑ MAY | Rarely present in Building Notice flows

---

# Building details

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
building-characteristics (full) | ✅ MUST | ❌ | ◑ MAY | Building Notice deliberately avoids detailed design data
building-summary (lightweight) | ❌ | ✅ MUST | ◑ MAY | Simplified replacement for Building Notice
higher-risk / FSO classification | ✅ MUST | ❌ | ✅ MUST | Critical for regulatory classification and Initial Notice

---

# Dutyholders (post–Building Safety Act)

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
client | ✅ MUST | ✅ MUST | ✅ MUST | Required in all submissions
principal-designer | ✅ MUST | ◑ MAY | ✅ MUST | Often unknown at Building Notice stage
principal-contractor | ✅ MUST | ◑ MAY | ✅ MUST | Same as above
approver (Registered Building Control Approver) | ❌ | ❌ | ✅ MUST | Unique to Initial Notice

---

# Technical & regulatory detail

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
regulatory-declarations | ✅ MUST | ❌ | ◑ MAY | Typically absent in Building Notice submissions
drainage (detailed) | ✅ MUST | ❌ | ◑ MAY | Building Notice captures only high-level services
services (water/drainage basic) | ❌ | ✅ MUST | ◑ MAY | Building Notice-specific simplification
materials | ◑ MAY | ❌ | ◑ MAY | More common in Full Plans submissions
hazardous-substances | ◑ MAY | ❌ | ◑ MAY | Relevant for complex or regulated works

---

# Charges & fees

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
charges (floor area / cost) | ◑ MAY | ✅ MUST | ❌ | Core mechanism for Building Notice charging
fee | ✅ MUST | ✅ MUST | ❌ | Initial Notice fees handled outside submission

---

# Process & compliance

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
checklist | ◑ MAY | ❌ | ❌ | Primarily used in Full Plans validation workflows
conflict-of-interest | ◑ MAY | ❌ | ✅ MUST | Critical for private sector building control
site-visit-details | ◑ MAY | ❌ | ◑ MAY | Relevant for inspection planning

---

# Declaration

module | Full Plans | Building Notice | Initial Notice | notes
-- | -- | -- | -- | --
declaration | ✅ MUST | ✅ MUST | ✅ MUST | Required across all submission types

---

# Summary

## Universal modules (present in all submission types)

- application-fields
- site-details
- proposed-work
- declaration
- client (within dutyholders)

## Full Plans dominant modules

- building-characteristics
- drainage (detailed)
- regulatory-declarations
- documents (required)

## Building Notice dominant modules

- building-summary
- services (basic)
- charges
- proposed-start-date (mandatory)

## Initial Notice dominant modules

- approver (Registered Building Control Approver)
- conflict-of-interest
- strict dutyholder requirements

---

# Notes

- This matrix is based on observed practice across local authority and private sector building control submissions.
- It is intended to support:
    - consistent API design
    - data standardisation
    - interoperability across systems
- It does not represent statutory wording or a mandated schema.

---