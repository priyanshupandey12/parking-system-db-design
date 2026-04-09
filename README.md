# 🚗 Multi-Zone Event Parking System (ERD Design)

## Overview

This project models a **multi-zone parking system** designed for large-scale events such as conventions, exhibitions, or festivals. The system handles vehicle entry/exit, parking allocation, reserved categories, ticketing, and payments.

The design ensures:

* Proper normalization
* Clear separation of concerns
* Scalability for large events
* Accurate tracking of parking usage and payments

---

## Core Features

* Track vehicles entering and exiting the venue
* Categorize vehicles (bike, car, SUV, etc.)
* Assign parking spots based on compatibility and availability
* Support reserved parking (VIP, staff, exhibitors, EV, etc.)
* Manage zones and levels within the parking facility
* Generate tickets for each parking session
* Track entry and exit timestamps
* Calculate and store parking fees
* Record payment details
* Monitor real-time parking availability

---

## Database Design

### 1. Vehicle Domain

#### `vehicle_categories`

Stores types of vehicles.

| Field | Type      | Description                         |
| ----- | --------- | ----------------------------------- |
| id    | SERIAL PK | Unique identifier                   |
| name  | VARCHAR   | Vehicle type (bike, car, SUV, etc.) |

---

#### `vehicles`

Stores individual vehicles.

| Field       | Type      | Description                 |
| ----------- | --------- | --------------------------- |
| id          | SERIAL PK | Unique identifier           |
| category_id | INT FK    | References vehicle category |
| number      | TEXT      | Vehicle number              |
| created_at  | TIMESTAMP | Record creation time        |

---

### 2. Parking Structure

#### `zones`

Represents parking zones.

| Field | Type      |
| ----- | --------- |
| id    | SERIAL PK |
| name  | VARCHAR   |

---

#### `levels`

Represents levels inside zones.

| Field        | Type      |
| ------------ | --------- |
| id           | SERIAL PK |
| zone_id      | INT FK    |
| level_number | INT       |

---

#### `spot_categories`

Defines reservation types.

| Field | Type      |
| ----- | --------- |
| id    | SERIAL PK |
| name  | VARCHAR   |

Examples:

* VIP
* Staff
* Exhibitor
* EV
* Cosplayer
* General

---

#### `spots`

Represents individual parking slots.

| Field               | Type      | Description                     |
| ------------------- | --------- | ------------------------------- |
| id                  | SERIAL PK | Unique spot                     |
| vehicle_category_id | INT FK    | Allowed vehicle type            |
| spot_category_id    | INT FK    | Reservation category            |
| level_id            | INT FK    | Location                        |
| slot_number         | INT       | Unique slot number              |
| status              | VARCHAR   | available / occupied / reserved |
| created_at          | TIMESTAMP | Created time                    |

---

### 3. Parking Flow

#### `tickets`

Generated at entry.

| Field     | Type      |
| --------- | --------- |
| id        | SERIAL PK |
| issued_at | TIMESTAMP |

---

#### `sessions`

Represents a complete parking lifecycle.

| Field        | Type      | Description        |
| ------------ | --------- | ------------------ |
| id           | SERIAL PK | Session ID         |
| vehicle_id   | INT FK    | Vehicle            |
| spot_id      | INT FK    | Assigned spot      |
| ticket_id    | INT FK    | Entry ticket       |
| entry_time   | TIMESTAMP | Entry time         |
| exit_time    | TIMESTAMP | Exit time          |
| status       | VARCHAR   | active / completed |
| total_amount | DECIMAL   | Parking fee        |
| created_at   | TIMESTAMP | Record time        |

---

### 4. Payments

#### `payments`

Stores payment transactions.

| Field      | Type      |                               |
| ---------- | --------- | ----------------------------- |
| id         | SERIAL PK |                               |
| session_id | INT FK    |                               |
| method     | VARCHAR   | cash / upi                    |
| amount     | DECIMAL   |                               |
| status     | VARCHAR   | failed / completed / rejected |
| created_at | TIMESTAMP |                               |

---

## Relationships

### Vehicle Relationships

* vehicles.category_id → vehicle_categories.id

### Session Flow

* vehicles.id → sessions.vehicle_id
* spots.id → sessions.spot_id
* tickets.id → sessions.ticket_id

### Payment Flow

* sessions.id → payments.session_id

### Parking Structure

* levels.zone_id → zones.id
* spots.level_id → levels.id





## Conclusion

This ERD provides a **normalized, scalable, and production-ready** foundation for a multi-zone parking system. It ensures clean relationships, avoids redundancy, and supports all operational requirements for large event parking management.
