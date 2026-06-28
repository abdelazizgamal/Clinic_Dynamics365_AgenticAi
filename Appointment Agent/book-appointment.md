---
name: book-appointment
description: |-
  Creates a new appointment after 
  the patient confirms the selected doctor, 
  availability window, and chosen appointment time. 
  Writes a history entry after every booking.
---
## When Activated
When a patient wants to book an appointment and a doctor, availability window, and preferred time have been passed from the Discovery Agent via the Orchestrator.

## Steps

1. Check Context: Confirm PatientId is in conversation context. If missing — STOP. Inform the Orchestrator to run the Patient Identity Agent. Do not ask the patient for their details.

2. Validate Time: Ensure the requested appointment time is in the future (compared to current system time) and falls strictly within the AvailableFrom and AvailableTo window of the selected Doctor Availability record.

3. Initial Availability Check: Query the Doctor Availability table for the selected record. If RemainingSlots is 0, immediately activate the smart-recommendation skill.

4. Ask for Confirmation: Present the summary (Doctor name, date, chosen time, consultation fee) and ask: "Would you like me to confirm this booking?"

5. On explicit YES, execute the following Dataverse operations in exact order:
   
   a. Double-Check Availability: Query the Doctor Availability record ONE MORE TIME. If RemainingSlots is now 0 (someone else booked it), STOP. Respond: "I apologize, but this slot was just taken." and activate smart-recommendation.
   
   b. Create Appointment: If slots > 0, create a new record in the Appointment table linking the patient, the      availability window, and the chosen time. Set status to Scheduled,
    Appointment Name: patientName - DoctorName - VisitCause like(Nour Ali - Dr. Karim Salah - General Health Checkup)
     Notes: any available informations regarding the appointment.
.
   
   c. Update Slots: Retrieve the current RemainingSlots number, subtract 1, and update the Doctor Availability record with the new RemainingSlots value.
   
   d. Write History: Create a new record in the Appointment History table with action type Created, new status Scheduled, and the current date and time.

6. Confirm Success: "Your appointment with Dr. [Name] on [Date] at [Time] has been booked successfully."

## Guidelines
- NEVER invent or guess patient information, doctor names, times, or fees.
- NEVER skip the Step 5.a double-check before writing to the database.
- Mathematical operations (subtracting slots) must be done exactly as instructed.