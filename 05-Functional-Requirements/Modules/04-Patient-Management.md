# Patient Management Module

---

## Document Information

| Item | Value |
|------|-------|
| Document ID | FR-PAT-001 |
| Module | Patient Management |
| Section | Functional Requirements |
| Version | 1.0 |
| Status | Draft |
| Owner | Product Management |
| Last Updated | 2026-07-25 |

---
# Business Objective

The Patient Management Module is responsible for managing the complete patient lifecycle within the LOUTAS Care platform.

The module provides a centralized and secure patient registry that enables healthcare organizations to create, maintain, search, and manage patient demographic, administrative, and clinical identification information throughout the patient's relationship with the organization.

It serves as the authoritative source of patient identity and supports seamless integration with Appointments, Reception, EMR, Billing, Laboratory, Radiology, Pharmacy, Reporting, and other clinical and administrative modules.

The Patient Management Module ensures accurate patient identification, prevents duplicate records, supports multi-branch organizations, maintains comprehensive patient history, and complies with organizational governance, privacy, and security policies.
# Scope

The Patient Management Module includes the functional requirements for:

- Patient Registration
- Patient Profile Management
- Patient Search
- Patient Demographic Information
- Medical Record Number (MRN) Management
- National Identification Management
- Contact Information Management
- Emergency Contact Management
- Insurance Information Management
- Patient Alerts and Flags
- Allergies and Clinical Alerts Reference
- Multi-Branch Patient Access
- Patient Merge Management
- Duplicate Patient Detection
- Patient Status Management
- Patient Photo Management
- Patient Document Management
- Patient History Overview
- Consent Management
- Patient Portal Readiness
- Audit Trail for Patient Records
- # Primary Users

The Patient Management Module supports the following primary users:

- Receptionist
- Registration Officer
- Physician
- Nurse
- Branch Manager
- Medical Records Staff
- Health Information Management (HIM) Staff
- System Administrator

Secondary users include:

- Cashier (patient verification)
- Laboratory Staff (patient identification)
- Radiology Staff (patient identification)
- Pharmacy Staff (patient verification)
- Call Center Staff (patient lookup and appointment coordination)
- # Primary Users

The Patient Management Module supports the following primary users:

- Receptionist
- Registration Officer
- Physician
- Nurse
- Medical Records Staff
- Branch Manager
- Clinic Owner
- System Administrator

Secondary users include:

- Cashier (patient verification)
- Laboratory Staff (patient identification)
- Radiology Staff (patient identification)
- Pharmacy Staff (patient verification)
- Insurance Coordinator
- Call Center Staff (patient lookup and appointment support)
- # Module Vision

The Patient Management Module is the authoritative source of patient identity and demographic information within the LOUTAS Care platform.

Its purpose is to ensure that every patient is uniquely identified through a standardized, secure, and reliable registration process that supports safe, efficient, and coordinated healthcare delivery.

The module shall:

- Maintain a single, unified patient record across all clinics and branches.
- Prevent duplicate patient records through identity validation and duplicate detection.
- Support configurable Medical Record Number (MRN) generation policies.
- Maintain complete demographic and administrative patient information.
- Enable secure sharing of patient identity across integrated modules.
- Support patient alerts, administrative flags, and consent management.
- Maintain complete traceability and auditability for all patient record changes.
- Provide a scalable foundation for future patient engagement services, including patient portals and digital identity integration.

The Patient Management Module shall serve as the master patient registry of the platform while remaining flexible enough to support future interoperability standards, national health identifiers, and healthcare information exchange initiatives.
# Functional Overview

The Patient Management Module manages the complete patient identity lifecycle from initial registration through ongoing profile maintenance and administrative management.

The module provides centralized patient identity management while ensuring data accuracy, patient uniqueness, regulatory compliance, and seamless interoperability across all LOUTAS Care modules.

The core functional capabilities include:

