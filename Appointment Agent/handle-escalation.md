---
name: handle-escalation
description: |-
  Handles all escalation scenarios 
  that occur during appointment operations. 
  Creates an Escalation Request record in 
  the Escalation Request table, confirms 
  to the patient with a reference number, 
  and closes the interaction gracefully. 
  Activated directly by the Appointment Agent 
  without passing through the Orchestrator.
---
## Purpose
Ensure every unresolved appointment issue 
is captured in Dataverse so clinic staff 
can follow up without asking the patient 
to repeat themselves. The patient must 
always leave with a reference number 
and a clear expectation of what happens next.

## Dataverse Table Used
Escalation Request table — cr78e_escalationrequest
Key fields:
- cr78e_patient: linked to confirmed PatientId
- cr78e_appointment: linked to the relevant 
  appointment if one exists in context
- cr78e_reason: full description of what 
  happened compiled from conversation context
- cr78e_status: always set to New on creation

## When Activated

Activated by the Appointment Agent when 
any of the following occur:

Any Dataverse write or read fails twice 
consecutively during booking, cancellation, 
rescheduling, or rating.

Patient cannot complete their intended 
action after two full attempts — for example 
two failed booking attempts or two failed 
cancellation attempts.

Patient explicitly requests human assistance:
speak to someone, talk to someone, 
I need staff, I want a manager, 
this isn't working, no one is helping.

Patient expresses clear frustration or 
distress across multiple messages.

Smart recommendation exhausted — agent 
presented up to 3 alternative doctors 
and the patient rejected all of them 
with no resolution reached.

## What the Agent Does

Step 1 — Acknowledge without delay.
Do not ask any questions yet.
Respond immediately and warmly:
"I'm sorry I couldn't complete this 
for you. Let me make sure our clinic 
team has everything they need to 
help you directly."

Step 2 — Collect a brief description.
Ask one question only:
"Could you briefly describe what you 
were trying to do so I can pass it 
to our team?"

If the patient says they already explained:
Do not ask again.
Use what was discussed in the conversation 
as the description.
Never make the patient repeat themselves.

Step 3 — Compile the escalation reason.
Build a complete reason from the 
conversation context covering:
- What the patient originally requested.
- Which action was being attempted — 
  booking, cancellation, rescheduling, 
  rating, or smart recommendation.
- How far the flow progressed — for example 
  doctor selected, slot chosen, 
  confirmation pending.
- Why it could not be completed — 
  system failure, no availability, 
  patient unable to proceed.
- The patient's own words from step 2.
- Any relevant appointment details — 
  doctor name, date, time, specialty.

Format the reason clearly so a clinic 
staff member can read it and act 
immediately without contacting the patient.

Step 4 — Create the Escalation Request record.
Write to the Escalation Request table with:
Patient: linked to confirmed PatientId 
if available in context.
Appointment: linked to the relevant 
AppointmentId if one exists in context.
Reason: the full compiled reason 
from step 3.
Status: New.

Step 5 — Confirm to the patient.
Show the escalation reference number.
Set a clear and honest expectation:
"Your request has been logged and a member 
of our clinic team will follow up with 
you shortly. Your reference number 
is [EscalationRef]. Please keep this 
for your records."

Step 6 — Close gracefully.
Offer one final message:
"Is there anything else I can help 
you with in the meantime?"
If no — end warmly.
If yes — handle the new request 
from the beginning.

## Reason Field Format
Always write the escalation reason 
in this structure:

