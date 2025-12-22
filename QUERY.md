# Sample SQL Queries - Expected Output

This document provides sample data and the expected outputs for the SQL queries defined in the assignment.

## Sample Data (Input)

### Users Table
| user_id | name | email | phone | role |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Alice | alice@example.com | 1234567890 | Customer |
| 2 | Bob | bob@example.com | 0987654321 | Admin |
| 3 | Charlie | charlie@example.com | 1122334455 | Customer |

### Vehicles Table
| vehicle_id | name | type | model | registration_number | rental_price | status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Toyota Corolla | car | 2022 | ABC-123 | 50 | available |
| 2 | Honda Civic | car | 2021 | DEF-456 | 60 | rented |
| 3 | Yamaha R15 | bike | 2023 | GHI-789 | 30 | available |
| 4 | Ford F-150 | truck | 2020 | JKL-012 | 100 | maintenance |

### Bookings Table
| booking_id | user_id | vehicle_id | start_date | end_date | status | total_cost |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | 2 | 2023-10-01 | 2023-10-05 | completed | 240 |
| 2 | 1 | 2 | 2023-11-01 | 2023-11-03 | completed | 120 |
| 3 | 3 | 2 | 2023-12-01 | 2023-12-02 | confirmed | 60 |
| 4 | 1 | 1 | 2023-12-10 | 2023-12-12 | pending | 100 |

---

## Query

### Query 1: JOIN
**Requirement**: Retrieve booking information along with Customer name and Vehicle name.
**Answer**:

```
SELECT
  b.booking_id,
  u.name as customer_name,
  v.name as vehicle_name,
  b.start_date,
  b.end_date,
  b.status 

FROM bookings b
JOIN users u on b.user_id = u.user_id
JOIN vehicles v on b.vehicle_id = v.vehicle_id 
  
ORDER BY b.booking_id;
```

**Output**:
| booking_id | customer_name | vehicle_name | start_date | end_date | status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Sayed | Toyota Crolla | 2023-10-01 | 2023-10-05 | completed |
| 2 | Jayed | Honda Civic | 2023-10-10 | 2023-10-12 | completed |
| 3 | Fatima | Yamaha R15 | 2023-11-01 | 2023-11-02 | confirmed |
| 4 | Omar | Tesla Model 3 | 2023-11-05 | 2023-11-07 | completed |
| 5 | Omar | Chevrolet Silverado | 2023-12-01 | 2023-12-05 | confirmed |
| 6 | Hasan | Mercedes Benz Sprinter | 2023-12-15 | 2023-12-20 | confirmed |
| 7 | Mariam | Vespa Primavera | 2023-12-22 | 2023-12-23 | pending |
| 8 | Yusuf | Ford F-150 | 2023-12-24 | 2023-12-26 | cancelled |
| 9 | Layla | Kawasaki Ninja | 2023-11-05 | 2023-11-07 | pending |
| 10 | Ahmed | BMW 3 Series | 2024-01-05 | 2024-01-07 | pending |
---

### Query 2: EXISTS
**Requirement**: Find all vehicles that have never been booked.
**Answer**:

```
SELECT *
FROM vehicles v
WHERE NOT EXISTS (
    SELECT *
    FROM vehicles v2 
    WHERE v2.vehicle_id = v.vehicle_id 
    AND v2.status = 'rented'
);
```

**Output**:
| vehicle_id | name | type | model | registration_number | rental_price | status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Toyota Corolla | car | 2022 | ABC-123 | 50 | available |
| 3 | Yamaha R15 | bike | 2023 | GHI-789 | 30 | available |
| 4 | Ford F-150 | truck | 2020 | JKL-012 | 100 | maintenance |
| 5 | Tesla Model 3 | car | 2023 | MNO-345 | 120 | available |
| 7 | Chevrolet Silverado | truck | 2021 | STU-901 | 110 | available | 
| 8 | BMW 3 Series | car | 2022 | VWX-234 | 90 | maintenance | 
| 9 | Vespa Primavera | bike | 2024 | YZA-567 | 25 | available | 
| 10 | Mercedes Benz Sprinter | truck | 2023 | BCD-890 | 150 | available |

---

### Query 3: WHERE
**Requirement**: Retrieve all available vehicles of a specific type (e.g. cars).
**Answer**:

```
SELECT *  
FROM vehicles v WHERE type = 'car'
AND status = 'available';
```

**Output**:
| vehicle_id | name | type | model | registration_number | rental_price | status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Toyota Corolla | car | 2022 | ABC-123 | 50 | available |
| 5 | Tesla Model 3 | car | 2023 | MNO-345 | 120 | available |

---

### Query 4: GROUP BY and HAVING
**Requirement**: Find the total number of bookings for each vehicle and display only those vehicles that have more than 2 bookings.
**Answer**:

```
SELECT v.name as vehicle_name,
COUNT(b.booking_id) as total_booking
FROM vehicles v 
JOIN bookings b on v.vehicle_id = b.vehicle_id
GROUP BY v.name
HAVING COUNT(b.booking_id) > 2 ;
```

**Expected Output**:
| vehicle_name | total_bookings |
| :--- | :--- |
| Honda Civic | 3 |
| Toyota Corolla | 3 |