# sql-prep


1. Select date and temp, which is having more temparature than the previous date.\
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
