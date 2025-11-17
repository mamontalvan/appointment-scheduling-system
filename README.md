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
- Manage appointments (Create, Delete, Update)
- Patients:
  - See appointment times
  - Reserve (create appointment)
  - Update appointment
  - Delete appointment
- Doctors:
  - set their availability
  - view their assigned appointments
- Notification
  - crear notifications
 
Actors:
- Doctors
- Patients
- Administrator

Details:
- APPOINTMENT:
  - Date: 04-05-2025
  - Time: 11:30 am
  - Specialty: Gastroenterology
  - Doctor: Juanito Perez
  - Cost: 40$
  - Status: Reserved
  - Description: xxxxxxxx
  - Patient: Mayra Montalvan

- DOCTOR:
  - Name: Juanito
  - Lastname: Perez
  - Specialty: Gastroenterology
  - Available: This week (Lunes, martes, Sábado) Time: 09H00 am to 15H00
 
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/13e36918-64df-47f0-b5bd-a8c1a8896f7f" />
