---
name: smart-recommendation
description: |-
  Finds and presents up to 3 
  alternative active doctors in the same 
  specialty with available slots when the 
  originally requested doctor or date has 
  no availability. Ranked by soonest slot, 
  then lowest fee, then most experience.
---
## Purpose
Prevent the patient from reaching a dead end 
when no slot is available by proactively 
offering the best alternatives from the 
same specialty.

## When Activated
Automatically when:
A slot search returns no available windows 
for the requested doctor or date.
A booking attempt fails because the slot 
became unavailable.
A reschedule attempt finds no suitable window.

## What the Agent Does

Identify the specialty from the current 
conversation context.

Query the Doctor table joined with the 
Availability table for other active doctors 
in the same specialty where at least one 
availability window exists with status 
Available and remaining slots greater 
than zero.

Rank the results by:
First — soonest AvailableFrom date.
Then — lowest ConsultationFees.
Then — most YearsOfExperience.

Present up to 3 alternatives in this format 
for each:
Doctor name and title.
Consultation fee.
Next available window: date range 
and working hours.
Slots remaining.

Ask the patient to choose one or decline.

If the patient selects an alternative:
Store the new doctor and availability 
in conversation context.
Return to book-appointment skill 
and proceed normally.

If the patient declines all options:
"I wasn't able to find a suitable option 
for you. Would you like me to connect 
you with our clinic team for assistance?"

On yes — create an Escalation Request record:
Patient: confirmed PatientId.
Reason: Patient could not find a suitable 
appointment slot in [Specialty]. 
Smart recommendation exhausted.
Status: New.

Confirm to patient:
"Your request has been logged and 
our clinic team will follow up with you."

On no — close gracefully:
"No problem at all. Please come back 
whenever you're ready."

## Edge Cases

No alternative doctors exist in the specialty:
"There are currently no available doctors 
in [Specialty]. Would you like me to 
log a request for our clinic team?"

All alternative windows are in the far future:
Show them with the actual dates — do not hide.
Let the patient decide if they are acceptable.

Patient wants a specific time that no 
alternative can offer:
Show what is available.
Let the patient decide.
Do not fabricate times that do not exist.

## What the Agent Never Does
Never recommend inactive doctors.
Never recommend windows where status 
is not Available.
Never recommend windows with zero 
remaining slots.
Never fabricate availability or fees.
Never show more than 3 alternatives 
at once.

## Examples

Example 1 — Alternatives available
Agent: Dr. Ahmed Khalil has no available 
slots on your requested date.
Here are the next best options in Cardiology.

Dr. Mohamed Fathy — Professor
Fee: 800 EGP
Available: 1 July – 31 July, 08:00–11:00 AM
6 slots remaining

Dr. Ahmed Khalil — Consultant
Fee: 500 EGP
Available from: 5 July, 14:00–17:00 PM
8 slots remaining

Would you like to book with one of these doctors?

Example 2 — No alternatives at all
Agent: There are currently no available 
doctors in Cardiology.
Would you like me to log a request for 
our clinic team to contact you?

Example 3 — Patient declines all
Patient: None of these work for me.
Agent: I understand. Would you like me 
to connect you with our clinic team 
who can help find a suitable time?