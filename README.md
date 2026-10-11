**# 🎬 Movie Ticket Booking System
**
**MySQL Database Management & SQL Analytics Project**

A MySQL-based Movie Ticket Booking System designed to manage movie information, theatres, screens, seats, users, show schedules, bookings, and payments. This project demonstrates relational database design and SQL querying through booking reports, revenue analysis, seat pricing, and payment tracking.

## 📖 Project Overview

The Movie Ticket Booking System simulates the database operations of a cinema ticket booking platform. It organizes movie schedules, theatre screens, seat information, customer details, ticket bookings, and payment transactions in a relational database.

The project focuses on:

- Database Design and Management
- Relational Database Modeling
- Movie and Show Management
- Customer and Booking Management
- Seat and Ticket Price Management
- Payment Tracking
- Revenue Analysis
- SQL Queries and Reporting
- Data Integrity and Validation
- Advanced SQL Concepts

## 🚀 Features

### 🎥 Movie Management
- Store movie names, languages, genres, and durations.
- Maintain movie release dates and certificates.
- Track movie status, such as Upcoming, Running, and Completed.
- Filter and sort movies using SQL queries.

### 🏢 Theatre and Screen Management
- Store theatre names, locations, and cities.
- Maintain screen details for each theatre.
- Track screen types such as 2D, 3D, IMAX, and 4DX.
- Connect theatres and screens using foreign keys.

### 🎟️ Seat Management
- Maintain seat numbers and screen assignments.
- Categorize seats as Regular, Premium, and Recliner.
- Store seat prices.
- Analyze seat prices using aggregate functions and subqueries.

### 👥 User Management
- Store customer names, email addresses, phone numbers, and cities.
- Enforce unique email and phone values.
- Maintain customer registration timestamps.
- Analyze customer booking activity.

### 🕒 Show Management
- Schedule movies on specific dates and times.
- Link shows to movies and screens.
- Store show languages and statuses.
- Retrieve movie schedules using JOIN queries.

### 📋 Booking Management
- Record customer bookings and selected shows.
- Maintain ticket quantities and booking amounts.
- Track booking statuses such as Confirmed, Cancelled, and Pending.
- Generate booking reports using multiple JOINs.

### 💳 Payment Management
- Store transaction IDs and payment methods.
- Track payment amounts and payment dates.
- Maintain payment statuses such as Success, Failed, Pending, and Refunded.
- Calculate successful payment revenue.

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| MySQL | Relational database management |
| SQL | Data manipulation and analysis |
| MySQL Workbench | SQL execution and database modeling |
| Mockaroo (optional) | Sample test data generation |
| Git and GitHub | Version control and project hosting |

## 🗄️ Database Tables

The project contains **9 relational tables**.

| No. | Table Name | Description |
|---:|---|---|
| 1 | `movies` | Stores movie information |
| 2 | `theatres` | Stores theatre details |
| 3 | `screens` | Stores screens associated with theatres |
| 4 | `seats` | Stores seat numbers, types, and prices |
| 5 | `users` | Stores customer details |
| 6 | `shows` | Stores movie schedules |
| 7 | `bookings` | Stores customer booking information |
| 8 | `booking_seats` | Maps seats to individual bookings |
| 9 | `payments` | Stores payment and transaction information |

Test data can contain up to 1,000 records per suitable table, depending on the data generated and inserted. The actual row count may differ between tables.

## 🔗 Entity Relationship Diagram (ERD)

The database uses primary keys and foreign keys to connect the tables.

### Entity Relationship Highlights

- One movie can have multiple shows.
- One theatre can contain multiple screens.
- One screen can contain multiple seats.
- One screen can host multiple shows.
- One user can make multiple bookings.
- One show can have multiple bookings.
- One booking can contain multiple booking-seat records.
- One seat can be referenced by booking-seat records.
- One booking can have associated payment records.

The `booking_seats` table connects bookings and seats, while the `payments` table stores payment details associated with bookings.

**ER Diagram:** Add your exported diagram to `ER-Diagram/Movie-Ticket-Booking-ER-Diagram.png` and link it here after uploading:

