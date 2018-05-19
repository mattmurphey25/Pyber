

```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```


```python
#Define Path
city_path = 'raw_data/city_data.csv'
ride_path = 'raw_data/ride_data.csv'

#Read CSV
city_df = pd.read_csv(city_path)
ride_df = pd.read_csv(ride_path)
```


```python
#Pull data for fares
city_fare_data = {"city": ride_df["city"], "Average Fare": ride_df["fare"]}

#Put data in DataFrame
city_fare_df = pd.DataFrame(data=city_fare_data)
```


```python
#Group DataFrame by city
city_fare_group = city_fare_df.groupby(["city"])

#Take averages for each city
city_fare_mean = city_fare_group.mean()
```


```python
#Pull data for rides
city_ride_data = {"city": ride_df["city"], "Total Rides": ride_df["ride_id"]}

#Put data in DataFrame
city_ride_df = pd.DataFrame(data=city_ride_data)
```


```python
#Group DataFrame by city
city_ride_group = city_ride_df.groupby(["city"])

#Count the rides for each city
city_ride_count = city_ride_group.count()
```


```python
#Merge fare and ride DataFrames on city
ride_fare_merge = pd.merge(city_fare_mean, city_ride_count, left_index=True, right_index=True)
```


```python
#Merge previous merge with the city data
city_merge = pd.merge(city_df, ride_fare_merge, left_on="city", right_index=True)
```


```python
#Get urban data for bubble chart
urban_df = city_merge.groupby(['type']).get_group('Urban')

#Get suburban data for bubble chart
suburban_df = city_merge.groupby(['type']).get_group('Suburban')

#Get rural data for bubble chart
rural_df = city_merge.groupby(['type']).get_group('Rural')
```


```python
#Urban bubble plot
urban = plt.scatter(urban_df["Total Rides"], urban_df["Average Fare"], s=3*urban_df["driver_count"], c="lightcoral", alpha=.8, linewidths=.5, edgecolors="black", label="Urban")

#suburban bubble plot
suburban = plt.scatter(suburban_df["Total Rides"], suburban_df["Average Fare"], s=3*suburban_df["driver_count"], c="lightskyblue", alpha=.8, linewidths=.5, edgecolors="black", label="Suburban")

#rural bubble plot
rural = plt.scatter(rural_df["Total Rides"], rural_df["Average Fare"], s=3*rural_df["driver_count"], c="gold", alpha=.8, linewidths=.5, edgecolors="black", label="Rural")

#Set the upper and lower limits of the y axis
plt.ylim(17,50)

#Set the upper and lower limits of the x axis
plt.xlim(0,35)

#Create a title, x label, and y label for bubble chart
plt.title("Pyber Ride Sharing Data (2016)")
plt.xlabel("Total Number of Riders (Per City)")
plt.ylabel("Average Fare ($)")

#Create a legend
plt.legend(loc="best", title="City Type")

#Place a grid
plt.grid()

# Save an image of the chart and print to screen
plt.savefig("Images/Pyber_bubble.png")
plt.show()
print("Note: Size of the points correlates to driver count")
```


![png](output_9_0.png)


    Note: Size of the points correlates to driver count
    


```python
#Group city data by city type
driver_type_group = city_df.groupby(["type"])

#Take the sum of all the drivers by city type
driver_type_sum = driver_type_group.sum()
```


```python
#Set labels for driver_type pie chart
labels = ["Rural", "Suburban", "Urban"]

#Set the colors of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]

#set the urban slice to explode out
explode = (0, 0, 0.1)

#make driver_type pie chart
plt.pie(driver_type_sum["driver_count"], explode=explode, colors=colors, labels=labels,
        autopct="%1.1f%%", shadow=True, startangle=140)

#set driver_type title
plt.title("% of Total Drivers by City Type")

# Tell matplotlib to make a pie chart with equal axes
plt.axis("equal")

#Save image and print the pie chart to the screen
plt.savefig("Images/Pyber_driver_pie.png")
plt.show()
```


![png](output_11_0.png)



```python
#get data for ride_type and fare_type pie charts
ride_type_data = {"type": city_merge["type"], "Average Fare": city_merge["Average Fare"], "Total Rides": city_merge["Total Rides"]}

#put into DataFrame
ride_type_df = pd.DataFrame(data=ride_type_data)

#Calculat total fares
ride_type_df["Total Fare"] = ride_type_df["Average Fare"] * ride_type_df["Total Rides"]

#group DataFrame by city type
ride_type_group = ride_type_df.groupby(["type"])

#sum all the columns of each type
ride_type_sum = ride_type_group.sum()
```


```python
#Set labels for fare_type pie chart
labels = ["Rural", "Suburban", "Urban"]

#Set the colors of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]

#set the urban slice to explode out
explode = (0, 0, 0.1)

#make fare_type pie chart
plt.pie(ride_type_sum["Total Fare"], explode=explode, colors=colors, labels=labels,
        autopct="%1.1f%%", shadow=True, startangle=140)

#set fare_type title
plt.title("% of Total Fares by City Type")

#Tell matplotlib to make a pie chart with equal axes
plt.axis("equal")

#save image and Print the pie chart to the screen
plt.savefig("Images/Pyber_fare_pie.png")
plt.show()
```


![png](output_13_0.png)



```python
#Set labels for ride_type pie chart
labels = ["Rural", "Suburban", "Urban"]

#Set the colors of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]

#set the urban slice to explode out
explode = (0, 0, 0.1)

#make ride_type pie chart
plt.pie(ride_type_sum["Total Rides"], explode=explode, colors=colors, labels=labels,
        autopct="%1.1f%%", shadow=True, startangle=140)

#set ride_type title
plt.title("% of Total Rides by City Type")

#tell matplotlib to make a pie chart with equal axes
plt.axis("equal")

#save an image and print the pie chart to the screen
plt.savefig("Images/Pyber_ride_pie.png")
plt.show()
```


![png](output_14_0.png)

