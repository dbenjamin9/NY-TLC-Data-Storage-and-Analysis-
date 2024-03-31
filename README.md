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
Answer: 3.847343

```
## Your Code here
import pymysql
import matplotlib.pyplot as plt

# Connect to the database: These credentials will change whether you are connecting to your local db or via GCloud db
connection = pymysql.connect(host='localhost', user='root', password='root', database='nyc_taxi_data')

try:
    with connection.cursor() as cursor:
        # Executing a query
        cursor.execute("SELECT trip_distance, fare_amount FROM nyctaxidata WHERE fare_amount > 0 AND trip_distance < 300")
        
        # Fetch all rows from the last executed query
        result = cursor.fetchall()
        
        trip_distance = [row[0] for row in result]
        fare_amount =  [row[1] for row in result]
finally:
    connection.close()
plt.figure(figsize=(10, 6))
plt.scatter(trip_distance, fare_amount)
plt.title('Relationship between Trip Distance and Fare Amount')
plt.xlabel('Trip Distance (miles)')
plt.ylabel('Fare Amount ($)')
plt.show()

```

![](https://github.com/dbenjamin9/NY-TLC-Data-Storage-and-Analysis-/blob/main/ipy%20image1.png)


