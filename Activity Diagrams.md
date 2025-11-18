### 1. Activity Diagram 1: Request Appointment

```mermaid
flowchart TD
        A(["Start"])
        A --> B["Patient selects 'Request Appointmen'"]
        B --> C[System informs doctor will be assigned automatically]
        C --> D[System creates Appointment Request]
        D --> E{Are there available time slots?}
            E -->|Yes| F[Show available appointment times]
            F --> G[End]
        
            E -->|No| H["Show message:''No slots availabl'"]
            H --> I[Offer notifications for future slots]
            I --> G[End]
        E -->|System Error| J[Show error message]
        J --> G[End]


```

### 2. Sequence Diagram – View Available Times

```mermaid
flowchart TD
A[Start] --> B["Patient selects 'View Available Times'"]
B --> C[System retrieves all available time slots]
C --> D{Slots available?}
D -->|Yes| E[Display available slots]
E --> F[End]

D -->|No| G[Show message: 'No available slots']
G --> F

D -->|System Error| H[Show error message]
H --> F

```

### 3. Reserve Appointment Slot

```mermaid

flowchart TD
A[Start] --> B[Patient selects a time slot]
B --> C[System validates slot availability]
C --> D{Slot still available?}

D -->|Yes| E[Trigger automatic doctor assignment]
E --> F[System creates Appointment <<Confirmed>>]
F --> G[Send confirmation to patient]
G --> H[Update doctor's schedule]
H --> I[End]

D -->|No| J[Show message: 'Slot no longer available']
J --> K[Refresh available slots]
K --> I

D -->|System Error| L[Show technical error]
L --> I

```

### 4. Automatically Assign Doctor

```mermaid

flowchart TD
A[Start] --> B[System receives selected time slot]
B --> C[Retrieve available doctors for the slot]
C --> D{Any doctors available?}

D -->|Yes| E[Evaluate doctors: workload, availability]
E --> F[Select best doctor]
F --> G[Assign doctor to appointment]
G --> H[Update appointment record]
H --> I[End]

D -->|No| J[Notify: 'No doctors available for this slot']
J --> I

D -->|System Error| K[Log error]
K --> L[Retry or notify patient]
L --> I

```

### 5. Send Appointment Notifications

```mermaid

flowchart TD
A[Start] --> B[Appointment is created or updated]
B --> C[System prepares notification content]
C --> D[Determine delivery channels: Email, SMS, App]
D --> E[Send notification]

E --> F{Notification sent successfully?}

F -->|Yes| G[Log notification in system]
G --> H[End]

F -->|No| I[Log failure]
I --> J[Retry sending?]

J -->|Yes| E
J -->|No| K[Mark as Failed and alert admin]
K --> H

```

### 6. Admin Manage Appointments

```mermaid

flowchart TD
A[Start] --> B[Admin opens Appointment Management panel]
B --> C[System displays list of appointments]
C --> D[Admin selects an appointment]

D --> E{Select action: Edit / Cancel / Reassign?}

E -->|Edit| F[Admin edits appointment details]
F --> G[System validates changes]
G --> H{Valid?}
H -->|Yes| I[Update appointment]
I --> X[Notify patient & doctor]
X --> Z[Log changes]
Z --> AA[End]

H -->|No| AB[Show validation error]
AB --> D

E -->|Cancel| AC[Admin cancels appointment]
AC --> AD[System confirms cancellation]
AD --> X

E -->|Reassign| AE[Admin selects new doctor]
AE --> AF[System checks doctor availability]
AF --> AG{Doctor available?}

AG -->|Yes| AH[Assign new doctor]
AH --> X

AG -->|No| AI[Show error: doctor unavailable]
AI --> D

E -->|None| AA

```
