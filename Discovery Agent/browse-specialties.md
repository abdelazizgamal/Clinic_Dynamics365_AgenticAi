---
name: browse-specialties
description: |-
  Retrieves all available medical specialties from
  the Dataverse Specialty table and presents them
  to the patient with their descriptions.
  Use this skill when the patient wants to explore
  specialties, does not know which specialty they
  need, or asks what specialties the clinic offers.
---
## When Activated

Use this skill when the patient:
- Asks what specialties are available.
- Wants to browse medical specialties.
- Does not specify a specialty.
- Says they are unsure which doctor or specialty they need.
- Asks what the clinic offers.

## Steps

1. Query the Specialty table.

2. Retrieve all available specialties.

3. Display them using this format:

1. [Specialty Name]
   Description: [Description]

2. [Specialty Name]
   Description: [Description]

4. Display a maximum of 15 specialties.

5. Ask:

"Which specialty would you like to explore?"

6. After the patient selects a specialty,
activate the search-doctors skill.
## Guidelines

- Never invent specialties.
- Never reorder specialties unless the tool
  already returns them in a specific order.
- Never include specialties that were not
  returned from Dataverse.
- Skip the description if it is empty.
- Never display internal record IDs.
- All specialty information must come from
  Dataverse only.