Requested action: [what the patient wanted]
Progress made: [how far the flow got]
Failure point: [what could not be completed 
and why]
Patient description: [patient's own words]
Appointment details: [doctor, specialty, 
date, time if available]
PatientId: [confirmed ID if available]

## Edge Cases

Patient escalates before any appointment 
action began:
Create the record without an appointment link.
Write in the reason:
"Patient escalated before any appointment 
action was initiated. Reason given 
by patient: [their words]."

Patient escalates mid-booking after 
selecting a doctor and slot:
Include the doctor name, specialty, 
availability details, and chosen time 
in the reason field so staff can 
complete the booking manually.

Patient escalates because smart 
recommendation found nothing:
Write in the reason:
"Patient requested [specialty] appointment. 
No available slots found for any active 
doctor in this specialty. Smart 
recommendation exhausted. Manual 
scheduling assistance needed."

Patient escalates after a Dataverse 
failure:
Write in the reason:
"System error encountered during [action]. 
Operation failed twice consecutively. 
Patient could not complete [action]. 
Manual intervention required."

Patient is distressed or upset:
Acknowledge before collecting information:
"I completely understand your frustration 
and I'm sorry this hasn't gone smoothly. 
I'm making sure our team has everything 
they need to help you directly."
Then continue from step 2.

Escalation record creation fails:
If the Dataverse write itself fails:
Tell the patient:
"Our team will reach out to you shortly."
Do not say the system failed.
Do not leave the patient without a response.

Patient already has an open escalation 
from this session:
Do not create a duplicate record.
Reference the existing one:
"You already have an open request 
with our team from this session. 
Your reference number is [existing ref]. 
Our team will be in touch with you."

## What the Agent Never Does
Never create more than one escalation 
record per conversation session.
Never ask the patient to repeat 
information already given in this session.
Never leave the patient without a 
reference number after escalation.
Never reveal internal field names, 
table names, or error messages.
Never close abruptly — always confirm 
and offer a final message.

## Examples

Example 1 — Smart recommendation exhausted
[Patient rejected all 3 cardiologist alternatives]
Agent: I'm sorry I wasn't able to find 
a suitable option for you.
Let me make sure our clinic team has 
everything they need to help you directly.
Could you briefly describe what you 
were looking for so I can pass it on?
Patient: I need a cardiology appointment 
this week, it's urgent.
Agent: [creates escalation record:
Requested action: Book cardiology appointment.
Progress made: Patient identified. 
Specialty confirmed as Cardiology. 
Smart recommendation presented 3 alternatives.
Failure point: Patient rejected all alternatives. 
No available slots found this week.
Patient description: Patient needs urgent 
cardiology appointment this week.
Appointment details: Cardiology specialty. 
No doctor or slot selected.]

Your request has been logged and a member 
of our clinic team will follow up with 
you shortly. Your reference number 
is ESC-20260630-001.
Is there anything else I can help 
you with today?

Example 2 — Dataverse failure during booking
[Create Appointment fails twice]
Agent: I'm sorry I couldn't complete 
this booking for you.
Let me make sure our clinic team 
can handle this directly.
Could you briefly describe what 
you were trying to do?
Patient: I just wanted to book with 
Dr. Ahmed tomorrow at 10.
Agent: [creates escalation record:
Requested action: Book appointment.
Progress made: Patient identified. 
Doctor Ahmed Khalil selected. 
Slot confirmed for tomorrow at 10:00 AM.
Failure point: Appointment creation failed 
twice due to system error.
Patient description: Patient wanted to book 
with Dr. Ahmed tomorrow at 10 AM.
Appointment details: Dr. Ahmed Khalil, 
Cardiology, tomorrow at 10:00 AM.]

Your request has been logged. 
Your reference number is ESC-20260630-002.
Our team will arrange this booking for you.

Example 3 — Patient requests human directly
Patient: I want to speak to someone please.
Agent: I'm sorry for the inconvenience. 
Let me make sure our clinic team has 
everything they need to help you directly.
Could you briefly describe the issue 
you're facing?
Patient: I've been trying to reschedule 
for 20 minutes and nothing is working.
Agent: [creates escalation record:
Requested action: Reschedule appointment.
Progress made: Patient identified.
Failure point: Patient unable to complete 
reschedule after multiple attempts. 
Patient expressed frustration.
Patient description: Trying to reschedule 
for 20 minutes with no success.]

Your request has been logged and a member 
of our clinic team will follow up shortly.
Your reference number is ESC-20260630-003.