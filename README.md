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
2. Brief Description: The patient initiates a request to schedule an appointment without selecting a specific doctor. The system registers the appointment request and will later proceed with doctor assignment and time reservation.
   
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

### Use Case 2: View Available Appointment Times
1. Actors: Patient (primary actor)
2. Brief The patient views the list of available appointment time slots after submitting an appointment request. The system retrieves and displays time slots that are free and valid.
   
4. Preconditions:
   - The patient must have initiated an appointment request.
   - The patient must be authenticated.
   - The system must have available appointment slots stored.
   - Doctor availability must be already configured in the system.
     
5. Postconditions:
   - The patient sees a list of available appointment times.
   - The patient can proceed to select and reserve a slot.
     
6. Main Flow
   - The patient selects the option to view available times.
   - The system retrieves all open appointment slots.
   - The system filters out unavailable or already reserved slots.
   - The system displays the list of available times.
   - The use case ends when the list is presented to the patient.
     
7. Alternative Flows
   
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
  
9. Business Rules
   
   - BR-01: Only valid, unassigned time slots must be displayed.
   - BR-02: Time slot availability must reflect real-time system state.
   - BR-03: The system must ensure low latency when retrieving available times.


### Use Case 1: Reserve Appointment Slot
1. Actors: Patient (primary actor)
2. Brief Description: The patient initiates a request to schedule an appointment without selecting a specific doctor. The system registers the appointment request and will later proceed with doctor assignment and time reservation.
   
4. Preconditions:
   - The patient must be registered and authenticated.
   - The system must have available appointment slots.
   - At least one doctor must have a defined availability schedule.
     
5. Postconditions:
   - The appointment is successfully scheduled.
   - A doctor is assigned to the appointment.
   - A confirmation notification is sent to the patient.
     
6. Main Flow
   - The patient selects an available appointment slot.
   - The system validates that the selected slot is still available.
   - The system automatically assigns a doctor based on availability and load.
   - The system creates an Appointment record with status Confirmed.
   - The system sends a confirmation email or in-app notification to the patient.
   - The system updates the doctor's schedule with the new appointment.
   - The use case ends successfully.
     
7. Alternative Flows
   
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


### Use Case 1: Request Appointment
1. Actors: Patient (primary actor)
2. Brief Description: The patient initiates a request to schedule an appointment without selecting a specific doctor. The system registers the appointment request and will later proceed with doctor assignment and time reservation.
   
4. Preconditions:
   - The patient must be registered and authenticated.
   - The system must have available appointment slots.
   - At least one doctor must have a defined availability schedule.
     
5. Postconditions:
   - The appointment is successfully scheduled.
   - A doctor is assigned to the appointment.
   - A confirmation notification is sent to the patient.
     
6. Main Flow
   - The patient selects an available appointment slot.
   - The system validates that the selected slot is still available.
   - The system automatically assigns a doctor based on availability and load.
   - The system creates an Appointment record with status Confirmed.
   - The system sends a confirmation email or in-app notification to the patient.
   - The system updates the doctor's schedule with the new appointment.
   - The use case ends successfully.
     
7. Alternative Flows
   
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
   
