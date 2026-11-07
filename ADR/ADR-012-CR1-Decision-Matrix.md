# ADR-012 — CR-1 Decision Matrix (Endoscopy)

**Document ID:** ADR-012-CR1  
**Linked to:** `ADR/ADR-012-Endoscopy-Module.md` (section 2.8)  
**Status:** **Preliminarily Approved — Pending Clinical Validation**  
**Product Owner preliminary approval:** 2026-11-07, for technical planning and preparation only  
**Overall:** E2 Technical Groundwork — Approved with Conditions · Clinical Activation — Blocked Pending Clinical Validation · Database Migration — Pending Separate Review and Approval  
**Designated clinical reviewer:** Dr. Mohammed Saleh — **has not reviewed or confirmed any item below**  
**Source sheet:** *Endoscopy CR-1 — Early Clinical Review Sheet* (Claude Docs, 2026-11-07)

---

## How to read this matrix

- **PO preliminary** = approved by the Product Owner as the design baseline for technical planning. It is **not** a clinical approval.
- **Clinical validation** = the gastroenterologist's confirmation. Every row is **Pending** until Dr. Mohammed Saleh reviews it personally; his review date and comments are recorded here when they exist. No row may be marked validated on anyone else's behalf.
- Nothing in this matrix is published, activated or enforced as a clinical rule until its row is validated (ADR-012 2.8).

## A. Procedure types and fields

| # | Item | PO preliminary decision | Rationale | Clinical validation | Gate |
|---|---|---|---|---|---|
| A1 | Seven V1 types: gastroscopy, colonoscopy, flexible sigmoidoscopy, ERCP, EUS, capsule endoscopy, other | Keep all seven | GI-only V1 scope (E-D4); already code-defined in E1 | Pending | E1 / E2 |
| A2 | Other | Procedure name required; optional free-text description; common fields only; never priced automatically | Avoids uncontrolled types and silent charges | Pending | E2 |
| A3 | Indication | Searchable list + free text | Structured reporting with room for detail | Pending (the list itself) | E3 |
| A4 | Sedation / anaesthesia | Type, medication, dose, responsible staff | Safety record of who sedated with what | Pending (type list) | E3 |
| A5 | Procedure start / end | System timestamps | Times cannot be back-dated | Pending | E3 |
| A6 | Endoscopist | Linked clinic user | Signing and accountability | Pending | E2 / E3 |
| A7 | Assisting staff | Several staff, each with a role (technical requirement, TD-08) | Real team composition | Pending (role list) | E3 |
| A8 | Endoscope ID | Text kept as recorded on each case | History never re-derived (E-P4) | Pending | E3 |
| A9 | Findings | Linked to an anatomical site | Site-level reporting | Pending | E3 |
| A10 | Interventions | Linked to a site where applicable; separate concept from findings | Findings ≠ actions | Pending | E3 |
| A11 | Specimens | Unique Specimen ID per specimen (technical requirement, TD-08); each links to a pathology lab order | E-D2; traceability | Pending | E4 |
| A12 | Complications | Description + immediate action | Safety record | Pending | E3 |
| A13 | Outcome | Completed / incomplete; reason required when incomplete | Quality and follow-up | Pending | E3 |
| A14 | Bowel preparation quality | Field prepared; **scale not chosen (Boston or other: pending)** | Scale is a clinical choice | Pending | E3 |
| A15 | Extent reached | Field prepared | Lower GI completeness | Pending | E3 |
| A16 | ERCP / EUS site list | Separate biliary / pancreatic site list | Different anatomy | Pending | E3 |
| A17 | Capsule | Capsule ID, ingestion time, reader, reading time, study complete | Capsule workflow differs | Pending | E3 |
| A18 | Caecal intubation, withdrawal time | Prepared as **proposed** colonoscopy quality indicators; not mandatory; caecal intubation criteria pending | No threshold invented | Pending | E3 |
| A19 | ASA classification | Configurable per the clinic's approved clinical policy; not mandatory by default | Practice-dependent | Pending | E2 / E3 |

## B. Lifecycle events

