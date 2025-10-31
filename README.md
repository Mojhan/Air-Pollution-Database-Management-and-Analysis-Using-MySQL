# Air-Pollution-Database-Management-and-Analysis-Using-MySQL
Designed and implemented a relational database to store and analyze air quality data for multiple cities. Applied MySQL queries for data cleaning, aggregation, and trend identification based on pollutant levels.

-- Create City table
CREATE TABLE City (
    city_id INT PRIMARY KEY,
    city_name VARCHAR(100),
    state VARCHAR(100),
    population INT
);

-- Create AirQuality table
CREATE TABLE AirQuality (
    record_id INT PRIMARY KEY,
    city_id INT,
    date_recorded DATE,
    pm25 FLOAT,
    pm10 FLOAT,
    no2 FLOAT,
    so2 FLOAT,
    co FLOAT,
    o3 FLOAT,
    FOREIGN KEY (city_id) REFERENCES City(city_id)
);
INSERT INTO City VALUES
(1, 'Delhi', 'Delhi', 19000000),
(2, 'Mumbai', 'Maharashtra', 12500000),
(3, 'Chennai', 'Tamil Nadu', 10000000),
(4, 'Bengaluru', 'Karnataka', 9500000);

INSERT INTO AirQuality VALUES
(1, 1, '2025-01-01', 210, 180, 45, 22, 1.5, 30),
(2, 2, '2025-01-01', 120, 95, 30, 18, 1.0, 35),
(3, 3, '2025-01-01', 80, 65, 25, 12, 0.8, 40),
(4, 4, '2025-01-01', 90, 75, 28, 15, 0.9, 38),
(5, 1, '2025-02-01', 190, 170, 40, 20, 1.4, 33),
(6, 2, '2025-02-01', 110, 90, 29, 17, 1.1, 36),
(7, 3, '2025-02-01', 75, 60, 23, 10, 0.7, 42),
(8, 4, '2025-02-01', 85, 70, 26, 13, 0.8, 39);
SELECT * FROM AirQuality;
SELECT 
    c.city_name,
    ROUND(AVG(a.pm25), 2) AS avg_pm25
FROM AirQuality a
JOIN City c ON a.city_id = c.city_id
GROUP BY c.city_name
ORDER BY avg_pm25 DESC;
SELECT 
    c.city_name,
    ROUND(AVG(a.pm25), 2) AS avg_pm25
FROM AirQuality a
JOIN City c ON a.city_id = c.city_id
GROUP BY c.city_name
ORDER BY avg_pm25 DESC
LIMIT 1;
SELECT 
    c.city_name,
    ROUND(AVG(a.pm25), 2) AS avg_pm25
FROM AirQuality a
JOIN City c ON a.city_id = c.city_id
GROUP BY c.city_name
ORDER BY avg_pm25 ASC
LIMIT 1;
SELECT 
    c.city_name, a.date_recorded, a.pm25
FROM AirQuality a
JOIN City c ON a.city_id = c.city_id
WHERE a.pm25 > 150
ORDER BY a.pm25 DESC;
SELECT 
    c.city_name,
    DATE_FORMAT(a.date_recorded, '%Y-%m') AS month,
    ROUND(AVG((a.pm25 + a.pm10 + a.no2 + a.so2 + a.co + a.o3)/6), 2) AS avg_aqi
FROM AirQuality a
JOIN City c ON a.city_id = c.city_id
GROUP BY c.city_name, month
ORDER BY month;
SELECT 
    c.city_name,
    ROUND(AVG(a.pm25), 2) AS avg_pm25,
    RANK() OVER (ORDER BY AVG(a.pm25) DESC) AS pollution_rank
FROM AirQuality a
JOIN City c ON a.city_id = c.city_id
GROUP BY c.city_name;
