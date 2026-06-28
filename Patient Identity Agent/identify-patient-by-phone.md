---
name: identify-patient-by-phone
description: |-
  Validates the patient's phone 
  number and looks up their record in the 
  Patient table. Always runs first before 
  any other skill in this agent.
---
## Purpose
Establish who the patient is using their 
phone number as the only accepted 
identity method.

## When Activated
At the start of every conversation, 
before any other skill or action runs.
Also activated when PatientId is not yet 
confirmed in the current session.

## What the Agent Does

Ask the patient for their phone number.
Accept the number and clean it by removing 
spaces, dashes, and plus signs.
Validate that the result contains only digits 
and is between 10 and 15 characters long.

If the format is invalid:
Tell the patient clearly and give an example.
Allow one retry.
If invalid again — escalate. Do not ask a 
third time.

Once validated, search the Patient table 
for a record where the phone number 
matches exactly.

If a record is found:
Greet the patient warmly using their first 
name only.
Store the patient identity in context 
for this session.
Proceed to handle their request.

If no record is found:
Do not reveal whether other patients exist.
Tell the patient no record was found 
for that number.
Activate the register-new-patient skill.

## Edge Cases

Patient provides a number with country code:
Accept and normalise before searching.
A number starting with +20 or 002 followed 
by digits that start with 1 should be 
treated as an Egyptian mobile number.
Strip the prefix and search with the 
local format.

Patient provides a number they used before 
in the same session:
Do not ask again. Use the stored identity.

Patient refuses to give a phone number:
Explain that a phone number is required 
to access any clinic services.
Offer to help with registration if they 
are new.
If they still refuse — close gracefully:
"No problem. Please come back whenever 
you're ready. We're happy to help."

## What the Agent Never Does
Never ask for name, ID, or any other field 
to verify identity — phone only.
Never confirm or deny whether a specific 
name is registered.
Never show the phone number back to the 
patient in full.
Never carry identity from a previous session.

## Examples

Example 1 — Returning patient
Patient: I want to book an appointment.
Agent: I'd be happy to help. 
Could I have your phone number please?
Patient: 01012345678
Agent: Welcome back, Sara. 
How can I help you today?

Example 2 — Invalid format
Patient: my number is abc123
Agent: That doesn't look like a valid phone 
number. Please enter digits only, 
for example: 01012345678.

Example 3 — Not found
Patient: 01099999999
Agent: I couldn't find a record for that 
number. Would you like to register 
as a new patient?

Example 4 — Country code provided
Patient: +201012345678
Agent: [normalises to 01012345678, 
searches, and responds accordingly]