| # | Item | PO preliminary decision | Rationale | Clinical validation | Gate |
|---|---|---|---|---|---|
| B0 | When the case is created | At the explicit **Start Visit** only, never by opening a tab (TD-01) | ADR-012 E-P2 | Not clinical (PO decision) | E2 |
| B1 | Thirteen separate timestamped events: check-in, Start Visit, pre-procedure assessment, cleared / not cleared, time-out, sedation started, procedure started, scope insertion, scope withdrawal, procedure ended, recovery, discharge, report signed | Each recorded separately with its own time and actor; none inferred from another | Accurate timeline and audit | Pending | E2–E6 |
| B2 | Time-out completed before procedure start | Enforced | Patient safety | Pending | E3 |
| B3 | Procedure end not before procedure start | Enforced | Data integrity (technical) | Not clinical | E3 |
| B4 | Visit cannot close while the patient is in recovery | Enforced | Safety | Pending | E6 |
| B5 | Only the responsible endoscopist signs the report | Enforced | Accountability | Pending | E5 |
| B6 | Who signs the time-out (endoscopist, nurse, both) | **Open** | Practice-dependent | Pending | E3 |

## C. Pre-procedure assessment

| # | Item | PO preliminary decision | Rationale | Clinical validation | Gate |
|---|---|---|---|---|---|
| C1 | Allergies and current medications | Shown read-only from the main patient record; medications = active prescription items only, with status and source; discontinued / cancelled never shown as current (TD-05) | Single source of truth; no double entry | Pending | E2 |
| C2 | Anticoagulant / antiplatelet review | Fields: drug, last dose, plan (stopped / continued / bridged), note | Bleeding risk documentation | Pending (whether required for every patient) | E2 |
| C3 | Fasting, bowel preparation, consent, escort | Captured through the Endoscopy forms (D) | E-D3 | Pending | E2 |
| C4 | Cleared / not cleared | Authorised clinician (`procedures:manage`, TD-03); reason required for not cleared | Accountability | Pending (clinical criteria) | E2 |
| C5 | Clearance refused until required forms are signed | Enforced once the clinic has **published** the forms | ADR-012 E2 exit criteria | Pending (which forms are required) | E2 |
| C6 | INR, platelet count, other labs | INR and platelets shown when present on the record (TD-09); **no automatic thresholds or eligibility rules** | Information only | Pending (other labs, time window) | E2 |

## D. Draft forms (Core Forms assigned to Endoscopy)

| # | Item | PO preliminary decision | Rationale | Clinical validation | Gate |
|---|---|---|---|---|---|
| D1 | Informed consent | Structure prepared as a **draft** | E-D3 | Pending | E2 |
| D2 | Pre-procedure checklist | Draft | E-D3 | Pending | E2 |
| D3 | Time-out | Draft | E-D3 | Pending | E3 |
| D4 | Discharge | Draft | E-D3 | Pending | E6 |
| D5 | Publication | Added per clinic on request as drafts; only the clinic's designated clinical lead reviews, approves and publishes; the system never publishes them (TD-04) | Clinic ownership of clinical content | Not clinical (product rule) | E2 |

## E. Terminology catalogs

| # | Item | PO preliminary decision | Rationale | Clinical validation | Gate |
|---|---|---|---|---|---|
| E1 | Anatomical sites (upper GI, lower GI, biliary / pancreatic) | First draft list as in the sheet | Starting point | Pending; **not clinically complete** | E3 |
| E2 | Findings (17 terms) | First draft | Starting point | Pending; not complete | E3 |
| E3 | Interventions (12 terms) | First draft; separate from findings | Distinct concepts | Pending; not complete | E3 |
| E4 | Complications (11 terms) | First draft | Starting point | Pending; not complete | E3 |
| E5 | Incomplete-procedure reasons (6) | First draft | Starting point | Pending | E3 |
| E6 | Terminology rules: stable codes, EN + AR labels, version stored on each record, retire without deleting, controlled additions | Approved as technical rules | Historical meaning preserved (E-P3) | Not clinical | E3 |
| E7 | Arabic medical wording of every term | Draft wording | Shown to doctors and patients | Pending | E2–E3 |

## F. Records and corrections

| # | Item | PO preliminary decision | Rationale | Clinical validation | Gate |
|---|---|---|---|---|---|
| F1 | Five distinct linked records: Endoscopy Case, Endoscopy Report, Pathology Order, Pathology Result, Doctor Acknowledgement | Kept separate | Traceability; E-D2 | Pending (flow) | E2–E5 |
| F2 | Signed report never overwritten; correction by signed amendment keeping the original | Enforced | E-D1 | Already a PO decision | E5 |
| F3 | Biopsy specimens → normal Core lab / pathology order | Kept | E-D2 | Pending (flow) | E4 |

## Sign-off

| Role | Name | Status | Date |
|---|---|---|---|
| Product Owner | Ahmed | Preliminary approval for technical planning | 2026-11-07 |
| Clinical Reviewer (gastroenterologist) | Dr. Mohammed Saleh | **Not yet reviewed** | — |
