# Диаграмма классов (Доменная модель)

Диаграмма описывает статическую структуру бизнес-сущностей гостиничного комплекса, их атрибуты, методы и связи, согласно лекции 4.

```mermaid
classDiagram
    direction TB

    class Guest {
        +int id
        +String fullName
        +String passportData
        +String phone
        +String email
    }

    class Room {
        +int id
        +String roomNumber
        +int floor
        +int capacity
        +RoomStatus status
        +changeStatus(RoomStatus newStatus)
    }

    class RoomType {
        +int id
        +String name
        +String description
        +decimal basePrice
    }

    class Booking {
        +int id
        +Date checkInDate
        +Date checkOutDate
        +BookingStatus status
        +confirm()
        +cancel()
        +checkIn()
    }

    class Stay {
        +int id
        +DateTime actualCheckIn
        +DateTime actualCheckOut
        +decimal totalAmount
        +checkOut()
    }

    class Payment {
        +int id
        +Date date
        +decimal amount
        +PaymentType type
        +PaymentStatus status
    }

    %% Перечисления (Enums) для статусов
    class RoomStatus {
        <<enumeration>>
        FREE
        OCCUPIED
        DIRTY
        UNDER_REPAIR
    }

    class BookingStatus {
        <<enumeration>>
        CREATED
        ACTIVE
        CANCELLED
        NO_SHOW
        COMPLETED
    }

    class PaymentType {
        <<enumeration>>
        CASH
        CARD
    }

    class PaymentStatus {
        <<enumeration>>
        PENDING
        PAID
        REFUNDED
    }

    %% Связи
    Guest "1" --> "*" Booking : создает
    Room "1" --> "*" Booking : бронируется
    Room "1" --> "1" RoomType : имеет тип
    
    Booking "1" *-- "1" Stay : превращается в
    Stay "1" *-- "*" Payment : включает
    
    %% Ограничения (бизнес-правила)
    note for Room "Бизнес-правило: заселение только если status == FREE"
    note for Booking "Бизнес-правило: отмена < 24ч = штраф"