- Registering new patients.
- Generating and managing Medical Record Numbers (MRN).
- Maintaining demographic and administrative patient information.
- Managing contact and emergency contact information.
- Recording insurance information.
- Managing patient photographs and identification documents.
- Detecting and preventing duplicate patient records.
- Merging duplicate patient records under controlled governance.
- Managing patient alerts and administrative flags.
- Supporting patient consent management.
- Providing comprehensive patient search capabilities.
- Maintaining patient audit history and record traceability.
- Supporting multi-branch patient access while preserving a unified patient identity.

The Patient Management Module serves as the foundation for all patient-related workflows by providing a trusted and authoritative patient registry that is shared across clinical, administrative, and financial modules.
# Business Rules

The Patient Management Module shall operate according to the following business rules to ensure accurate patient identification, data integrity, regulatory compliance, and consistent patient management across the LOUTAS Care platform.

---

## BR-PAT-001 Patient Registration

Every patient registered within the system shall be assigned a unique internal Patient ID.

Each patient record shall include, at a minimum:

- Medical Record Number (MRN)
- Full Name
- Date of Birth
- Gender
- Primary Contact Information

Additional demographic and administrative information may be collected according to organizational policies.

The system shall prevent incomplete patient registration when mandatory information is missing.
---

## BR-PAT-002 Patient Identity Validation

The system shall validate patient identity before creating a new patient record.

Identity validation shall use configurable matching criteria, which may include one or more of the following:

- National ID
- Passport Number
- Medical Record Number (MRN)
- Full Name
- Date of Birth
- Mobile Number

If potential duplicate records are detected, the system shall notify the user and require appropriate action according to organizational policy.

Organizations may configure duplicate detection rules based on their operational requirements.
---

## BR-PAT-003 Medical Record Number (MRN) Management

Every patient shall be assigned a unique Medical Record Number (MRN) according to the organization's configured numbering policy.

The MRN shall:

- Be unique within the organization.
- Remain permanently assigned to the patient.
- Never be reused.
- Be generated automatically or entered manually based on organizational configuration.
- Be searchable throughout the platform.

Changes to an existing MRN shall only be permitted for authorized users and shall be fully recorded in the audit trail.
---

## BR-PAT-004 Duplicate Patient Detection

The system shall detect potential duplicate patient records before creating a new patient.

Duplicate detection shall be performed using configurable matching rules that may include:

- Medical Record Number (MRN)
- National ID or Passport Number
- Full Name
- Date of Birth
- Mobile Number
- Email Address

When a potential duplicate is detected:

- The system shall display the matching patient records.
- The user shall be warned before creating a new patient record.
- Authorized users may proceed according to organizational policy.
- All override actions shall be recorded in the audit trail.

Organizations may configure duplicate detection thresholds and matching criteria based on operational requirements.
---

## BR-PAT-005 Patient Record Merge

The system shall support the controlled merging of duplicate patient records.

Patient record merging shall only be performed by authorized users with appropriate permissions.

During the merge process:

- A primary patient record shall be selected.
- Secondary patient records shall be merged into the primary record.
- Historical references shall be preserved.
- Related appointments, encounters, invoices, laboratory orders, radiology orders, prescriptions, and other linked records shall remain associated with the primary patient record.
- The original merged records shall not be permanently deleted.
- All merge activities shall be fully recorded in the audit trail.

Organizations may require managerial approval before completing a patient merge operation.
---

## BR-PAT-006 Patient Status Management

The system shall maintain a lifecycle status for every patient record.

The default patient statuses shall include:

- Active
- Inactive
- Deceased
- Archived (optional, according to organizational policy)

Patient status shall determine the availability of patient-related operations throughout the platform.

The system shall enforce the following rules:

- Active patients may receive appointments, clinical encounters, and other healthcare services.
- Inactive patients shall remain searchable and accessible for historical reference but may be restricted from new operational activities according to organizational policy.
- Deceased patients shall not be eligible for new appointments or clinical encounters.
- Archived patient records shall remain read-only and available only to authorized users.

All patient status changes shall be recorded in the audit trail, including the user, timestamp, previous status, new status, and reason for the change.
---

## BR-PAT-007 Patient Alerts and Administrative Flags

