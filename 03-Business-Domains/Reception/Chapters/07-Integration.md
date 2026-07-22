# Reception Integration

**Document ID:** REC-007

**Module:** Reception

**Status:** Draft

**Version:** 1.0

**Last Updated:** 2026-07-22

---

# Integration

The Reception Business Domain operates as the operational gateway to the LOUTAS Care platform and integrates with multiple business domains to ensure a continuous and coordinated Patient Journey.

---

# Integration with Patient Journey

Reception is responsible for initiating the Patient Journey and transferring operational ownership according to approved business workflows.

---

# Integration with Patient Management

Reception uses Patient Management services to:

* Search patients.
* Register new patients.
* Update demographic information.
* Verify patient identity.

Reception does not own the patient's master demographic record.

---

# Integration with Appointment

Reception manages appointment-related operations including:

* Appointment creation.
* Confirmation.
* Rescheduling.
* Cancellation.
* Walk-in registration.

Appointment scheduling policies are governed by the Appointment Business Domain.

---

# Integration with Doctor Workspace

After successful check-in, Reception transfers operational ownership to the assigned physician or authorized clinical department.

---

# Integration with EMR

Reception provides administrative information required before clinical documentation begins.

Clinical documentation is performed exclusively within the EMR Business Domain.

---

# Integration with Clinical Orders

Reception does not create clinical orders.

Clinical orders originate from authorized clinical workspaces such as the EMR.

Reception may display operational status of orders when required.

---

# Integration with Billing

Reception initiates administrative workflows required before billing activities.

Billing calculations, pricing, discounts, taxation, and payment processing are managed exclusively by the Billing Business Domain.

---

# Integration with Dashboard

Reception contributes operational information to the Dashboard including:

* Waiting patients.
* Check-in status.
* Appointment status.
* Operational workload.
* Queue information.

---

# Integration Principles

* Reception shall integrate through approved business workflows.
* Reception shall not duplicate responsibilities owned by other Business Domains.
* Reception shall remain fully aligned with the Patient Journey architecture.
* All integrations shall preserve operational traceability and auditability.

