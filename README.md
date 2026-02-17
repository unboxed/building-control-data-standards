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
- references **codelists** rather than free text
- applies explicit **validation rules**
- separates **core application metadata** from domain-specific modules

> The fields defined here are a **subset and adaptation** of those recommended for planning, adjusted to reflect Building Control needs.

---

# Beyond Full Plans: Competent Persons Scheme Notifications

Building Control does not consist solely of Full Plans and Building Notices.

A significant proportion of regulatory activity arises from:

- **Competent Persons Scheme (CPS) notifications**
- Self-certified work (e.g. electrical, roofing, heating installations)
- Bulk notifications submitted by scheme operators
- Post-completion certificate-based workflows

These differ fundamentally from Full Plans:

| Full Plans | CPS Notification |
|------------|-----------------|
| Pre-approval | Post-completion notification |
| Authority assesses compliance | Installer self-certifies |
| Requires detailed plans | Requires certificate reference |
| May involve inspection | Usually notification only |
| Applicant-led | Scheme-led |

Key characteristics of CPS notifications:

- Often submitted in **batches**
- Structured around a **certificate reference**
- Installer registration is central
- Limited technical drawing requirement
- Event-driven rather than approval-driven

This repository includes an exploratory CPS model in:

