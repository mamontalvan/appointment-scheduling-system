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
