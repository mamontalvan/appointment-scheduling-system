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
