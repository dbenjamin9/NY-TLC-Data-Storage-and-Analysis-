# NY-TLC-Data-Storage-and-Analysis-
```
import pymysql

connection = pymysql.connect(host='localhost', user='root', password='root', database='nyc_taxi_data')

try:
    with connection.cursor() as cursor:
        # Execute a query
        cursor.execute("SELECT AVG(trip_distance) from nyctaxidata")
        
        # Fetch all rows from the last executed query
        result = cursor.fetchall()
        
        for row in result:
            print(float(row[0]))
finally:
    connection.close()
```
