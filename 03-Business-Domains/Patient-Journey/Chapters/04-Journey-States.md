# Patient Journey States

**Document ID:** PJ-004

**Module:** Patient Journey

**Status:** Draft

**Version:** 1.0

**Last Updated:** 2026-07-22

---

# Journey States

The Patient Journey progresses through a series of standardized operational states.

These states provide a unified representation of the patient's current position within the healthcare process, regardless of the department performing the work.

## Standard Journey States

| State       | Description                                                                           |
| ----------- | ------------------------------------------------------------------------------------- |
| Created     | The Patient Journey has been created.                                                 |
| Scheduled   | An appointment has been scheduled.                                                    |
| Checked-In  | The patient has arrived and completed check-in.                                       |
| In Progress | Clinical or operational activities are currently being performed.                     |
| Waiting     | The patient is waiting for the next authorized activity.                              |
| On Hold     | The journey has been temporarily paused for a valid operational reason.               |
| Completed   | All required clinical and administrative activities have been completed successfully. |
| Cancelled   | The journey has been cancelled before completion.                                     |

## State Transition Principles

* Every journey has exactly one current state.
* State transitions shall follow approved business workflows.
* Unauthorized state changes are not permitted.
* All state transitions shall be recorded in the audit history.
* Department-specific statuses shall not replace the official Journey State.

Department-specific workflows may maintain additional internal statuses where required, provided they do not conflict with the standardized Patient Journey States.

