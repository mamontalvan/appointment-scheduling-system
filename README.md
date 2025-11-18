# System Design - Appointment Scheduling System

Requirements:
- The system should allow patients to request appointments without specifying a specific doctor.
- The platform should internally assign a doctor based on availability.
- The system should handle a high volume of appointment requests, ensuring reliable scheduling even in the event of failures.
- Patients should be able to see available appointment times and reserve that slot.
- The system should internally assign an available doctor based on their schedule.
- Doctors should have the ability to set their availability and view their assigned appointments.
- The system should handle a large number of appointment requests and ensure low latency for scheduling.
- The system should be fault-tolerant and capable of recovering from server crashes, network issues, or other technical problems.
- Patients should receive appointment confirmations and reminders via email or notifications.
- The system should provide an admin interface for clinic staff to manage doctor schedules and appointments.


Features:
- Handle appointments request 
- Patients:
  - See available times
  - Reserve slot
  - Request appointment
- Doctors:
  - set availability
  - view assigned appointments
- Notification
  - create notifications
 
    
Actors:
- Doctors
- Patients
- Administrator
 
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/13e36918-64df-47f0-b5bd-a8c1a8896f7f" />

## Use Cases

### Use Case 1: Request Appointment
1. Actors: Patient (primary actor)
2. Brief Description: The patient initiates a request to schedule an appointment without selecting a specific doctor. The system registers the appointment request and will later proceed with doctor assignment and time reservation.
   
3. Preconditions:
   - The patient must be registered and authenticated.
   - The system must have available appointment slots.
   - At least one doctor must have a defined availability schedule.
     
4. Postconditions:
   - The appointment is successfully scheduled.
   - A doctor is assigned to the appointment.
   - A confirmation notification is sent to the patient.
     
5. Main Flow
   - The patient selects an available appointment slot.
   - The system validates that the selected slot is still available.
   - The system automatically assigns a doctor based on availability and load.
   - The system creates an Appointment record with status Confirmed.
   - The system sends a confirmation email or in-app notification to the patient.
   - The system updates the doctor's schedule with the new appointment.
   - The use case ends successfully.
     
6. Alternative Flows
   
   A1 – Slot No Longer Available
   
     1. The system detects that the selected slot has been taken.
     2. The system displays a message: “This time slot is no longer available.”
     3. The system refreshes the list of available slots.
     4. The patient may select a new slot.
     5. The flow returns to Step 1 of the main flow.
   
   A2 – No Doctor Available for the Selected Slot

     1. The system cannot find an available doctor for the chosen slot.
     2. The system displays a message: “No doctors are available for this slot.”
     3. The system refreshes availability or suggests alternative times.
     4. The patient may choose a new slot.
        
   A3 – System Failure During Scheduling
  
     1. A technical issue prevents the system from confirming the appointment.
     2. The system displays an error message and logs the incident.
     3. The system may retry the operation automatically (depending on architecture).
     4. If retry fails, the patient is prompted to try again later.
  
7. Business Rules
   
   - BR-01: The doctor assignment must be automatic based on availability and workload.
   - BR-02: Appointment reservations must be atomic — either fully confirmed or not created.
   - BR-03: Notifications must be sent upon successful reservation.
   - BR-04: The system must maintain low-latency behavior even under high load.
   - BR-05: The system must prevent race conditions when two users select the same slot.
   
### Use Case 2: View Available Appointment Times
1. Actors: Patient (primary actor)
2. Brief Description: The patient views the list of available appointment time slots after submitting an appointment request. The system retrieves and displays time slots that are free and valid.  
   
3. Preconditions:
   - The patient must have initiated an appointment request.
   - The patient must be authenticated.
   - The system must have available appointment slots stored.
   - Doctor availability must be already configured in the system.
     
4. Postconditions:
   - The patient sees a list of available appointment times.
   - The patient can proceed to select and reserve a slot.
     
5. Main Flow
   - The patient selects the option to view available times.
   - The system retrieves all open appointment slots.
   - The system filters out unavailable or already reserved slots.
   - The system displays the list of available times.
   - The use case ends when the list is presented to the patient.
     
6. Alternative Flows
   
   A1 – No Available Time Slots
   
     1. The system determines that there are zero available slots.
     2. The system displays the message: “No appointment times are currently available.”
     3. The system may offer the patient an option to enable notifications for future availability.
     4. The use case ends.
   
   A2 – System Failure / High Load

     1. The system fails to retrieve available times due to high traffic or internal error.
     2. The system displays an error message.
     3. The system logs the error.
     4. The patient is asked to try again later.
  
7. Business Rules
   
   - BR-01: Only valid, unassigned time slots must be displayed.
   - BR-02: Time slot availability must reflect real-time system state.
   - BR-03: The system must ensure low latency when retrieving available times.

