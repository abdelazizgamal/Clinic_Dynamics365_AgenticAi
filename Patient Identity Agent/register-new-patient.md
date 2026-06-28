---
name: register-new-patient
description: |-
  Registers a new patient in the 
  Patient table after identify-patient-by-phone 
  has confirmed no existing record for 
  the provided number.
---
## Purpose
Create a new patient record for someone 
contacting the clinic for the first time.

## When Activated
Only when identify-patient-by-phone has 
already run in this session and returned 
no match for the phone number.
Never activated without a prior confirmed 
failed lookup in the same session.

## What the Agent Does

Inform the patient that no record exists 
for their number and ask if they would 
like to register.

If the patient declines:
Respond warmly and close:
"No problem at all. If you need help 
in the future, we're always here. 
Is there anything else I can help 
you with today?"
Do not push further.

If the patient agrees:
Ask for their full name.
Collect gender only if the patient 
offers it voluntarily — never ask for it.
Do not ask for email, address, date of birth, 
or any other field.

Validate the full name:
Accept only letters, spaces, hyphens, 
and apostrophes.
Reject numbers and special characters.
If invalid — ask once more with a 
clear explanation.

Before creating the record, run a 
duplicate check by searching the 
Patient table one more time using 
the same phone number.

If a record now exists — a concurrent 
registration may have occurred.
Abort registration, use the existing 
record, and greet the patient normally.

If no record exists — create the new 
patient record with the full name, 
phone number, and gender if provided.
Set the registration date to today.

After creation:
Greet the patient by first name.
Confirm registration briefly and warmly.
Store the new patient identity in context.
Continue with their original request.

## Edge Cases

Patient gives a name with mixed languages:
Accept Arabic and English names equally.
Do not reject a name because it is 
in a different script.

Patient gives only a first name:
Accept it — do not force a surname.

Patient tries to register a second time 
in the same session:
Detect that identity is already confirmed 
and skip registration.

Patient gives a name with numbers or symbols:
Explain clearly:
"Your name can only contain letters. 
Could you try again?"
Allow one retry. If still invalid — escalate.

## What the Agent Never Does
Never create a record without the 
duplicate check.
Never collect more fields than 
full name and optional gender.
Never expose another patient's 
information at any point.
Never reveal whether a similar name 
already exists.

## Examples

Example 1 — Patient agrees to register
Patient: I want to book an appointment.
[Phone lookup returns no match]
Agent: I couldn't find a record for 
that number. Would you like to register?
Patient: Yes please.
Agent: What is your full name?
Patient: Ahmed Hassan
Agent: Thank you, Ahmed. 
You're now registered. How can I help you?

Example 2 — Patient declines
Agent: I couldn't find a record for 
that number. Would you like to register?
Patient: No, not right now.
Agent: No problem at all. 
If you need help in the future, 
we're always here. Is there anything 
else I can help you with today?

Example 3 — Invalid name
Patient: Hassan123
Agent: Your name can only contain letters. 
Could you try again please?

Example 4 — Duplicate found at second check
[Agent runs duplicate check before creating]
[Record now exists — created by another session]
Agent: Welcome back, Hassan. 
I've found your record. 
How can I help you today?