---
name: show-patient-profile
description: |-
  Displays the confirmed patient's 
  profile details in a safe, masked format. 
  Never exposes sensitive information in full.
---
## Purpose
Allow a patient to review their own 
registered information on request.

## When Activated
When an identified patient asks to see 
their profile, their details, or 
their account information.
Never runs before patient identity 
is confirmed in this session.

## What the Agent Does

Query the Patient table for the confirmed 
patient's record using the PatientId 
stored in session context.

Read the actual values returned from 
the query result. Do not generate or 
assume any field value. If a field is 
empty in the result, skip it entirely.

Present only what the record actually contains:

Retrieve the confirmed patient's record 
from the Patient table.

Display the following fields only:
Full name — shown in full.
Phone number — always masked.
  Show first two digits, five asterisks, 
  last three digits.
  Example: 01*****678
Preferred language — if set.
Registration date — formatted clearly.
  Example: Registered since 15 January 2025.

Do not show email, notes, gender, 
or any internal fields unless 
the patient explicitly asks for 
their email address — then show it only 
if it is recorded and the patient 
has already been verified this session.

After displaying, ask if there is 
anything else they need.

## Edge Cases

Patient asks to update their details:
Explain that profile updates are handled 
by clinic staff.
Offer to escalate if they need a change.

Patient asks for their full phone number:
Never show it in full.
Explain that the full number is masked 
for security.
If they need to update it, offer to escalate.

Patient asks if their email is correct:
Show the email only if it is on record 
and the patient is already verified.
If no email is recorded — say so clearly.

## What the Agent Never Does
Never show the full phone number.
Never show internal IDs or record fields.
Never show another patient's profile.
Never allow edits from this skill.

## Examples

Example 1 — Standard profile view
Patient: Can I see my details?
Agent: Here are your registered details.

Name: Ahmed Hassan
Phone: 01*****678
Preferred language: Arabic
Registered since: 10 March 2025

Is there anything else I can help you with?

Example 2 — Patient asks for full phone
Patient: What is my full phone number?
Agent: For your security, I can only 
show a masked version of your number: 
01*****678.
If you need to update your contact details, 
I can connect you with our clinic team.

Example 3 — Patient wants to update name
Patient: Can you change my name?
Agent: Profile updates are handled by 
our clinic staff directly. 
Would you like me to connect you with 
someone who can help?