`![Movie Ticket Booking System ER Diagram](ER-Diagram/Movie-Ticket-Booking-ER-Diagram.png)`

## 🧠 SQL Concepts Used

The project practices the following SQL concepts:

### Database Design
- CREATE DATABASE
- CREATE TABLE
- PRIMARY KEY and FOREIGN KEY
- AUTO_INCREMENT
- NOT NULL, UNIQUE, and DEFAULT
- Data types: INT, VARCHAR, DECIMAL, DATE, TIME, and DATETIME

### Data Manipulation
- INSERT
- SELECT
- UPDATE
- DELETE

### Filtering and Sorting
- WHERE
- AND and OR
- IN and BETWEEN
- LIKE
- DISTINCT
- ORDER BY
- LIMIT

### Aggregate Functions
- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()
- GROUP BY
- HAVING

### Intermediate and Advanced SQL
- CASE
- COALESCE
- INNER JOIN
- LEFT JOIN
- Multiple-table JOINs
- Subqueries
- Views
- Stored Procedures
- Triggers
- Transactions
- COMMIT and ROLLBACK
- Indexing

*Note: Include advanced concepts in your final project description only after verifying that they execute successfully in your database.*

## 🔍 Sample Analytical Queries

### 1. Count Total Movies

```sql
SELECT COUNT(*) AS total_movies
FROM movies;
```

### 2. Find Movies Longer Than 150 Minutes

```sql
SELECT movie_name, duration_minutes
FROM movies
WHERE duration_minutes > 150
ORDER BY duration_minutes DESC;
```

### 3. Count Shows for Each Movie

```sql
SELECT
    m.movie_name,
    COUNT(sh.show_id) AS total_shows
FROM movies AS m
LEFT JOIN shows AS sh
    ON m.movie_id = sh.movie_id
GROUP BY m.movie_id, m.movie_name
ORDER BY total_shows DESC;
```

### 4. Calculate Average Seat Price

```sql
SELECT AVG(price) AS average_seat_price
FROM seats;
```

### 5. Find Seats Priced Above Average

```sql
SELECT seat_id, seat_number, seat_type, price
FROM seats
WHERE price > (
    SELECT AVG(price)
    FROM seats
);
```

### 6. Generate a Movie Show Schedule Report

```sql
SELECT
    m.movie_name,
    t.theatre_name,
    s.screen_name,
    sh.show_date,
    sh.start_time,
    sh.show_status
FROM shows AS sh
JOIN movies AS m
    ON sh.movie_id = m.movie_id
JOIN screens AS s
    ON sh.screen_id = s.screen_id
JOIN theatres AS t
    ON s.theatre_id = t.theatre_id
ORDER BY sh.show_date, sh.start_time;
```

### 7. Calculate Revenue from Successful Payments

```sql
SELECT
    SUM(amount) AS total_revenue
FROM payments
WHERE payment_status = 'Success';
```

### 8. Summarize Payments by Status

```sql
SELECT
    payment_status,
    COUNT(*) AS total_payments,
    SUM(amount) AS total_amount
FROM payments
GROUP BY payment_status;
```

### 9. Generate a Complete Booking Report

```sql
SELECT
    b.booking_id,
    u.user_name,
    m.movie_name,
    t.theatre_name,
    s.screen_name,
    sh.show_date,
    sh.start_time,
    b.total_seats,
    b.total_amount,
    b.booking_status,
    p.payment_method,
    p.payment_status
FROM bookings AS b
JOIN users AS u
    ON b.user_id = u.user_id
JOIN shows AS sh
    ON b.show_id = sh.show_id
JOIN movies AS m
    ON sh.movie_id = m.movie_id
JOIN screens AS s
    ON sh.screen_id = s.screen_id
JOIN theatres AS t
    ON s.theatre_id = t.theatre_id
LEFT JOIN payments AS p
    ON b.booking_id = p.booking_id
ORDER BY b.booking_id;
```

### 10. Find Confirmed Bookings per User

```sql
SELECT
    u.user_id,
    u.user_name,
    SUM(b.total_seats) AS total_tickets
FROM users AS u
JOIN bookings AS b
    ON u.user_id = b.user_id
WHERE b.booking_status = 'Confirmed'
GROUP BY u.user_id, u.user_name
ORDER BY total_tickets DESC;
```

