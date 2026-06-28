---
name: reschedule-appointment
description: |-
  Moves a Scheduled appointment 
  to a new availability window. Marks the 
  original as Rescheduled, creates a new 
  Scheduled appointment, and writes history 
  entries for both. Handles availability 
  window logic correctly.
---
## Purpose
Allow a patient to move an existing 
scheduled appointment to a new time 
or doctor while maintaining a complete 
audit trail.

## When Activated
When a patient requests to move, 
change the time of, or reschedule 
an existing appointment.

## What the Agent Does

Query the Appointment table for this 
patient's appointments where status 
is Scheduled.

If none found:
"You don't have any upcoming appointments 
to reschedule."
Ask if they would like to book a new one.

If one appointment exists:
Confirm it is the one they want to reschedule.

If multiple exist:
List them and ask which to reschedule.

Ask for the new preference:
New date, same doctor — or
Different doctor or specialty.

If same doctor, new date:
Query the Availability table for that doctor 
where the new requested date falls within 
a window, status is Available, and 
remaining slots is greater than zero.

If different doctor or specialty:
Return to Orchestrator to delegate 
to Discovery Agent for new selection.
Pass the context that this is a reschedule 
so the flow continues correctly.

If no availability found for new preference:
Activate smart-recommendation skill.

If availability is found:
Show the available windows and times.
Patient selects a new time within the window.

Present the full reschedule summary:
"You are moving your appointment from 
[Old Date] at [Old Time] with Dr. [Old Name]
to [New Date] at [New Time] with Dr. [New Name].
Shall I confirm this change?"

Wait for explicit yes.

On confirmed yes:
Update the original Appointment status 
to Rescheduled.
Release the slot on the original 
availability window — increment 
remaining slots by one.

Create a new Appointment record:
Appointment Name: patientName - DoctorName - VisitCause like(Nour Ali - Dr. Karim Salah - General Health Checkup)
Patient: same PatientId.
Availability: the new window.
Appointment time: the new chosen time.
Status: Scheduled.
Notes: Rescheduled from reference [OldRef].

Decrement remaining slots on the 
new availability window by one.

Write Appointment History for the 
original appointment:
Action type: Rescheduled
Old status: Scheduled
New status: Rescheduled
Action date: now
Comments: Patient requested reschedule 
to [New Date] at [New Time].

Write Appointment History for the 
new appointment:
Action type: Created
Old status: empty
New status: Scheduled
Action date: now
Comments: Created as reschedule from 
appointment reference [OldRef].

Confirm to patient:
"Your appointment has been rescheduled 
to [New Date] at [New Time] with 
Dr. [Name]. Your new reference 
number is [NewRef]."

## Edge Cases

Patient wants to reschedule to a date 
in the past:
Tell them the date has already passed.
Ask for a future date.

Patient wants to reschedule a Completed 
or Cancelled appointment:
"That appointment cannot be rescheduled 
as it has already been [completed/cancelled]."
Offer to book a new appointment instead.

Patient wants to reschedule to the 
same date and time:
Confirm they want no actual change.
If confirmed — do nothing and tell them 
their appointment is unchanged.

Patient changes mind after seeing new options:
Allow them to go back and choose differently.
Do not create any records until final 
explicit confirmation.

New slot becomes unavailable between 
showing options and confirming:
Always re-verify before creating the 
new appointment.
If taken — show updated options 
without making the patient start over.

## What the Agent Never Does
Never reschedule without explicit confirmation.
Never skip releasing the original slot.
Never skip writing history for both 
the original and new appointment.
Never reschedule a Completed, Cancelled, 
or already Rescheduled appointment.
Never apply the window range logic incorrectly — 
the new date must fall within the window, 
not match AvailableFrom exactly.

## Examples

Example 1 — Same doctor, new date
Patient: I want to reschedule my appointment 
with Dr. Ahmed.
Agent: I found your appointment.

Dr. Ahmed Khalil — Cardiology
Monday, 30 June 2026 at 10:00 AM

What new date would you prefer?
Patient: Next Wednesday.
Agent: Dr. Ahmed Khalil has availability 
on Wednesday, 2 July 2026 from 
09:00 AM to 12:00 PM with 9 slots remaining.
What time would you prefer?
Patient: 10:30 AM.
Agent: You are moving your appointment from 
Monday, 30 June at 10:00 AM 
to Wednesday, 2 July at 10:30 AM 
with Dr. Ahmed Khalil. Shall I confirm?
Patient: Yes.
Agent: Your appointment has been rescheduled 
to Wednesday, 2 July 2026 at 10:30 AM.
Your new reference number is APT-20260702-007.

Example 2 — Different doctor requested
Patient: Reschedule to a different cardiologist.
Agent: Let me find other cardiologists 
available for you.
[Returns to Orchestrator → Discovery Agent]

Example 3 — No availability for new date
Patient: Move it to next Friday.
Agent: Dr. Ahmed Khalil has no available 
slots on Friday, 3 July 2026.
Let me check if there are other cardiologists 
available for you.
[Activates smart-recommendation]