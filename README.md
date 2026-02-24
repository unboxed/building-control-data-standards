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
- were modular and machine-validatable
- and treated documents as supporting artefacts rather than the primary payload

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
- applies explicit **validation rules**
- separates **core application metadata** from domain-specific modules

> The fields defined here are a **subset and adaptation** of those recommended for planning, adjusted to reflect Building Control needs and legislation.

---

# Beyond Full Plans

Building Control does not consist solely of Full Plans and Building Notices.

This repository includes an exploratory CPS model and also one for Initial Notices.

