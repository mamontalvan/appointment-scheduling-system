### 1. Sequence Diagram – Request Appointment

```mermaid

sequenceDiagram
    participant P as Patient
    participant UI as Web/App UI
    participant SE as SchedulingEngine
    participant DB as Database

    P->>UI: Request appointment
    UI->>SE: createAppointmentRequest()
    SE->>DB: save AppointmentRequest(status=Pending)
    DB-->>SE: OK
    SE-->>UI: Confirm request created
    UI-->>P: Display message and proceed to available times

```

### 2. Sequence Diagram – View Available Times

```mermaid

sequenceDiagram
    participant P as Patient
    participant UI as Web/App UI
    participant SE as SchedulingEngine
    participant DB as Database

    P->>UI: View available times
    UI->>SE: getAvailableSlots()
    SE->>DB: query Availability where isAvailable = true
    DB-->>SE: list of available slots
    SE-->>UI: return available times
    UI-->>P: Show available time slots

```


### 3. Sequence Diagram – Reserve Appointment Slot

```mermaid

sequenceDiagram
    participant P as Patient
    participant UI as Web/App UI
    participant SE as SchedulingEngine
    participant DB as Database
    participant NS as NotificationService

    P->>UI: Reserve time slot
    UI->>SE: reserveSlot(selectedSlot)
    SE->>DB: validate slot availability
    DB-->>SE: slot still available = true
    SE->>SE: assignDoctor()  <!-- triggers next sequence -->
    SE->>DB: create Appointment(status=Confirmed)
    DB-->>SE: OK
    SE->>NS: sendConfirmation(patient)
    NS-->>SE: Notification sent
    SE-->>UI: Reservation successful
    UI-->>P: Show confirmation

```

### 4. Sequence Diagram – Automatically Assign Doctor

```mermaid

sequenceDiagram
    participant SE as SchedulingEngine
    participant DB as Database
    participant D as Doctor

    SE->>DB: getAvailableDoctors(timeSlot)
    DB-->>SE: list of doctors
    SE->>SE: evaluate doctors (availability + workload)
    SE->>DB: update Appointment(doctorId)
    DB-->>SE: OK
    SE->>D: notify doctor of new appointment
    D-->>SE: acknowledgment

```

### 5. Sequence Diagram – Send Appointment Notifications

```mermaid

sequenceDiagram
    participant SE as SchedulingEngine
    participant NS as NotificationService
    participant DB as Database
    participant P as Patient
    participant D as Doctor

    SE->>NS: createNotification(appointment)
    NS->>DB: save notification record
    DB-->>NS: OK

    NS->>P: sendEmail/SMS/Push
    P-->>NS: delivery confirmation

    NS->>D: sendDoctorNotification()
    D-->>NS: delivery confirmation

    NS->>DB: update notification status

```

### 6. Sequence Diagram – Admin Manage Appointments

```mermaid

sequenceDiagram
    participant A as Admin
    participant UI as Admin Panel
    participant SE as SchedulingEngine
    participant DB as Database
    participant NS as NotificationService
    participant P as Patient
    participant D as Doctor

    A->>UI: Open appointment list
    UI->>DB: getAppointments()
    DB-->>UI: list of appointments
    UI-->>A: display appointments

    A->>UI: Edit/Cancel/Reassign appointment
    UI->>SE: processAdminChange()

    SE->>DB: validate and update appointment
    DB-->>SE: OK

    SE->>NS: notify patient and doctor
    NS->>P: send update notification
    NS->>D: send update notification

    NS-->>SE: notifications logged
    SE-->>UI: update successful
    UI-->>A: show confirmation

```
