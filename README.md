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


```
import pymysql
import pandas as pd
import seaborn as sb
import matplotlib.pyplot as plt

# Connect to the database
connection = pymysql.connect(host='localhost', user='root', password='root', database='nyc_taxi_data')

try:
    with connection.cursor() as cursor:
        # Execute a query
        cursor.execute("SELECT HOUR(tpep_pickup_datetime), COUNT(*) from nyctaxidata GROUP BY HOUR(tpep_pickup_datetime) ORDER BY COUNT(*) DESC")
        
        
        result = cursor.fetchall()
        for row in result:
            print(row)
finally:
    connection.close()
```
### 17H and 18H are the busiest hours of the days

```
import pandas as pd
import matplotlib.pyplot as plt
import pymysql


connection = pymysql.connect(host='localhost', user='root', password='root', database='nyc_taxi_data')




try:
    with connection.cursor() as cursor:
       
        cursor.execute("SELECT total_amount from nyctaxidata WHERE fare_amount > 0")
        
        
        result = cursor.fetchall()
        
        
        
        df = pd.DataFrame(result, columns=['total_amount'])
        
        frequency = df['total_amount'].value_counts()

        plt.figure(figsize=(10, 6))
        plt.bar(frequency.index, frequency.values, color='blue', edgecolor='purple')
        plt.title('Distribution of Total Fare Amounts')
        plt.xlabel('Total Amount')
        plt.ylabel('Frequency')
        plt.grid(axis='y', alpha=0.7)
        plt.show()



finally:
    connection.close()
```
![]