The system shall support configurable patient alerts and administrative flags to improve patient safety and operational awareness.

Patient alerts may include, but are not limited to:

- Severe Allergies
- Infectious Diseases
- Fall Risk
- Special Needs
- VIP Patient
- Financial Hold
- Legal Restrictions
- Administrative Notes

Alerts shall be classified according to organizational configuration (e.g., Clinical, Administrative, Financial).

The system shall display active alerts prominently wherever the patient record is accessed, including:

- Reception
- Appointment Management
- EMR
- Billing
- Pharmacy
- Laboratory
- Radiology

Organizations may configure alert severity levels such as:

- Information
- Warning
- Critical

Critical alerts shall require acknowledgement before the user proceeds with patient-related activities.

All alert creation, modification, activation, and deactivation events shall be recorded in the audit trail.
---

## BR-PAT-008 Multi-Branch Patient Access

The system shall maintain a single unified patient identity across all branches within the same organization.

Patient demographic information shall be shared according to organizational data access policies while respecting branch-level security and privacy controls.

The system shall support:

- Viewing patient records from authorized branches.
- Creating appointments for patients at any authorized branch.
- Maintaining a unified patient history across branches.
- Recording the branch associated with each patient interaction.
- Applying branch-specific permissions where required.

Organizations may configure branch-level access restrictions according to operational and regulatory requirements.

All cross-branch patient access events shall be recorded in the audit trail.
---

## BR-PAT-009 Audit Trail and Record Traceability

The system shall maintain a complete audit trail for all patient record activities to ensure accountability, regulatory compliance, and data integrity.

Audit events shall include, but are not be limited to:

- Patient Registration
- Patient Record Updates
- Patient Record Merge
- Patient Status Changes
- Contact Information Changes
- Insurance Information Updates
- Patient Alert Management
- Consent Management
- Cross-Branch Patient Access

Each audit record shall include:

- Event Type
- Patient Identifier
- User
- Date and Time
- Previous Value (where applicable)# Functional Requirements

---

## FR-PAT-001

### Title

Register New Patient

### Business Value

Provide a standardized, secure, and efficient process for creating a new patient record while ensuring patient identity accuracy, preventing duplicate records, and establishing a trusted foundation for all future clinical and administrative activities.

### Primary Actors

- Receptionist
- Registration Officer
- Authorized Administrative Staff

### Preconditions

- User is authenticated.
- User has permission to register patients.
- Required patient information is available.

### Trigger

The user selects **Register New Patient** from the Patient Management or Reception module.

### Description

The system shall allow authorized users to register a new patient by capturing the required demographic and administrative information.

The registration process shall include:

- Full Name
- Date of Birth
- Gender
- National ID or Passport Number (where applicable)
- Mobile Number
- Address
- Emergency Contact Information
- Insurance Information (optional)
- Patient Photograph (optional)

Before creating the patient record, the system shall automatically perform duplicate patient detection according to the configured organizational matching rules.

If validation succeeds, the system shall:

- Generate a unique internal Patient ID.
- Generate or assign a Medical Record Number (MRN).
- Create the patient record.
- Record the registration event in the audit trail.

### Expected Result

- Patient record is created successfully.
- Patient receives a unique Patient ID.
- Medical Record Number (MRN) is assigned.
- Duplicate records are minimized.
- Registration activity is recorded in the audit trail.

### Related Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-002 Patient Identity Validation
- BR-PAT-003 Medical Record Number (MRN) Management
- BR-PAT-004 Duplicate Patient Detection

### Related Modules

- Reception
- Appointments
- EMR
- Billing
- Audit

### Priority

Critical

### Acceptance Criteria

- Mandatory fields are validated.
- Duplicate detection executes before record creation.
- Patient ID and MRN are generated successfully.
- Patient record is created only once.
- Audit trail records the registration event.
- ---

## FR-PAT-002

### Title

Search Patient

### Business Value

Enable authorized users to quickly locate patient records using flexible search criteria, improving operational efficiency, reducing registration errors, and ensuring rapid access to patient information.

### Primary Actors

