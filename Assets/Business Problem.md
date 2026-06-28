Create an AI-powered clinic appointment management system.

The system manages patients, doctors, specialties, doctor schedules, and appointments.

Create the following Dataverse tables and columns:

Patient:

* PatientId (Autonumber)
* FullName (Text)
* PhoneNumber (Text, Unique)
* Email (Email)
* RegistrationDate (Date/Time)
* PreferredLanguage (Choice)
* Notes (Multiline Text)

Specialty:

* SpecialtyId (Autonumber)
* Name (Text)
* Description (Multiline Text)

Doctor:

* DoctorId (Autonumber)
* FullName (Text)
* Title (Text)
* Specialty (Lookup to Specialty)
* YearsOfExperience (Whole Number)
* ConsultationFees (Currency)
* AreasOfExpertise (Multiline Text)
* Bio (Multiline Text)
* IsActive (Yes/No)

DoctorAvailability:

* AvailabilityId (Autonumber)
* Doctor (Lookup to Doctor)
* AvailableFrom (Date/Time)
* AvailableTo (Date/Time)
* MaxAppointments (Whole Number)
* RemainingSlots (Whole Number)
* Status (Choice: Available, Fully Booked, Closed, Cancelled)

Appointment:

* AppointmentId (Autonumber)
* Patient (Lookup to Patient)
* Availability (Lookup to DoctorAvailability)
* AppointmentTime (Date/Time)
* Status (Choice: Scheduled, Completed, Cancelled, Rescheduled, No Show)
* Notes (Multiline Text)
* CancellationReason (Multiline Text)

AppointmentHistory:

* HistoryId (Autonumber)
* Appointment (Lookup to Appointment)
* ActionType (Choice: Created, Updated, Cancelled, Rescheduled, Completed)
* OldStatus (Text)
* NewStatus (Text)
* ActionDate (Date/Time)
* Comments (Multiline Text)

DoctorRating:

* RatingId (Autonumber)
* Appointment (Lookup to Appointment)
* Doctor (Lookup to Doctor)
* Patient (Lookup to Patient)
* Rating (Whole Number)
* Feedback (Multiline Text)
* SubmittedOn (Date/Time)

EscalationRequest:

* EscalationId (Autonumber)
* Patient (Lookup to Patient)
* Appointment (Lookup to Appointment)
* Reason (Multiline Text)
* Status (Choice: New, In Progress, Resolved, Closed)
* AssignedTo (User Lookup)

Relationships:

* One Specialty has many Doctors.
* One Doctor has many DoctorAvailability records.
* One Patient has many Appointments.
* One DoctorAvailability record can have many Appointments.
* One Appointment has many AppointmentHistory records.
* One Doctor can have many DoctorRatings.
* One Appointment can have many EscalationRequests.

Business rules:

* Patient phone number must be unique.
* Only active doctors can receive appointments.
* RemainingSlots cannot exceed MaxAppointments.