### Use Case 3: Reserve Appointment Slot
1. Actors: Patient (primary actor)
2. Brief Description: The patient selects an available appointment time. The system reserves the slot, automatically assigns an available doctor, and confirms the appointment. The system ensures that the selected time is still available at the moment of reservation.
   
3. Preconditions:
   - The patient must have already requested an appointment.
   - The patient must be authenticated.
   - The system must have available appointment slots.
   - Doctor availability must be defined in the system.
     
4. Postconditions:
   - The appointment is successfully scheduled.
   - A doctor is assigned to the appointment.
   - A confirmation notification is sent to the patient.
     
5. Main Flow
   - The patient selects an available appointment slot.
   - The system validates that the selected slot is still available.
   - The system automatically assigns a doctor based on availability and load.
   - The system creates an Appointment record with status Confirmed.
   - The system sends a confirmation email or in-app notification to the patient.
   - The system updates the doctor's schedule with the new appointment.
   - The use case ends successfully.
     
6. Alternative Flows
   
   A1 – Slot No Longer Available
   
     1. The system detects that the selected slot has been taken.
     2. The system displays a message: “This time slot is no longer available.”
     3. The system refreshes the list of available slots.
     4. The patient may select a new slot.
     5. The flow returns to Step 1 of the main flow.
   
   A2 – No Doctor Available for the Selected Slot

     1. The system cannot find an available doctor for the chosen slot.
     2. The system displays a message: “No doctors are available for this slot.”
     3. The system refreshes availability or suggests alternative times.
     4. The patient may choose a new slot.
        
   A3 – System Failure During Scheduling
  
     1. A technical issue prevents the system from confirming the appointment.
     2. The system displays an error message and logs the incident.
     3. The system may retry the operation automatically (depending on architecture).
     4. If retry fails, the patient is prompted to try again later.
  
9. Business Rules
   
   - BR-01: The doctor assignment must be automatic based on availability and workload.
   - BR-02: Appointment reservations must be atomic — either fully confirmed or not created.
   - BR-03: Notifications must be sent upon successful reservation.
   - BR-04: The system must maintain low-latency behavior even under high load.
   - BR-05: The system must prevent race conditions when two users select the same slot.

### Use Case 4: Manage Doctor Availability
1. Actors: Doctor (primary actor) and Clinic Staff / Admin (secondary actor, optional)
2. Brief Description: Doctors (or clinic staff on their behalf) can define, update, or remove availability slots. This schedule determines when doctors can be assigned to appointments.
   
3. Preconditions:
   - The doctor or admin must be authenticated.
   - The doctor must exist in the system.
   - The system must support schedule management functionality.
     
4. Postconditions:
   - The doctor’s availability schedule is updated.
   - New time slots become visible to patients.
   - Invalid or outdated time slots are removed.
     
5. Main Flow
   - The doctor accesses the Manage Availability section.
   - The system displays the current availability schedule.
   - The doctor adds, edits, or removes available time slots.
   - The system validates the changes (no overlapping slots, correct format, etc.).
   - The system saves the updated schedule.
   - The system updates internal availability tables.
   - The use case ends successfully.
     
6. Alternative Flows
   
   A1 – Invalid Availability Slot
   
     1. The doctor enters an invalid time slot (overlapping, in the past, wrong format).
     2. The system highlights the invalid entry.
     3. The system displays a message explaining the issue.
     4. The doctor corrects the information.
     5. Flow returns to Step 4 of the main flow.
   
   A2 – System Failure

     1. The system is unable to save the updated schedule (e.g., database failure).
     2. The system displays an error message.
     3. The system logs the error.
     4. The doctor may attempt the operation again later.
  
   A3 – Admin Updates Schedule on Doctor’s Behalf

     1. Clinic staff selects the doctor’s profile.
     2. Clinic staff edits the schedule.
     3. The doctor receives a notification informing them of the updated schedule.
     4. Flow continues at Step 4 of the main flow.
  
7. Business Rules
   
   - BR-01: Schedules cannot overlap.
   - BR-02: A doctor must have availability defined before they can be assigned to appointments.
   - BR-03: Only doctors or authorized clinic staff may modify schedules.
   - BR-04: All schedule changes must be logged for auditing purposes.


### Use Case 5: Automatically Assign Doctor
1. Actors: System (primary actor) and Doctor (indirect actor)
2. Brief Description: When a patient selects an appointment slot, the system automatically assigns the best available doctor based on defined criteria such as availability, workload, and schedule compatibility.
   
3. Preconditions:
   - The patient has selected a valid appointment slot.
   - At least one doctor has availability for the selected time.
   - Doctor schedules have been previously configured.
     
