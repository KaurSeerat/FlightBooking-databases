# ✈️ Flight Booking Database System (SQL Coursework Project)

This project implements a **relational database system for managing airline flight bookings**, developed as part of the **CMP-4010B Database Systems module**.

The system models a simplified airline reservation platform where customers can search flights, book seats, manage passengers, and track booking information.

The project focuses on **database schema design, SQL constraints, and transaction processing** using relational database principles.

---

# 📌 Project Objective

The goal of this coursework was to design and implement a **fully functional relational database** that supports flight booking operations while ensuring **data integrity and consistency**.

The system demonstrates how SQL can be used to manage airline booking workflows such as:

- creating flights
- checking seat availability
- managing bookings
- handling passenger records
- tracking booking status

---

# 🗄️ Database Design

The database uses a **relational schema with multiple connected entities**.

## Main Entities

### Flights
- FlightID
- FlightDate
- Origin
- Destination
- Maximum capacity
- Price per seat

### Customers
- CustomerID
- FirstName
- Surname
- BillingAddress
- Email

### Bookings
- BookingID
- CustomerID
- FlightID
- Number of seats
- Booking status
- Booking timestamp

### Passengers
- PassengerID
- FirstName
- Surname
- Passport number
- Nationality
- Date of birth

These entities are connected through **primary and foreign key relationships** to maintain referential integrity.

---

# ⚙️ Database Features Implemented

The project demonstrates several core SQL concepts.

## Database Definition (DDL)

- Table creation
- Primary keys
- Foreign keys
- Check constraints
- Indexes
- Views
- Triggers and functions

## Data Manipulation (DML)

- Insert flight records
- Insert customer bookings
- Insert passenger data
- Delete records
- Update booking status

## Transactions

The database supports booking workflows such as:

- checking seat availability
- inserting new bookings
- handling booking cancellations
- validating data constraints

---

# 🔎 Example Queries

### Check seat availability

```sql
SELECT FlightID, FlightDate,
COUNT(BookingID) AS BookedSeats,
(MaxCapacity - COUNT(BookingID)) AS AvailableSeats
FROM Flights
JOIN Bookings USING (FlightID)
GROUP BY FlightID, FlightDate, MaxCapacity;
```

### Rank customers by total spending

```sql
SELECT CustomerID,
COUNT(BookingID) AS TotalBookings,
SUM(PricePerSeat * NumSeats) AS TotalSpend
FROM Bookings
JOIN Flights USING (FlightID)
GROUP BY CustomerID
ORDER BY TotalSpend DESC;
```

---

# 🧠 Key Skills Demonstrated

- SQL
- Relational database design
- Data integrity constraints
- Transaction management
- Multi-table queries
- Data modeling
- Query optimisation basics

---

# 🛠️ Tools Used

- PostgreSQL
- pgAdmin
- SQL

---

# 🎓 Academic Context

This project was completed as part of a **Database Systems coursework assignment** where the objective was to design and test a relational database capable of supporting flight booking transactions.

The coursework required implementing:

- database schema definitions
- constraints and triggers
- transactional queries
- testing scenarios demonstrating successful and unsuccessful operations.

---

# 📂 Repository Contents

- SQL scripts for database schema
- queries for transaction testing
- booking and passenger management queries

---

# 👩‍💻 Author

**Seerat Kaur**

Data Analyst | Data Science Enthusiast  
Python • SQL • Machine Learning • Power BI
