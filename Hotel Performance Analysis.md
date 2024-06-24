# 🏨 Hotel-Performance-Project
Microsoft SQL Server was used on this project.

## Data Exploration

### Quick Insights

#### 1. Total Number of Bookings from 2018-2020
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	COUNT(*) AS total_bookings
FROM hotel_data
````
**Output:**

![1](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/fd86e8a8-5595-49d0-a74d-24afdc823946)

***

#### 2. Total Number of Bookings per Hotel Type
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	hotel,
	COUNT(*) AS total_bookings
FROM hotel_data
GROUP BY hotel
````
**Output:**

![2](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/77ac1e79-0e6f-49a8-b6b6-8819ecb4d322)

***

#### 3. Number of Bookings per Month 
To see which months have the highest and lowest bookings
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	arrival_date_month,
	COUNT(*) AS total_bookings
FROM hotel_data
GROUP BY arrival_date_month
ORDER BY total_bookings DESC
````
**Output:**

![3](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/0af75f7a-2a4c-4236-9716-2585592dfd7d)

***

#### 4. Distribution of Guest Nationalities (Top 20)
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	TOP 20 country,
	COUNT(*) AS guest_count
FROM hotel_data
GROUP BY country
ORDER BY guest_count DESC
````
**Output:**

![4](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/001adee7-44e9-491d-8533-1ef0d4f96eb0)

***

#### 5. Repeat Guests vs. New Guests
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	is_repeated_guest,
	COUNT(*) AS guest_count
FROM hotel_data
GROUP BY is_repeated_guest
````
**Output:**

![5](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/a2449b53-a4bd-4395-8047-912e9085eb49)

***

#### 6. Average Daily Rate (ADR) per Month
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	arrival_date_month,
	ROUND(AVG(adr),2) AS average_adr
FROM hotel_data
GROUP BY arrival_date_month
ORDER BY 
	CASE arrival_date_month
		WHEN 'January' THEN 1
		WHEN 'February' THEN 2
		WHEN 'March' THEN 3
		WHEN 'April' THEN 4
		WHEN 'May' THEN 5
		WHEN 'June' THEN 6
		WHEN 'July' THEN 7
		WHEN 'August' THEN 8
		WHEN 'September' THEN 9
		WHEN 'October' THEN 10
		WHEN 'November' THEN 11
		WHEN 'December' THEN 12
		ELSE 0
	END
````
**Output:**

![6](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/e988fa88-d7b8-42fa-9115-9d66301be1c9)

***

#### 7. Revenue per Hotel Type (Discount amount and Meal pricing not Accounted)
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	hotel,
	SUM((stays_in_week_nights+stays_in_weekend_nights)*adr) AS revenue
FROM hotel_data
GROUP BY hotel
````
**Output:**

![7](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/cf6e8029-c011-4e4f-8fbc-5781342aa7a8)

***

#### 8. Revenue per Year
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	arrival_date_year,
	ROUND(SUM((stays_in_week_nights+stays_in_weekend_nights)*adr),2) AS revenue
FROM hotel_data
GROUP BY arrival_date_year
````
**Output:**

![8](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/2842820c-ad99-45de-b361-e72a01eddcc2)

***

#### 9. Most Booked Room Type
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT
	reserved_room_type,
	COUNT(*) as number_of_bookings
FROM hotel_data
GROUP BY reserved_room_type
ORDER BY number_of_bookings DESC
````
**Output:**

![9](https://github.com/colinryanx/Hotel-Performance-Project/assets/171652558/869cf0c3-7fff-45d0-b35f-43fbc42a7951)

***

### Combine ALL Tables to a single table for connection to Power BI
#### 10. Denormalize tables to optimize data retrieval and improve performance
````sql
WITH hotel_data AS (
	SELECT * FROM dbo.[hotel2018]
	UNION
	SELECT * FROM dbo.[hotel2019]
	UNION
	SELECT * FROM dbo.[hotel2020]
	)
SELECT *
FROM hotel_data
LEFT JOIN dbo.[hotel_market_segment]
ON hotel_data.market_segment = dbo.[hotel_market_segment].[market_segment]
LEFT JOIN dbo.[hotel_meal_cost]
ON hotel_data.meal = dbo.[hotel_meal_cost].[meal]
````

***
