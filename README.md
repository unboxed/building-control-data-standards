# Building Control Application Data Standards (Exploratory)

This repository is an **open, exploratory space** for discussing and shaping what a **standardised data model for Building Control applications** could look like in England.

It builds on the **MHCLG Planning Application Data Specification**, but explores how similar principles might be applied to **Building Control**—particularly **Full Plans** applications—where data requirements, validation needs, and downstream processes differ significantly from planning.

The intent is **not** to propose a finished or authoritative standard, but to:
- provide concrete examples,
- surface gaps and ambiguities,
- and prompt informed debate across local authorities, software suppliers, and digital practitioners.

---

## Why this exists

Planning data standards have benefited from:
- clear schemas,
- shared codelists,
- validation rules,
- and a growing open data ecosystem.

Building Control, by contrast:
- remains largely document-led,
- varies significantly between authorities,
- and lacks a shared, structured representation of applications.

This repository explores what it might look like if Building Control applications:
- were submitted as **structured data first**
- and with clear, machine-readable validation rules.

---

## Status and scope

- **Exploratory / non-normative**
- **Designed to provoke discussion, not mandate adoption**
- The `/examples` directory contains **illustrative schema fragments**, not full implementations
- Currently focuses on **submission-time data only**

There is **no claim of completeness**, correctness, or alignment with existing statutory forms.

---

## Relationship to MHCLG Planning Data Standards

The structure and naming conventions deliberately mirror those used in the  
[MHCLG Planning Application Data Specification](https://github.com/digital-land/planning-application-data-specification).

Where possible, this work:
- reuses planning-style **modules**
- references **codelists** rather than free text
- applies explicit **validation rules**
- separates **core application metadata** from domain-specific modules

> The fields defined here are a **subset and adaptation** of those recommended for planning, adjusted to reflect Building Control needs.

---

## What is currently modelled

The repository currently includes a **proposed submission schema** covering:

- Core application metadata
- Applicants and agents
- Contact details
- Site location and area
- Documents and files
- Fees and transactions
- Declarations
- Conflict of interest
- Materials
- Foul sewage disposal
- Hazardous substances
- Site visit details
- Validation rules and constraints
- Example codelists (e.g. building elements, use classes, tenure types)

The emphasis is on:
- explicit references
- modularity
- internal consistency
- machine validation

---

## What is deliberately missing (and why)

The following areas are **explicitly not yet modelled**, despite being required on real Full Plans forms. These gaps are intentional and highlighted to prompt discussion.

### Roles beyond applicant and agent

Full Plans applications often distinguish between:
- client
- applicant
- principal contractor
- principal designer

This schema currently includes:
- `applicant`
- optional `agent`

…but does **not** explicitly model:
- client consent
- contractor responsibilities
- designer accountability

---

### Existing vs proposed building characteristics

Full Plans forms require:
- existing and proposed use
- number of storeys
- building height

This schema:
- includes use class and site data
- does **not** distinguish existing vs proposed
- does **not** model height or storey count

---

### General description of proposed works

Although specific domains are covered (materials, drainage, site),
there is **no single canonical field** for:

- narrative description of proposed works
- summary of material change or change of use

---

### Drainage, water, and highway checks

While foul sewage disposal is included, the schema does not yet cover:
- surface water drainage
- water supply details
- proximity to public sewers
- private street or highway relationships

---

### Technical drawings and calculations

The schema includes a generic `documents[]` structure but does not:
- mandate specific drawing types
- distinguish technical calculations
- encode plan scope or drawing completeness

Examples currently missing explicit treatment:
- structural calculations
- insulation / ventilation specifications
- glazing details
- block plans
- foundation and drainage layouts

---

### Regulatory and safety declarations

Common Full Plans checks not currently modelled include:
- fire safety
- Part P electrical works
- sewer proximity
- tree constraints
- Party Wall matters
- statutory safety declarations

---

### Dates, consent, and statutory timeframes

The schema includes:
- `submission-date`

It does **not** include:
- commencement date
- expected start or completion date
- consent to conditional approval
- consent to extend statutory decision periods

---

### Client consent where applicant is not the client

Where someone submits on behalf of the client, Full Plans forms require:
- a signed client consent statement

This schema:
- does not include a distinct client role
- does not include a consent declaration for that client

---

## Design principles being explored

- **Data first, documents second**
- **Explicit validation rules**
- **References over repetition**
- **Codelists over free text**
- **Separation of concerns via modules**
- **Alignment (where useful) with planning standards**
- **Acknowledgement of Building Control’s additional technical depth**

---

## How to use this repository

You can:
- review and critique the proposed structures
- compare them to existing Building Control forms
- suggest missing modules or fields
- challenge assumptions about validation or scope
- propose alternative modelling approaches

There is **no expectation** that this schema is “right” — only that it is **concrete enough to argue with**.

---

## Contributions and discussion

Issues, comments, and alternative proposals are very welcome.

Useful contributions might include:
- identifying statutory requirements not represented here
- proposing additional modules
- suggesting where alignment with planning breaks down
- highlighting authority-specific constraints
- sharing real-world submission pain points

---

## Licence and intent

This repository is published openly to support:
- discussion,
- experimentation,
- and better shared understanding of Building Control data needs.

It is **not** an official standard, endorsement, or policy proposal.

---

> *If planning data standards made planning applications easier to validate, exchange, and analyse — what would the equivalent look like for Building Control?*