## 🎯 Project Objectives

- Design a relational database for a cinema booking workflow.
- Establish relationships between nine tables.
- Store and retrieve movie, theatre, seat, user, booking, and payment information.
- Practice SQL queries for reporting and analysis.
- Calculate payment revenue and summarize booking activity.
- Apply constraints to improve data integrity.
- Understand how JOINs and subqueries retrieve related information.

## 📚 Learning Outcomes

This project provides practical experience with:

- MySQL database creation and table design
- Primary and foreign key relationships
- SQL CRUD operations
- Filtering, sorting, and aggregation
- JOINs and subqueries
- Views and stored procedures
- Triggers and transaction control
- Indexes and query performance fundamentals
- Database testing and error debugging
- Relational database modeling

## 🔮 Future Enhancements

- Develop a Python or Django web application.
- Build a user login and authentication system.
- Prevent double-booking of the same seat for a show.
- Add a seat availability checking feature.
- Integrate a payment gateway in a future application.
- Create a Power BI dashboard for booking and revenue analysis.
- Add automated booking confirmation and cancellation workflows.
- Improve query performance through testing and indexing.

## 📁 Project Structure

```text
Movie-Ticket-Booking-System/
│
├── README.md
│
├── Database/
│   ├── 01_Create_Database.sql
│   ├── 02_Create_Tables.sql
│   ├── 03_Insert_Movies.sql
│   ├── 04_Insert_Theatres.sql
│   ├── 05_Insert_Screens.sql
│   ├── 06_Insert_Seats.sql
│   ├── 07_Insert_Users.sql
│   ├── 08_Insert_Shows.sql
│   ├── 09_Insert_Bookings.sql
│   ├── 10_Insert_Booking_Seats.sql
│   └── 11_Insert_Payments.sql
│
├── SQL-Queries/
│   ├── 01_Basic-Queries.sql
│   ├── 02_Intermediate-Queries.sql
│   ├── 03_Advanced-Queries.sql
│   └── 04_Reports.sql
│
├── Database-Objects/
│   ├── Views.sql
│   ├── Stored-Procedures.sql
│   ├── Triggers.sql
│   ├── Transactions.sql
│   └── Indexes.sql
│
├── ER-Diagram/
│   └── Movie-Ticket-Booking-ER-Diagram.png
│
└── Screenshots/
    ├── Database.png
    ├── Tables.png
    ├── Booking-Report.png
    └── Revenue-Report.png
```

*Adjust the folder names and filenames to match the files actually present in your repository.*

## ⚙️ How to Run the Project

### Step 1: Install MySQL

Install MySQL Server and MySQL Workbench.

### Step 2: Create the Database

```sql
CREATE DATABASE movie_ticket_booking;
USE movie_ticket_booking;
```

If the database already exists, select it instead of recreating it.

### Step 3: Create the Tables

Run the table creation script to create all nine tables with their primary keys, foreign keys, and constraints.

### Step 4: Insert Sample Data

Run the relevant data insertion scripts or import your generated test data.

**Important:** Insert parent-table data before child-table data to avoid foreign-key errors.

A suitable order is:

1. `movies`
2. `theatres`
3. `screens`
4. `seats`
5. `users`
6. `shows`
7. `bookings`
8. `booking_seats`
9. `payments`

### Step 5: Execute SQL Queries

Run the basic, intermediate, advanced, and reporting queries in MySQL Workbench.

### Step 6: Verify the Results

Check table row counts, validate relationships, and confirm that the booking and payment reports return the expected results.

## 🏁 Project Conclusion

The Movie Ticket Booking System is a MySQL-based database project that demonstrates relational database design and practical SQL skills. It organizes movie schedules, theatre screens, seats, customers, bookings, and payment information into related tables.

The project provides practice in retrieving related data, analyzing booking activity, calculating successful payment revenue, and generating reports using SQL. It also establishes a foundation for future development into a Python-based web application.

---

**Project:** Movie Ticket Booking System  
**Category:** SQL / Database Management  
**Database:** MySQL  
**Tools:** MySQL Workbench, Git, GitHub
