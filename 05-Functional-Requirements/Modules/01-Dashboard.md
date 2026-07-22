# Dashboard

---

## Document Information

| Item         | Value                   |
| ------------ | ----------------------- |
| Document ID  | FR-DASH-001             |
| Module       | Dashboard               |
| Section      | Functional Requirements |
| Version      | 1.0                     |
| Status       | Draft                   |
| Owner        | Product Management      |
| Last Updated | 2026-07-22              |

---

# Business Objective

The Dashboard is the primary entry point to the LOUTAS Care platform.

Its objective is to provide every authenticated user with a personalized operational overview, enabling rapid situational awareness, efficient navigation, and immediate access to high-priority tasks.

The Dashboard shall reduce navigation time and support informed decision-making throughout the working day.

---

# Module Vision

The Dashboard is not merely a home page.

It is the operational command center of the platform.

Every user should immediately understand:

* What requires attention.
* What work is pending.
* What actions are available.
* What important events occurred.
* What should be done next.

The Dashboard must adapt dynamically according to the authenticated user's role and responsibilities.

---

# Primary Users

The Dashboard supports:

* Physicians
* Receptionists
* Nurses
* Cashiers
* Laboratory Staff
* Radiology Staff
* Pharmacists
* Branch Managers
* Clinic Owners
* System Administrators

Each role receives a personalized Dashboard experience.

---

# Dashboard Philosophy

The Dashboard follows four core principles:

* Show only relevant information.
* Prioritize actionable items.
* Reduce unnecessary navigation.
* Deliver information in real time whenever possible.

The Dashboard is designed to help users start work immediately after login.

---

# Related Documents

* LC-PS-003 User Roles
* LC-PS-004 System Modules
* Workspace Architecture (Future)
* Dashboard Architecture (Future)

---

# Notes

Detailed dashboard widgets, business rules, KPIs, and workflows are defined in the following sections of this module.
# Dashboard Widgets

The Dashboard shall display widgets dynamically based on the authenticated user's role.

Widgets provide contextual information, actionable insights, and quick access to the user's daily responsibilities.

---

# Physician Dashboard

Widgets include:

* Today's Appointments
* Waiting Patients
* Current Patient Queue
* Recent Clinical Notes
* Pending Laboratory Results
* Pending Radiology Reports
* Prescription Alerts
* Follow-up Patients
* Clinical Notifications
* Quick Access to Doctor Workspace

---

# Reception Dashboard

Widgets include:

* Today's Schedule
* Waiting Patients
* Check-in Queue
* New Patient Registrations
* Appointment Requests
* Available Time Slots
* No-show Patients
* Quick Patient Search
* Quick Registration
* Reception Workspace Shortcut

---

# Nurse Dashboard

Widgets include:

* Patients Awaiting Vitals
* Active Examination Rooms
* Physician Requests
* Pending Nursing Tasks
* Medication Administration Alerts
* Nursing Workspace Shortcut

---

# Cashier Dashboard

Widgets include:

* Draft Invoices
* Outstanding Payments
* Today's Revenue
* Recent Transactions
* Pending Refund Requests
* Collection Summary
* Cashier Workspace Shortcut

---

# Laboratory Dashboard

Widgets include:

* Pending Orders
* Samples Awaiting Processing
* Completed Results Pending Approval
* Critical Results
* Laboratory Workspace Shortcut

---

# Radiology Dashboard

Widgets include:

* Pending Imaging Requests
* Scheduled Studies
* Reports Awaiting Completion
* Completed Studies
* Radiology Workspace Shortcut

---

# Pharmacy Dashboard

Widgets include:

* Pending Prescriptions
* Low Stock Alerts
* Expiring Medications
* Dispensing Queue
* Pharmacy Workspace Shortcut

---

# Branch Manager Dashboard

Widgets include:

* Daily Operations Summary
* Appointment Utilization
* Staff Performance
* Revenue Summary
* Operational Alerts
* Branch KPIs
* Manager Workspace Shortcut

---

# Executive Dashboard

Widgets include:

* Revenue Overview
* Branch Comparison
* Growth Indicators
* Financial KPIs
* Patient Volume Trends
* Strategic Alerts
* Executive Workspace Shortcut

---

# Widget Principles

Every widget shall:

* Display accurate real-time information whenever possible.
* Support direct navigation to the related Workspace.
* Respect user permissions.
* Present only relevant information.
* Minimize unnecessary visual clutter.
* Support responsive layouts across devices.


