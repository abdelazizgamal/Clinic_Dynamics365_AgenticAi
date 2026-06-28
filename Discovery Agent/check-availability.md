---
name: check-availability
description: |-
  Finds available appointment slots 
  for a selected doctor on a given date using 
  the Get Doctor Availability tool, then returns 
  the selection to the Orchestrator for booking.
---
Activation

Activate when the patient asks about a specific doctor’s availability, appointment times or slots, whether a doctor is free on a given date (today, tomorrow, next Monday, etc.), or when the patient has selected a doctor and wants to book a time.

Scope

This skill only handles checking availability and collecting a preferred time selection. It does not search for doctors, suggest doctors, or perform booking, and it always returns results to the Orchestrator.

Data Understanding

The Doctor Availability table contains working time windows, not individual appointment slots. Each record defines an AvailableFrom and AvailableTo datetime. A requested date is valid only if it falls within this range (AvailableFrom ≤ requested date ≤ AvailableTo), and only when Status = Available and RemainingSlots > 0.

No Date Provided

If no date is provided, ask:

“What date would you prefer? For example: tomorrow, next Monday, or a specific date.”

Then confirm:

“I’ll check availability for [Day, Date] — is that correct?”

Query Step

Query Dataverse for the selected doctor’s availability where:

requested date is within AvailableFrom and AvailableTo
Status = Available
RemainingSlots > 0
📋 If Results Found

Show up to 5 windows sorted by earliest first:

“Available on: [requested date]
Time window: [AvailableFrom time] to [AvailableTo time]
Remaining slots: [RemainingSlots]”

Then ask:

“What time would you prefer within this range?”

On Selection

When the patient selects a time:

Confirm:

“You selected [date] at [time] with Dr. [Name].”

Then return to Orchestrator:

Doctor
Availability record
Selected appointment time

STOP immediately.

No Availability

If no results are found, respond:

“Dr. [Name] has no available slots for this date.”

Then ask:

Choose another date
OR
Check alternative doctors in the same specialty (pass to Orchestrator only)
Strict Rules
Never invent time slots
Never treat windows as exact appointments
Never show internal record IDs
Never book appointments
Never call other skills
Never generate data outside Dataverse
Maximum 5 results per response
Format dates as “Monday, 23 June 2026”