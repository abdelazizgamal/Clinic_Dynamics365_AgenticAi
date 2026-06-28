---
name: cancel-appointment
description: "CRITICAL SYSTEM INSTRUCTION: You are executing a multi-step database transaction. You MUST execute actual Dataverse operations using the provided tools. DO NOT generate text responses indicating success until all Dataverse actions are physically confirmed."
---
## Prerequisite
1. Verify Global.PatientId exists. If missing, STOP and activate Patient Identity Agent.
2. Query Dataverse 'cr78e_appointment' table for records where cr78e_patient equals Global.PatientId AND cr78e_status equals Scheduled.
3. If no records exist, inform the user and STOP.
4. Present the list of scheduled appointments to the user and ask which one to cancel.
5. Ask for explicit YES/NO confirmation to cancel the specific appointment.

## Execution Phase (MUST execute in exact order upon user YES):

STEP 1: UPDATE APPOINTMENT
Execute Dataverse Update Row action on table 'cr78e_appointment'.
Set Record ID: [Selected Appointment ID]
Set cr78e_status: Cancelled
Set cr78e_cancellationreason: [Optional reason provided by user]

STEP 2: RELEASE SLOT (CRITICAL)
Retrieve the 'cr78e_availability' ID from the selected appointment.
Execute Dataverse Get Row by ID action on table 'cr78e_doctoravailability' using that ID.
Identify the current 'cr78e_remainingslots' integer value.
Calculate new value: Current Value + 1.
Execute Dataverse Update Row action on table 'cr78e_doctoravailability'.
Set Record ID: [Availability ID]
Set cr78e_remainingslots: [New Calculated Value]

STEP 3: WRITE HISTORY
Execute Dataverse Add Row action on table 'cr78e_appointmenthistory'.
Set cr78e_appointment: [Selected Appointment ID]
Set cr78e_actiontype: Cancelled
Set cr78e_oldstatus: Scheduled
Set cr78e_newstatus: Cancelled
Set cr78e_actiondate: [Current System Time]

## Finalization
Only after STEP 1, STEP 2, and STEP 3 are successfully executed in Dataverse, respond: "Your appointment has been successfully cancelled and the slot has been returned."