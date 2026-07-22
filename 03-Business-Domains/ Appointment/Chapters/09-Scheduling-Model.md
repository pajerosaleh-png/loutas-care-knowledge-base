# Enterprise Scheduling Model

**Document ID:** APT-009

**Module:** Appointment

**Status:** Draft

**Version:** 1.0

**Last Updated:** 2026-07-22

---

# Purpose

The Enterprise Scheduling Model defines the scheduling architecture used across the LOUTAS Care platform.

The scheduling engine is designed to support multiple healthcare specialties, operational models, and healthcare resources through a single standardized scheduling architecture.

The model is independent of medical specialty and is designed for long-term scalability.

---

# Design Philosophy

LOUTAS Care adopts a Resource-Based Scheduling Architecture.

Appointments are scheduled against healthcare resources rather than individual healthcare providers alone.

Healthcare resources may include people, rooms, equipment, devices, operational locations, or any other schedulable healthcare asset.

This design enables the scheduling engine to support outpatient clinics, dental centers, physiotherapy centers, radiology centers, endoscopy units, diagnostic centers, and future healthcare services without architectural redesign.

---

# Scheduling Resources

Schedulable resources may include, but are not limited to:

* Healthcare Providers
* Treatment Rooms
* Consultation Rooms
* Procedure Rooms
* Dental Chairs
* Imaging Equipment
* Physiotherapy Equipment
* Medical Devices
* Branches
* Operational Units
* Other configurable healthcare resources

Every schedulable resource shall maintain its own availability calendar.

---

# Resource Requirements

Healthcare services define the resources required for execution.

Examples include:

* Consultation → Provider + Consultation Room
* Physiotherapy Session → Therapist + Treatment Room + Equipment
* MRI Examination → MRI Device + Technician + Imaging Room
* Dental Procedure → Dentist + Dental Chair
* Endoscopy Procedure → Physician + Endoscopy Room + Equipment + Nursing Team

The scheduling engine allocates appointments according to required resources.

---

# Appointment Models

The scheduling engine shall support multiple appointment models, including:

* Single Appointment
* Walk-In Appointment
* Follow-Up Appointment
* Recurring Appointment
* Treatment Course
* Group Appointment
* Telemedicine Appointment
* Home Visit
* Future configurable appointment models

---

# Schedule Templates

The scheduling engine shall support configurable scheduling templates including:

* Working Days
* Working Hours
* Multiple Daily Sessions
* Variable Time Slots
* Specialty Templates
* Branch Templates

Templates may differ between providers, resources, specialties, and organizational policies.

---

# Availability Management

The scheduling engine shall manage resource availability including:

* Annual Leave
* Sick Leave
* Public Holidays
* Temporary Closure
* Branch Closure
* Maintenance Windows
* Emergency Resource Blocking

---

# Time Management

Scheduling shall support configurable:

* Appointment Duration
* Buffer Before Appointment
* Buffer After Appointment
* Cleanup Time
* Preparation Time
* Recovery Time

Time calculations shall be configurable according to organizational policies.

---

# Scheduling Policies

The scheduling engine shall support configurable policies including:

* Double Booking
* Waiting Lists
* Booking Horizon
* Cancellation Rules
* Rescheduling Rules
* No Show Policies
* Priority Scheduling
* Emergency Scheduling

Organizational policies determine how each rule is applied.

---

# Future Scheduling Capabilities

The scheduling architecture shall support future capabilities including:

* Intelligent Scheduling
* AI-Based Slot Recommendation
* Automatic Wait List Management
* Online Patient Booking
* WhatsApp Appointment Requests
* Self-Service Scheduling
* Resource Optimization
* Predictive Capacity Planning

These capabilities shall be supported without requiring architectural redesign.

---

# Enterprise Principles

The scheduling engine shall:

* Remain independent from medical specialties.
* Schedule healthcare resources rather than individual providers only.
* Support configurable operational policies.
* Integrate with Patient Journey without creating clinical encounters.
* Support future healthcare services without redesign.
* Maintain complete operational traceability and auditability.

