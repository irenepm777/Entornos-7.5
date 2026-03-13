# Entornos de Desarrollo — Actividad 7.5
## Gestión de Clases Colectivas con Reserva Previa — GymMaster

Documentación técnica del módulo de gestión de clases colectivas mediante diagramas UML en Mermaid.


## Tarea 1 — Diagrama de Casos de Uso

```mermaid
graph LR
%% Actores
Member((Member))
Administrator((Administrator))

%% Límite del sistema
subgraph "System: GymMaster Booking Management"
    Login([Login])
    BookClass([Book class])
    JoinWaitingList([Join waiting list])
    CreateClass([Create class])
    CancelSession([Cancel session])
end

%% Relaciones principales
Member --- BookClass
Member --- JoinWaitingList
Administrator --- CreateClass
Administrator --- CancelSession

%% Include
BookClass -. "<<include>>" .-> Login
CreateClass -. "<<include>>" .-> Login
CancelSession -. "<<include>>" .-> Login

%% Extend
JoinWaitingList -. "<<extend>>" .-> BookClass
```

## Tarea 2 — Diagrama de Secuencia: Confirmar Reserva

```mermaid
sequenceDiagram
    actor Member
    participant WebInterface
    participant BookingManager
    participant Database

    Member->>WebInterface: confirmBooking(classId)
    WebInterface->>BookingManager: confirmBooking(memberId, classId)
    BookingManager->>Database: checkAvailability(classId)
    Database-->>BookingManager: availabilityStatus

    alt Seats available
        BookingManager->>Database: saveBooking(memberId, classId)
        Database-->>BookingManager: bookingSaved
        BookingManager-->>WebInterface: bookingConfirmed()
        WebInterface-->>Member: showSuccessMessage()
    else No seats available
        BookingManager-->>WebInterface: noSeatsAvailable()
        WebInterface-->>Member: showWaitingListOption()
    end
```

## Tarea 3 — Diagrama de Comunicación
```mermaid
graph LR

Member
WebInterface
BookingManager
Database

Member --> WebInterface
WebInterface --> BookingManager
BookingManager --> Database
Database --> BookingManager
BookingManager --> WebInterface
WebInterface --> Member
BookingManager --> WebInterface
WebInterface --> Member

1. Member → WebInterface : confirmBooking(classId)  
1.1 WebInterface → BookingManager : confirmBooking(memberId, classId)  
1.1.1 BookingManager → Database : checkAvailability(classId)  
1.1.2 Database → BookingManager : availabilityStatus  
1.2 BookingManager → WebInterface : bookingConfirmed()  
1.3 WebInterface → Member : showSuccessMessage()  

Flujo alternativo si no hay plazas:

1.2a BookingManager → WebInterface : noSeatsAvailable()  
1.3a WebInterface → Member : showWaitingListOption()  
```

## Tarea 4 — Diagrama de Actividades: Validación de Reserva
```mermaid
flowchart TD
    A([Start]) --> B[Receive booking request]
    B --> C{Fee paid?}

    C -- Yes --> D{Seats available?}
    C -- No --> Z[Reject booking]

    D -- Yes --> E[Block seat]
    D -- No --> Y[Add to waiting list]

    E --> F[Send confirmation email]
    F --> G([End])

    Z --> G
    Y --> G
```

## Tarea 5 — Diagrama de Estados: Objeto Reservation
```mermaid
stateDiagram-v2
    [*] --> Pending

    Pending --> Confirmed: confirm()
    Pending --> Cancelled: cancel()

    Confirmed --> Cancelled: cancel()
    Confirmed --> Realized: checkIn()
    Confirmed --> NoShow: markNoShow()

    Realized --> [*]
    Cancelled --> [*]
    NoShow --> [*]
```
