---
name: show-doctor-profile
description: |-
  Retrieves and displays a full 
  doctor profile using the Get Doctor Profile 
  tool, then offers to check availability.
---
## When Activated

Activate when:

- The patient selects a doctor.
- The patient asks for more information.
- The patient asks about a specific doctor.
## Steps

1. Query the Doctor table for the selected 
   doctor's full record. Include the linked 
   specialty name from the Specialty table.

2. Display the profile in this format:

   [Title] [Full Name]
   Specialty: [Specialty Name]
   Experience: [X] years
   Consultation Fee: [Fee]

   Areas of Expertise:
   [Expertise]

   About:
   [Bio]

3. Ask: "Would you like to check this 
   doctor's available appointment slots?"
   Yes → activate check-availability skill.
   No → return to doctor list.

## Guidelines
- Skip any section where the field is empty.
- Never invent or fill in missing information.
- Never show internal record IDs to the patient.
- All data must come from Dataverse only.