- Receptionist
- Registration Officer
- Physician
- Nurse
- Medical Records Staff
- Authorized Administrative Staff

### Preconditions

- User is authenticated.
- User has permission to access patient records.

### Trigger

The user enters search criteria in the Patient Search interface.

### Description

The system shall allow authorized users to search for patients using one or more of the following criteria:

- Medical Record Number (MRN)
- Patient Name
- National ID
- Passport Number
- Mobile Number
- Date of Birth
- Patient ID
- Insurance Number (where applicable)

The system shall support partial matching where appropriate and display matching patient records according to the user's access permissions.

Search results shall display key patient information, including:

- Patient Name
- MRN
- Date of Birth
- Gender
- Mobile Number
- Patient Status
- Primary Branch (if applicable)

### Expected Result

- Matching patient records are displayed.
- Search results are returned accurately and efficiently.
- Users can open the selected patient profile directly from the search results.

### Related Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-002 Patient Identity Validation
- BR-PAT-008 Multi-Branch Patient Access

### Related Modules

- Reception
- Appointments
- EMR
- Billing
- Laboratory
- Radiology
- Pharmacy

### Priority

Critical

### Acceptance Criteria

- Search supports multiple search criteria.
- Partial search returns appropriate results.
- Results respect role-based and branch-level permissions.
- Users can open the selected patient record directly.
- Search performance meets platform response standards.
- ---

## FR-PAT-003

### Title

View Patient Profile

### Business Value

Provide authorized users with a comprehensive and centralized view of patient information, enabling efficient clinical and administrative decision-making while maintaining a single source of truth for patient identity.

### Primary Actors

- Physician
- Nurse
- Receptionist
- Medical Records Staff
- Authorized Administrative Staff

### Preconditions

- User is authenticated.
- User has permission to view patient records.
- Patient record exists.

### Trigger

The user opens a patient record from search results or another integrated module.

### Description

The system shall display a comprehensive patient profile containing demographic, administrative, and summary information.

The patient profile shall include, where applicable:

#### Patient Identity

- Patient Photograph
- Full Name
- Medical Record Number (MRN)
- Patient ID
- National ID / Passport Number
- Date of Birth
- Age
- Gender
- Marital Status
- Nationality

#### Contact Information

- Mobile Number
- Secondary Contact Number
- Email Address
- Residential Address
- Emergency Contact

#### Administrative Information

- Patient Status
- Registration Date
- Primary Branch
- Insurance Information
- Preferred Language
- Consent Status

#### Clinical Summary (Read-Only)

- Allergies
- Patient Alerts
- Chronic Conditions (Summary)
- Last Visit Date
- Primary Physician
- Active Care Programs (if applicable)

The profile shall provide navigation to related modules without duplicating ownership of clinical or financial data.

### Expected Result

- Patient profile loads successfully.
- Information is displayed according to user permissions.
- Clinical summaries are presented as references only.
- Users can navigate to related modules from the profile.

### Related Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-007 Patient Alerts and Administrative Flags
- BR-PAT-008 Multi-Branch Patient Access
- BR-PAT-009 Audit Trail and Record Traceability

### Related Modules

- Appointments
- EMR
- Billing
- Laboratory
- Radiology
- Pharmacy
- Documents

### Priority

Critical

### Acceptance Criteria

- Patient profile displays the latest demographic information.
- Clinical summary information is presented in read-only mode.
- Navigation to related modules works correctly.
- Displayed information respects role-based and branch-level permissions.
- Patient profile loads within platform performance requirements.
- ---

## FR-PAT-004

### Title

Update Patient Information

### Business Value

Allow authorized users to maintain accurate and up-to-date patient demographic and administrative information while preserving data integrity, ensuring regulatory compliance, and maintaining a complete audit history.

### Primary Actors

- Receptionist
- Registration Officer
- Medical Records Staff
- Authorized Administrative Staff

### Preconditions

- User is authenticated.
- User has permission to update patient information.
- Patient record exists.

### Trigger

The user selects **Edit Patient Information** from the Patient Profile.

