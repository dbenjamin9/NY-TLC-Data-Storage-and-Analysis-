# NY-TLC-Data-Storage-and-Analysis-
[Click here to download the dataset](https://drive.google.com/drive/u/0/folders/19D0EgpxLh2d6WTbw8Pjp8kW2puOCw5yk)

### **Problem 1: What is the average trip distance?**
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

### **Problem 2: What is the relationship between trip distance and fare amount?**

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

### **Problem 3: What are the busiest hours for taxi trips?**
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

### **Problem 4: What is the distribution of total fare amounts?**
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
![](https://github.com/dbenjamin9/NY-TLC-Data-Storage-and-Analysis-/blob/main/img2%20ipy.png)

### **Problem 5: What is the average trip duration?**
```
import numpy as np 
import pandas as pd
import seaborn as sb 
import matplotlib.pyplot as plt

import pymysql

connection = pymysql.connect(host= 'localhost', user='root', password='root', database='nyc_taxi_data')

try:
    with connection.cursor() as cursor:
        cursor.execute("SELECT tpep_dropoff_datetime, tpep_pickup_datetime FROM nyctaxidata")
        
        result = cursor. fetchall()
        df = pd.DataFrame(result, columns=['tpep_dropoff_datetime', 'tpep_pickup_datetime'])
        df['total_duration'] = (df['tpep_dropoff_datetime'] - df['tpep_pickup_datetime']).dt.total_seconds () / 60
        average_duration = df['total_duration'].mean()
        print(f"Average trip duration: {average_duration}")

finally:
    connection.close()

```

Average trip duration: 15.66
