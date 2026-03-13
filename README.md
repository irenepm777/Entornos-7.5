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
