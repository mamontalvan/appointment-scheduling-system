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

### 5. Automatically Assign Doctor (versión final)

```mermaid

flowchart TD
A[Start] --> B[System receives appointment slot]
B --> C[Retrieve doctors available at that time]
C --> D{Any available doctors?}

D -->|Yes| E[Evaluate doctors: workload, availability, rules]
E --> F[Select best doctor]
F --> G[Assign doctor to appointment]
G --> H[Update doctor schedule]
H --> I[End]

D -->|No| J[Notify: 'No doctors available']
J --> I

D -->|System Error| K[Log error]
K --> L[Retry or notify patient]
L --> I

```
