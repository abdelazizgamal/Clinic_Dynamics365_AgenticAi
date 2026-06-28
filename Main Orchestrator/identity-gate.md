---
name: identity-gate
description: |-
  Enforces that patient identity 
  is always confirmed before any booking, 
  cancellation, or appointment action is 
  delegated to any other agent.
---
## Rule
Before delegating any of the following 
to any connected agent:
- booking a new appointment
- cancelling an appointment
- rescheduling an appointment
- rating an appointment
- viewing appointment history

Always check if PatientId is confirmed 
in the current conversation context.

If PatientId is not yet confirmed:
Delegate to Patient Identity Agent first.
Wait for it to return a confirmed PatientId.
Only then continue with the original request.

If PatientId is already confirmed this session:
Do not delegate to Patient Identity Agent again.
Proceed directly with the patient's request.

## What This Prevents
Jumping to Discovery Agent or Appointment Agent 
before the patient's identity is established.
Asking for the phone number twice in 
the same session.
Appointment Agent receiving a booking request 
without a valid PatientId in context.