### Description

The system shall allow authorized users to update patient demographic and administrative information.

Editable information may include:

- Full Name
- Contact Information
- Address
- Emergency Contact
- Marital Status
- Nationality
- Preferred Language
- Insurance Information
- Administrative Notes

The system shall validate all modified information according to organizational rules before saving.

Certain fields, including the internal Patient ID, shall never be editable.

Changes to sensitive information may require additional authorization according to organizational policy.

### Expected Result

- Patient information is updated successfully.
- Data validation rules are enforced.
- Sensitive updates follow organizational approval policies.
- All modifications are recorded in the audit trail.

### Related Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-002 Patient Identity Validation
- BR-PAT-003 Medical Record Number (MRN) Management
- BR-PAT-009 Audit Trail and Record Traceability

### Related Modules

- Reception
- Appointments
- EMR
- Billing
- Audit

### Priority

Critical

### Acceptance Criteria

- Authorized users can update permitted fields.
- Restricted fields cannot be modified.
- Validation prevents invalid data.
- All updates are recorded in the audit trail.
- Updated information is immediately available throughout the platform.
  
- New Value (where applicable)
- Source Module
- Branch
- Reason for Change (when required)

Audit records shall be immutable and accessible only to authorized users according to organizational security policies.
---

## FR-PAT-005

### Title

Manage Patient Contact Information

### Business Value

Enable authorized users to maintain accurate and up-to-date patient contact information to support effective communication, appointment coordination, emergency notifications, and continuity of care.

### Primary Actors

- Receptionist
- Registration Officer
- Medical Records Staff
- Authorized Administrative Staff

### Preconditions

- User is authenticated.
- User has permission to update patient contact information.
- Patient record exists.

### Trigger

The user selects **Edit Contact Information** from the Patient Profile.

### Description

The system shall allow authorized users to manage patient contact information independently from other demographic information.

The contact information shall include:

- Primary Mobile Number
- Secondary Mobile Number (optional)
- Home Telephone (optional)
- Email Address
- Residential Address
- City
- State / Governorate
- Postal Code (optional)
- Country

The system shall also support maintaining one or more emergency contacts, including:

- Contact Name
- Relationship
- Mobile Number
- Alternative Number (optional)

The system shall validate contact information according to organizational rules before saving.

### Expected Result

- Patient contact information is updated successfully.
- Emergency contact information is maintained.
- Changes are immediately available to authorized modules.
- All modifications are recorded in the audit trail.

### Related Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-009 Audit Trail and Record Traceability

### Related Modules

- Reception
- Appointments
- Notifications
- EMR
- Audit

### Priority

High

### Acceptance Criteria

- Contact information is validated before saving.
- Emergency contacts can be created and updated.
- Changes are reflected across integrated modules.
- All updates are recorded in the audit trail.
- ---

## FR-PAT-006

### Title

Manage Patient Insurance Information

### Business Value

Enable authorized users to record, maintain, and verify patient insurance information, supporting accurate billing, eligibility validation, and healthcare reimbursement processes.

### Primary Actors

- Receptionist
- Insurance Coordinator
- Billing Staff
- Authorized Administrative Staff

### Preconditions

- User is authenticated.
- User has permission to manage insurance information.
- Patient record exists.

### Trigger

The user selects **Manage Insurance Information** from the Patient Profile.

### Description

The system shall allow authorized users to create, update, and maintain one or more insurance records associated with a patient.

Insurance information may include:

- Insurance Provider
- Insurance Plan
- Membership Number
- Policy Number
- Coverage Start Date
- Coverage Expiry Date
- Class / Category
- Primary or Secondary Insurance
- Eligibility Status
- Notes

The system shall support multiple insurance records while allowing one record to be designated as the primary insurance.

Expired insurance records shall remain available for historical reference.

### Expected Result

- Insurance information is stored successfully.
- Primary insurance is clearly identified.
- Historical insurance records are preserved.
- Insurance information is available to authorized billing and clinical modules.

### Related Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-009 Audit Trail and Record Traceability

### Related Modules

