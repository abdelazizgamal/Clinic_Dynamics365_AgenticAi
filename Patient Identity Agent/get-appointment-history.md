---
name: get-appointment-history
description: |-
  Retrieves and presents the 
  confirmed patient's appointment records 
  from the Appointment table, grouped by 
  status and ordered by date.
---
## Purpose
Give the patient a clear view of their 
upcoming and past appointments when requested.

## When Activated
When an identified patient asks to see 
their appointments, upcoming visits, 
past bookings, or appointment history.
Never runs before patient identity is 
confirmed in this session.

## What the Agent Does

Query the Appointment table for all 
records linked to the confirmed patient.

If no records are found:
"You don't have any appointments on record 
yet. Would you like to book one?"
Offer to hand back to the Orchestrator 
for booking.

If records are found:
Group and present them clearly:

Show upcoming appointments first — 
those with status Scheduled.
Then show past appointments — 
those with status Completed, 
Cancelled, Rescheduled, or No Show.

For each appointment show:
Doctor name and specialty if available.
Appointment date and time.
Status in plain patient-friendly language.

Translate status values as follows:
Scheduled → Upcoming
Completed → Visited
Cancelled → Cancelled
Rescheduled → Rescheduled
No Show → Missed

After displaying, ask if the patient 
wants to take any action such as 
cancelling or rescheduling an appointment.
If yes — hand back to Orchestrator.

## Edge Cases

Patient has only cancelled appointments:
Show them clearly under past appointments.
Offer to help book a new one.

Patient has a large number of appointments:
Show the 5 most recent upcoming appointments 
and the 5 most recent past appointments.
Tell the patient the results are limited 
and offer to filter by a specific period 
if needed.

Patient asks about a specific appointment:
Show only that appointment's details.
If it cannot be uniquely identified — 
ask for the date or reference number 
to narrow it down.

Patient asks if a specific appointment 
is confirmed:
Find the matching appointment and report 
its current status clearly.

## What the Agent Never Does
Never Generate unknown Data or data not from the Dataverse
Never show appointments belonging to 
another patient.
Never expose internal record IDs 
or field names.
Never allow the patient to modify 
an appointment from this skill — 
always hand to Orchestrator for that.

## Examples

Example 1 — Has upcoming appointment
Patient: Can I see my appointments?
Agent: Of course. Here are your appointments.

Upcoming:
Dr. Ahmed Khalil — Cardiology
Monday, 30 June 2026 at 10:00 AM — Upcoming

Past:
Dr. Sara Nour — Dermatology
Tuesday, 3 June 2026 at 2:00 PM — Visited

Would you like to cancel or reschedule 
any of these?

Example 2 — No appointments
Patient: Show me my appointments.
Agent: You don't have any appointments 
on record yet. Would you like me to 
help you book one?

Example 3 — Only past appointments
Patient: What appointments have I had?
Agent: Here are your past appointments.

Dr. Ahmed Khalil — Cardiology
Monday, 2 June 2026 at 10:00 AM — Visited

Dr. Sara Nour — Dermatology
Thursday, 5 June 2026 at 3:00 PM — Cancelled

Would you like to book a new appointment?