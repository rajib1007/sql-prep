# sql-prep


### 1. Select date and temp, which is having more temparature than the previous date.\
temp_id, date, temp\
1,1-6-2025,15\
2,2-6-2025,20\
3,3-6-2025,12\
4,4-6-2025,25

SELECT 
    temp_id,
    date,
    temp
FROM (\
    SELECT 
        temp_id,
        date,
        temp,\
        LAG(temp) OVER (ORDER BY TO_DATE(date, 'DD-MM-YYYY')) AS prev_temp\
    FROM temperature_table\
) t\
WHERE temp > prev_temp;


### 2. Generate 1 to 10 sequence 


✅ Standard SQL (Using Recursive CTE):

WITH RECURSIVE seq AS (\
    SELECT 1 AS num\
    UNION ALL\
    SELECT num + 1 FROM seq WHERE num < 10\
)\
SELECT num FROM seq;

✅ PostgreSQL / Snowflake:

SELECT * FROM GENERATE_SERIES(1, 10);

or in Snowflake (no generate_series):

SELECT ROW_NUMBER() OVER () AS num\
FROM TABLE(GENERATOR(ROWCOUNT => 10));
