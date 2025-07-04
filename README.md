# sql-prep


### 1. Select date and temp, which is having more temparature than the previous date.
temp_i | date     | temp
-------|----------|--------
1      | 1-6-2025 | 15
2      | 2-6-2025 | 20
3      | 3-6-2025 | 12
4      | 4-6-2025 | 25

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


### 3. Write sql to get the desired output - 

code   | event 
-------|------
com1   | sent  
com2   | open
com3   | sent
com1   | bounced
com3   | bounced
com2   | sent
com2   | sent
com1   | bounced


SELECT \
    code,\
    COUNT(CASE WHEN event = 'sent' THEN 1 END) AS sent,\
    COUNT(CASE WHEN event = 'open' THEN 1 END) AS open,\
    COUNT(CASE WHEN event = 'bounced' THEN 1 END) AS bounced\
FROM your_table_name\
GROUP BY code\
ORDER BY code;


output 
-----------------------------
code   | sent | open | bounced
-------|------|------|--------
com1   | 1    | 0    | 2
com2   | 2    | 1    | 0
com3   | 1    | 0    | 1
