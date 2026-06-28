---
name: rate-appointment
description: |-
  Allows a patient to submit a 
  rating between 1 and 5 for a completed 
  appointment. Checks for duplicate ratings 
  before creating. Writes to the Doctor Rating 
  table with submission date.
---
## Purpose
Collect patient feedback on completed 
visits to help the clinic monitor 
doctor performance and patient satisfaction.

## When Activated
When a patient wants to rate, review, 
or give feedback on a past appointment.

## What the Agent Does

Query the Appointment table for this 
patient's appointments where status 
is Completed.

If none found:
"You don't have any completed appointments 
to rate yet. Your rating can be submitted 
after your appointment is marked as visited."
Stop here.

If one completed appointment exists:
Confirm it with the patient before asking 
for a rating.

If multiple completed appointments exist:
List them — doctor name, date, reference.
Ask which one they want to rate.

Before asking for a rating, check the 
Doctor Rating table for an existing rating 
linked to the selected appointment.

If a rating already exists:
"You have already submitted a rating 
for this appointment.
Thank you for your feedback."
Ask if there is anything else they need.
Do not allow a second rating.

If no rating exists:
Ask for a rating from 1 to 5.
Explain the scale briefly:
"1 is poor and 5 is excellent."

If the patient gives a number outside 
1 to 5 — ask once more:
"Please choose a number between 1 and 5."
If still invalid — skip the rating 
and ask for written feedback only.

Ask for optional written feedback:
"Would you like to share any comments 
about your experience? 
This is completely optional."

Create a Doctor Rating record:
Appointment: the selected appointment.
Doctor: the doctor from that appointment.
Patient: confirmed PatientId.
Rating: the number provided.
Feedback: the written comment if provided.
Submitted on: today.

Confirm to patient:
"Thank you for your feedback, [FirstName].
Your rating for Dr. [Name] has been 
submitted successfully."

## Edge Cases

Patient wants to change a rating 
they already submitted:
"I'm sorry, submitted ratings cannot 
be changed. Your original feedback 
has been recorded."
Do not create a duplicate.

Patient gives a rating but no written feedback:
Accept it — feedback is always optional.
Create the rating record with the 
numeric score only.

Patient gives written feedback but 
no numeric rating:
Ask once more for a number between 1 and 5.
If still not provided — do not create 
a rating record without a score.

Patient rates before the appointment 
is completed:
"Ratings can only be submitted after 
your appointment is marked as visited.
Your appointment on [Date] is currently 
scheduled — you can rate it after it 
takes place."

Patient rates a cancelled appointment:
"Ratings can only be submitted for 
completed visits."
Show their completed appointments 
if any exist.

## What the Agent Never Does
Never create more than one rating 
per appointment.
Never create a rating without a valid 
numeric score.
Never allow rating a Scheduled, Cancelled, 
Rescheduled, or No Show appointment.
Never expose internal record IDs.

## Examples

Example 1 — Successful rating
Patient: I want to rate my last appointment.
Agent: I found your completed appointment.

Dr. Ahmed Khalil — Cardiology
Monday, 30 June 2026 at 10:00 AM

How would you rate this experience 
from 1 to 5, where 1 is poor 
and 5 is excellent?
Patient: 5
Agent: Would you like to add any comments?
Patient: Excellent doctor, very professional.
Agent: Thank you for your feedback, Ahmed.
Your rating for Dr. Ahmed Khalil 
has been submitted successfully.

Example 2 — Already rated
Patient: I want to rate Dr. Ahmed.
Agent: You have already submitted a rating 
for your appointment with Dr. Ahmed Khalil.
Thank you for your feedback.
Is there anything else I can help you with?

Example 3 — No completed appointments
Patient: I want to leave a review.
Agent: You don't have any completed 
appointments to rate yet.
Your rating can be submitted after 
your appointment has taken place.
Would you like to book an appointment?

Example 4 — Rating a cancelled appointment
Patient: Can I rate my cancelled appointment?
Agent: Ratings can only be submitted 
for completed visits.
You don't currently have any completed 
appointments on record.
Would you like help booking a new appointment?