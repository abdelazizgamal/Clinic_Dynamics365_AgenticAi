---
name: search-doctors
description: |-
  Finds and presents active doctors 
  by specialty or by date using available tools. 
  Never generates or assumes clinic data.
---
## Specialty First Flow
Activate when:

- The patient selects a specialty.
- The patient asks for doctors in a specialty.
- The patient asks for a specialist.
- The patient mentions a doctor's name.

1. If no specialty given — query the Specialty 
   table and present all specialties as a 
   numbered list with name and description. 
   Ask the patient to choose one.

2. Query the Doctor table for active doctors 
   in the chosen specialty ordered by most 
   experienced first.

3. If no results — respond:
   "There are no available doctors in that 
   specialty right now. Would you like to 
   explore another specialty?"

4. Present up to 5 doctors showing:
   name and title, years of experience, 
   consultation fee, first line of expertise.

5. Ask if the patient wants full details 
   on any of the doctors.

## Query Completeness Rule
When querying doctors by specialty, 
retrieve ALL active doctors in that 
specialty — not just the first page 
of results.

If the initial query returns results, 
do not assume it is complete.
Always present all returned doctors 
up to the maximum of 5 at a time.

If the patient says there should be 
more doctors or asks to see more:
Run the query again requesting 
additional records beyond the 
first result set.
Never tell the patient a list is 
complete unless you have confirmed 
all records were returned.

## Date First Flow

1. Patient mentions a date before choosing 
   a doctor.

2. Confirm your interpretation of the date:
   "I'll search for [Day, Date] — is that right?"

3. Ask for specialty if not given.

4. Query the Doctor Availability table joined 
   with the Doctor table. Find doctors in the 
   chosen specialty where IsActive is true, 
   the requested date falls within the 
   availability window, status is Available, 
   and remaining slots is greater than zero.

5. Present results showing doctor name, title, 
   consultation fee, and window hours. 
   Let the patient choose a doctor.

6. Continue to show-doctor-profile skill.

## Guidelines
If the patient mentions a doctor's name directly:

Skip specialty browsing.

Search for that doctor immediately.
- Maximum 5 doctors per response.
- Only show doctors where IsActive is true.
- Never fabricate a doctor name or detail.
- All data must come from Dataverse only.
- Reject past dates immediately:
  "That date has already passed. 
  Please choose a future date."