4. Postconditions:
   - A doctor is successfully assigned to the appointment.
   - The appointment is updated and ready for confirmation.
   - The doctor's schedule reflects the new assignment.
     
5. Main Flow
   - The system receives the selected appointment slot.
   - The system retrieves the list of doctors available at that time.
   - The system evaluates doctors based on workload, availability, or defined rules.
   - The system selects the most suitable doctor.
   - The system assigns the doctor to the appointment.
   - The appointment is updated with the assigned doctor.
   - The use case ends successfully.
     
7. Alternative Flows
   
   A1 – No Available Doctors
   
     1. The system finds zero doctors available for the chosen slot.
     2. The system displays a message to the patient: “No doctors are available for this time slot.”
     3. The system suggests alternative times.
     4. The workflow returns to viewing or selecting another slot.
   
   A2 – System Error During Assignment

     1. A technical issue prevents doctor assignment.
     2. The system logs the error.
     3. The system retries the assignment automatically if possible.
     4. If retry fails, the patient is informed and asked to select a new time.
  
9. Business Rules
   
   - BR-01: The system must assign the doctor automatically—patients cannot choose.
   - BR-02: Assignment must balance workload among doctors.
   - BR-03: A doctor cannot be double-booked.
   - BR-04: Assignment must be atomic (all-or-nothing).


### Use Case 6: Send Appointment Notifications
1. Actors: System (primary actor), Patient (receiver) and Doctor (receiver)
2. Brief Description: The system sends appointment confirmations, updates, and reminders via email or in-app notifications to ensure that patients and doctors stay informed.
   
3. Preconditions:
   - An appointment has been created or updated.
   - The system has access to email or notification services.
   - Patient and doctor communication preferences are stored.
     
4. Postconditions:
   - The patient receives a confirmation or reminder.
   - The doctor receives notification of a newly assigned appointment.
   - Notification logs are recorded.
     
5. Main Flow
   - A new appointment is created or an existing one is updated.
   - The system prepares the notification content.
   - The system sends the notification via the configured channels (email, app, SMS, etc.).
   - The system saves a record of the sent notifications.
   - The use case ends successfully.
     
6. Alternative Flows
   
   A1 – Notification Service Failure
   
     1. The notification service is temporarily unavailable.
     2. The system logs the failure.
     3. The system retries after a predefined interval.
     4. If retries fail, the system marks the notification as “Failed.”
     5. Patient may receive an alternative message when service is restored.
   
   A2 – Invalid or Missing Contact Information

     1. TPatient or doctor has no valid email/notification channel.
     2. The system flags the account and records the issue.
     3. Admin is alerted to update contact details.
  
7. Business Rules
   
   - BR-01: All confirmed appointments must generate notifications.
   - BR-02: Reminders must be sent within a configurable timeframe (e.g., 24 hours before).
   - BR-03: All notifications must be logged for audit purposes.
   

### Use Case 7: Manage Appointments (Admin)
1. Actors: Clinic Staff / Administrator (primary actor)
2. Brief Description: Admins can view, edit, cancel, or reassign appointments. This use case ensures that clinic staff can intervene when necessary, especially in emergencies, doctor absences, or patient changes.
   
3. Preconditions:
   - Admin user must be authenticated.
   - Appointments must exist in the system.
   - Admin has proper permissions.
     
4. Postconditions:
   - Appointment records are updated to reflect admin changes.
   - Patients and doctors receive updated notifications.
   - All changes are logged.
     
5. Main Flow
   - Admin accesses the appointment management panel.
   - The system displays the list of appointments.
   - Admin selects an appointment.
   - Admin performs an action:
     - Edit appointment details
     - Cancel appointment
     - Reassign doctor
     - Change appointment time
   - The system validates the changes.
   - The system updates the appointment.
   - Notifications are sent to affected users.
   - The changes are logged.
   - The use case ends successfully.
     
6. Alternative Flows
   
   A1 – Invalid Update
   
     1. Admin attempts to assign a doctor who is unavailable.
     2. The system rejects the change.
     3. The system informs the admin of the issue.
     4. Admin must correct the change.
   
   A2 – Appointment Cannot Be Found

     1. The appointment has been deleted or corrupted.
     2. The system displays an error.
     3. Admin returns to the appointment list.
  
   A3 – System Failure

     1. A system or database failure prevents the update.
     2. The system logs the error.
     3. Admin is asked to retry later.
  
7. Business Rules
   
   - BR-01: Only authorized staff may modify appointments.
   - BR-02: All manual changes must trigger new notifications.
   - BR-03: All modifications must be recorded for auditing.
   
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/faa10caf-77da-4269-a6c9-5915f4f75be4" />