- Billing
- Reception
- Appointments
- EMR
- Audit

### Priority

High

### Acceptance Criteria

- Multiple insurance records are supported.
- Only one insurance record can be marked as primary.
- Expired insurance remains available for historical purposes.
- All changes are recorded in the audit trail.
- ---

## FR-PAT-007

### Title

Manage Patient Documents

### Business Value

Enable authorized users to securely upload, organize, and manage patient-related documents while maintaining regulatory compliance, improving operational efficiency, and supporting clinical and administrative workflows.

### Primary Actors

- Receptionist
- Medical Records Staff
- Insurance Coordinator
- Authorized Administrative Staff

### Preconditions

- User is authenticated.
- User has permission to manage patient documents.
- Patient record exists.

### Trigger

The user selects **Manage Documents** from the Patient Profile.

### Description

The system shall allow authorized users to upload, view, download, replace, and manage patient-related documents.

Supported document categories may include:

- National ID / Passport
- Insurance Card
- Consent Forms
- Referral Letters
- External Medical Reports
- External Laboratory Reports
- External Radiology Reports
- Administrative Documents
- Other Attachments

For each document, the system shall maintain:

- Document Name
- Document Category
- Upload Date
- Uploaded By
- File Type
- File Size
- Version (where applicable)
- Status

Documents shall remain linked to the patient record and be accessible according to role-based permissions.

### Expected Result

- Documents are uploaded successfully.
- Documents are categorized correctly.
- Authorized users can securely access patient documents.
- All document activities are recorded in the audit trail.

### Related Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-009 Audit Trail and Record Traceability

### Related Modules

- EMR
- Billing
- Insurance
- Reception
- Audit

### Priority

High

### Acceptance Criteria

- Documents upload successfully.
- Supported file types are validated.
- Documents are categorized correctly.
- Unauthorized users cannot access restricted documents.
- All document activities are recorded in the audit trail.
- ---

## FR-PAT-008

### Title

Manage Patient Photograph

### Business Value

Enable authorized users to capture, upload, update, and maintain a patient photograph to improve patient identification, reduce identification errors, and support safe clinical and administrative workflows.

### Primary Actors

- Receptionist
- Registration Officer
- Medical Records Staff

### Preconditions

- User is authenticated.
- User has permission to manage patient photographs.
- Patient record exists.

### Trigger

The user selects **Manage Patient Photograph** from the Patient Profile.

### Description

The system shall allow authorized users to:

- Capture a photograph using a connected camera.
- Upload an existing photograph.
- Replace an existing photograph.
- Remove a photograph according to organizational policy.

The system shall validate supported image formats and enforce configurable size limitations.

The patient photograph shall be displayed consistently across authorized modules to assist in patient identification.

### Expected Result

- Patient photograph is stored successfully.
- Updated photograph becomes immediately available throughout the platform.
- Previous photograph handling follows organizational retention policies.
- All photograph management activities are recorded in the audit trail.

### Related Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-009 Audit Trail and Record Traceability

### Related Modules

- Reception
- Appointments
- EMR
- Billing
- Pharmacy
- Laboratory
- Radiology
- Audit

### Priority

Medium

### Acceptance Criteria

- Supported image formats are accepted.
- Image size validation is enforced.
- Photograph displays correctly across authorized modules.
- Unauthorized users cannot modify patient photographs.
- All photograph changes are recorded in the audit trail.
- ---

## FR-PAT-009

### Title

Merge Patient Records

### Business Value

Enable authorized users to safely merge duplicate patient records into a single authoritative record while preserving historical information, maintaining data integrity, and ensuring complete auditability.

### Primary Actors

- Medical Records Staff
- Health Information Management (HIM)
- System Administrator

### Preconditions

- User is authenticated.
- User has permission to merge patient records.
- Two or more patient records have been identified as potential duplicates.
- The selected records are eligible for merging according to organizational policy.

### Trigger

The user initiates the **Merge Patient Records** process after reviewing duplicate records.

### Description

The system shall allow authorized users to merge duplicate patient records into a single primary patient record.

