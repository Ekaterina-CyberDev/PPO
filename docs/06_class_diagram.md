# Диаграмма классов (Доменная модель)

**Цель:** Показать статическую структуру бизнес-сущностей, их атрибуты и связи, без привязки к технической реализации (БД, фреймворки).

### Основные сущности и связи:
*   **Гость (Guest)** может иметь много **Броней (Booking)** (связь 1 ко многим).
*   **Номер (Room)** относится к определенной **Категории (RoomType)**.
*   **Бронь (Booking)** связывает одного Гостя и один Номер на определенные даты.
*   **Проживание (Stay)** является фактом заселения по Брони.
*   **Платеж (Payment)** привязан к Проживанию.

```mermaid
classDiagram
    class Guest {
        +String guestId
        +String fullName
        +String passportData
        +String phone
        +String email
        +createBooking()
    }

    class RoomType {
        +String typeId
        +String name
        +String description
        +Money basePrice
    }

    class Room {
        +String roomNumber
        +Integer floor
        +Integer capacity
        +RoomStatus status
        +changeStatus()
    }

    class Booking {
        +String bookingId
        +Date checkInDate
        +Date checkOutDate
        +BookingStatus status
        +cancel()
        +confirm()
    }

    class Stay {
        +String stayId
        +DateTime actualCheckIn
        +DateTime actualCheckOut
        +Money totalAmount
        +checkOut()
    }

    class Payment {
        +String paymentId
        +Date paymentDate
        +Money amount
        +PaymentType type
        +PaymentStatus status
    }

    class Money {
        +Decimal amount
        +String currency
    }

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
        CONFIRMED
        CANCELLED
        NO_SHOW
        COMPLETED
    }

    %% Связи
    Guest "1" --> "*" Booking : создает
    Booking "*" --> "1" Room : бронирует
    Room "*" --> "1" RoomType : относится к
    Booking "1" --> "0..1" Stay : переходит в
    Stay "1" --> "*" Payment : оплачивает
    Room --> RoomStatus
    Booking --> BookingStatus
    RoomType *-- Money : содержит
    Stay *-- Money : итоговая сумма
