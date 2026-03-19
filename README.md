# ✈️ Flight Booking Database System

> Relational SQL database modelling airline bookings — DDL schema design, constraints, triggers, transactions, and analytical queries using PostgreSQL.

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-336791?style=flat&logo=postgresql)](https://postgresql.org)
[![SQL](https://img.shields.io/badge/SQL-DDL%20%2B%20DML-003B57?style=flat)](DDL_Statements.sql)

---

## 📌 Project Overview

This project implements a fully functional relational database for managing airline flight bookings. It demonstrates real-world SQL skills including schema design, data integrity constraints, transactional operations, and analytical querying.

**Academic context:** Completed as part of the CMP-4010B Database Systems module, BSc Computing Science, University of East Anglia.

---

## 🗄️ Database Schema

The system models five interconnected entities:

```
Flights ──< Bookings >── Customers
              │
         Passengers
```

| Table | Key Columns |
|---|---|
| `Flights` | FlightID, FlightDate, Origin, Destination, MaxCapacity, PricePerSeat |
| `Customers` | CustomerID, FirstName, Surname, BillingAddress, Email |
| `Bookings` | BookingID, CustomerID (FK), FlightID (FK), NumSeats, Status, Timestamp |
| `Passengers` | PassengerID, FirstName, Surname, PassportNumber, Nationality, DOB |

> 

---

## ⚙️ Features Implemented

**DDL (Schema Definition)**
- Primary keys, foreign keys, check constraints
- Indexes on frequently queried columns
- Views for common reporting queries
- Triggers to enforce business rules (e.g. seat capacity validation)

**DML (Data Manipulation)**
- Insert / update / delete operations
- Booking status management (confirmed, cancelled, pending)

**Transactions**
- Seat availability check → booking insert as atomic operation
- Cancellation workflow with status rollback
- Data constraint validation scenarios (success + failure cases)

---

## 🔎 Analytical Queries

### Seat availability by flight
```sql
SELECT
    f.FlightID,
    f.FlightDate,
    f.Origin,
    f.Destination,
    f.MaxCapacity,
    COALESCE(SUM(b.NumSeats), 0) AS BookedSeats,
    f.MaxCapacity - COALESCE(SUM(b.NumSeats), 0) AS AvailableSeats
FROM Flights f
LEFT JOIN Bookings b ON f.FlightID = b.FlightID
    AND b.Status = 'confirmed'
GROUP BY f.FlightID, f.FlightDate, f.Origin, f.Destination, f.MaxCapacity
ORDER BY f.FlightDate;
```

### Customer spend ranking
```sql
SELECT
    c.CustomerID,
    c.FirstName || ' ' || c.Surname AS CustomerName,
    COUNT(b.BookingID) AS TotalBookings,
    SUM(f.PricePerSeat * b.NumSeats) AS TotalSpend
FROM Customers c
JOIN Bookings b ON c.CustomerID = b.CustomerID
JOIN Flights f ON b.FlightID = f.FlightID
WHERE b.Status = 'confirmed'
GROUP BY c.CustomerID, CustomerName
ORDER BY TotalSpend DESC;
```

### Busiest routes by booking volume
```sql
SELECT
    f.Origin,
    f.Destination,
    COUNT(b.BookingID) AS TotalBookings,
    SUM(b.NumSeats) AS TotalPassengers,
    ROUND(AVG(f.PricePerSeat), 2) AS AvgFareGBP
FROM Flights f
JOIN Bookings b ON f.FlightID = b.FlightID
WHERE b.Status = 'confirmed'
GROUP BY f.Origin, f.Destination
ORDER BY TotalBookings DESC
LIMIT 10;
```

### Cancellation rate by route
```sql
SELECT
    f.Origin,
    f.Destination,
    COUNT(*) AS TotalBookings,
    SUM(CASE WHEN b.Status = 'cancelled' THEN 1 ELSE 0 END) AS Cancellations,
    ROUND(
        100.0 * SUM(CASE WHEN b.Status = 'cancelled' THEN 1 ELSE 0 END) / COUNT(*), 1
    ) AS CancellationRatePct
FROM Flights f
JOIN Bookings b ON f.FlightID = b.FlightID
GROUP BY f.Origin, f.Destination
HAVING COUNT(*) >= 5
ORDER BY CancellationRatePct DESC;
```

---

## 📂 Repository Contents

| File | Description |
|---|---|
| `DDL_Statements.sql` | Full schema — CREATE TABLE, constraints, indexes, triggers, views |
| `DML_Statements.sql` | Insert, update, delete operations with test data |
| `Transactions.sql` | Transactional booking workflows — success and failure scenarios |
| `Transactions Of Interest (Test data).docx` | Test case documentation |

---

## 🛠 Tools

- **PostgreSQL** — relational database engine
- **pgAdmin** — schema design and query execution

---

## 🧠 Skills Demonstrated

SQL schema design · DDL constraints & triggers · Transaction management · Window functions · Multi-table analytical queries · Data integrity enforcement · Query optimisation basics

---

## 👩‍💻 Author

**Seerat Kaur** — Junior Data Analyst
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/seerat-kaur-4878bb249/)
[![GitHub](https://img.shields.io/badge/GitHub-KaurSeerat-181717?style=flat&logo=github)](https://github.com/KaurSeerat)
