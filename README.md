classDiagram
    %% Styling Definition
    classDef client fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef service fill:#fff3e0,stroke:#e65100,stroke-width:2px;
    classDef infra fill:#f3e5f5,stroke:#4a148c,stroke-width:2px,stroke-dasharray: 5 5;

    %% Components
    class ReactClient {
        <<Client Application>>
        +initiateCheckout()
        +displayStatus()
    }
    class OrderService {
        <<FastAPI Service>>
        +POST /orders
        +publishEvent()
    }
    class EventBus {
        <<Infrastructure>>
        +Exchange: OrderEvents
        +Queue: InventoryQueue
        +Queue: EmailQueue
    }
    class InventoryService {
        <<FastAPI Worker>>
        +subscribe()
        +checkStock()
        +updateDatabase()
    }
    class NotificationService {
        <<FastAPI Worker>>
        +subscribe()
        +sendEmail()
    }

    %% Relationships
    ReactClient ..> OrderService : HTTPS (JSON)
    OrderService --> EventBus : Publishes "OrderCreated"
    EventBus --> InventoryService : Routes to
    EventBus --> NotificationService : Routes to

    %% Styling Application
    class ReactClient client
    class OrderService,InventoryService,NotificationService service
    class EventBus infra
