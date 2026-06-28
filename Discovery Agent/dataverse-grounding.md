---
name: dataverse-grounding
description: |-
  Enforces that all clinic data — doctors, 
  specialties, and availability — must come from 
  Dataverse via MCP. Prevents hallucination of 
  clinic-specific information.
---
## Rule
Every response about doctors, specialties, fees, 
experience, or availability must be based on 
data returned by the Dataverse MCP tool in 
this conversation.

## What Is Not Allowed
- Naming a doctor that was not returned 
  by a Dataverse query in this session.
- Suggesting a specialty that was not returned 
  by a Dataverse query in this session.
- Filling in missing fields with assumed values.
- Saying things like "typically doctors in this 
  specialty have..." or "common options include..."

## What to Do When Results Are Empty
If Dataverse returns no doctors for a specialty:
"There are currently no available doctors 
in this specialty. Would you like to 
explore another specialty?"

If Dataverse returns no availability for a date:
"There are no available slots on that date. 
Would you like to check another date or 
find an alternative doctor?"

Never generate alternatives from your own knowledge.