During the merge process, the system shall:

- Designate one patient record as the Primary Record.
- Mark the remaining records as Merged Records.
- Reassign all related clinical, financial, and operational data to the Primary Record.
- Preserve historical references to the original records.
- Prevent loss of clinical or administrative information.
- Record the reason for the merge.
- Record the user performing the merge.
- Record the date and time of the merge.

The system shall not permanently delete merged records.

### Expected Result

- Duplicate records are successfully merged.
- All related data references point to the Primary Record.
- Historical traceability is preserved.
- The merge operation is fully audited.

### Related Business Rules

- BR-PAT-004 Duplicate Patient Detection
- BR-PAT-005 Patient Record Merge
- BR-PAT-009 Audit Trail and Record Traceability

### Related Modules

- Reception
- Appointments
- EMR
- Billing
- Laboratory
- Radiology
- Pharmacy
- Audit

### Priority

Critical

### Acceptance Criteria

- Authorized users can merge duplicate records.
- One Primary Record is designated.
- No clinical, financial, or administrative data is lost.
- Historical references remain available.
- The merge operation is fully recorded in the audit trail.
- ---

## FR-PAT-010

### Title

Archive or Deactivate Patient Record

### Business Value

Enable authorized users to deactivate or archive patient records when appropriate while preserving historical information, ensuring regulatory compliance, and preventing unauthorized reuse of patient identities.

### Primary Actors

- Medical Records Staff
- Health Information Management (HIM)
- System Administrator

### Preconditions

- User is authenticated.
- User has permission to manage patient status.
- Patient record exists.

### Trigger

The user selects **Archive Patient** or **Deactivate Patient** from the Patient Profile.

### Description

The system shall allow authorized users to change the operational status of a patient record.

Supported statuses may include:

- Active
- Inactive
- Archived
- Deceased

When changing the patient status, the system shall:

- Require a reason for the status change.
- Record the effective date.
- Record the user performing the action.
- Prevent unauthorized status changes.
- Preserve all historical clinical, financial, and administrative data.

Archived or inactive patient records shall remain searchable by authorized users for historical and legal purposes.

The system shall not permanently delete patient records through this function.

### Expected Result

- Patient status is updated successfully.
- Historical information is preserved.
- The patient record remains available according to access permissions.
- The status change is fully recorded in the audit trail.

### Related Business Rules

- BR-PAT-006 Patient Status Management
- BR-PAT-009 Audit Trail and Record Traceability

### Related Modules

- Reception
- Appointments
- EMR
- Billing
- Laboratory
- Radiology
- Pharmacy
- Audit

### Priority

High

### Acceptance Criteria

- Authorized users can change patient status.
- A reason for the status change is required.
- Archived records remain accessible to authorized users.
- No patient data is permanently deleted.
- All status changes are recorded in the audit trail.
- ---

# Module Completion Summary

## Functional Requirements

- FR-PAT-001 Register New Patient
- FR-PAT-002 Search Patient
- FR-PAT-003 View Patient Profile
- FR-PAT-004 Update Patient Information
- FR-PAT-005 Manage Patient Contact Information
- FR-PAT-006 Manage Patient Insurance Information
- FR-PAT-007 Manage Patient Documents
- FR-PAT-008 Manage Patient Photograph
- FR-PAT-009 Merge Patient Records
- FR-PAT-010 Archive or Deactivate Patient Record

## Business Rules

- BR-PAT-001 Patient Registration
- BR-PAT-002 Patient Identity Validation
- BR-PAT-003 Medical Record Number (MRN) Management
- BR-PAT-004 Duplicate Patient Detection
- BR-PAT-005 Patient Record Merge
- BR-PAT-006 Patient Status Management
- BR-PAT-007 Patient Alerts and Administrative Flags
- BR-PAT-008 Multi-Branch Patient Access
- BR-PAT-009 Audit Trail and Record Traceability
- 




The module establishes a standardized patient registration and management process that improves operational efficiency, enhances continuity of care, and provides a trusted foundation for all patient-related workflows across the LOUTAS Care platform.
