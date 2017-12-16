

```python
#########################PLEASE NOTE DYLAN IS OK FOR ME TO USE SEABORN - RAJAT #####################
#Import Dependencies
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
#seaborn settings
sns.set_palette("deep",desat=.6)
sns.set_context(rc={"figure:figsize":(8,4)})
```


```python
city_df=pd.read_csv("./city_data.csv")
```


```python
ride_df=pd.read_csv("./ride_data.csv")
```


```python
city_df.shape
```




    (126, 3)




```python
ride_df.shape
```




    (2375, 4)




```python
x=city_df["city"].unique().tolist()
y=ride_df["city"].unique().tolist()
```


```python
len(x)
```




    125




```python
len(y)
```




    125




```python
# merging the 2 datasets
city_ride=pd.merge(city_df,ride_df,on='city')
```


```python
city_ride.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_ride=city_ride[["city","date","fare","ride_id","driver_count","type"]]
```


```python
#city_ride.head()
```


```python
#Buble plot of ride sharing data
#df.plot.scatter(x='a', y='b', s=df['c']*200);
#Your objective is to build a Bubble Plot that showcases the relationship between four key variables:
#y=Average Fare ($) Per City
#x=Total Number of Rides Per City
#buble_size=Total Number of Drivers Per City
#colors=City Type (Urban, Suburban, Rural)
```


```python
#create data
city_ride_group=city_ride.groupby(["city","driver_count","type"])
x_axis=city_ride_group["ride_id"].count()
y_axis=city_ride_group["fare"].mean()
#z_axis=city_ride.set_index('city')['driver_count']
#z_axis=city_ride.groupby('driver_count')
```


```python
df_per_city=pd.DataFrame({"Rides":x_axis,
                          "Average fare":y_axis,
                         #"Drivers Count":z_axis
                         })
df_per_city=df_per_city.reset_index()
df_per_city.max()
```




    city            Zimmermanmouth
    driver_count                73
    type                     Urban
    Average fare             49.62
    Rides                       34
    dtype: object




```python
#x1_axis=df_per_city[df_per_city['type'] == "Rural"]["Rides"]
#y1_axis=df_per_city[df_per_city['type'] == "Rural"]["Average fare"]
#z1_axis=df_per_city[df_per_city['type'] == "Rural"]["driver_count"]
#df_per_city.plot(kind='scatter',x=df_per_city["Rides"],y=df_per_city["Average fare"],s=df_per_city["driver_count"])
colors={"Urban":'lightskyblue',"Suburban":'lightcoral',"Rural":'gold'}
#sns.lmplot('Rides','Average fare',data=df_per_city,fit_reg=False,hue='type',palette=colors,size=5)
bins=[0,30,60,80]
sizes=[100,200,300]
driver_size=pd.cut(df_per_city['driver_count'],bins,labels=sizes)
sns.lmplot('Rides','Average fare',data=df_per_city,fit_reg=False,hue='type',palette=colors,size=6,scatter_kws={"s":driver_size})
```




    <seaborn.axisgrid.FacetGrid at 0x1fd5bb39048>




```python
# plot with the seaborn style
#ax=plt.scatter(x1_axis, y1_axis, s=z1_axis, c="gold", alpha=0.6, linewidth=6, label='Rural')

#x2_axis=df_per_city[df_per_city['type'] == "Rural"]["Rides"]
#y2_axis=df_per_city[df_per_city['type'] == "Rural"]["Average fare"]
#z2_axis=df_per_city[df_per_city['type'] == "Rural"]["driver_count"]
#plt.scatter(x2_axis, y2_axis, s=z2_axis, c="blue", alpha=0.6, linewidth=6, label='Urban')
# Add titles (main and on axis)
plt.xlabel("Total number of rides per city")
plt.ylabel("Average Fares($)")
plt.title("Pyber Ride Sharing Data(2016)", loc="center")
plt.show()
```


![png](README_files/README_17_0.png)



```python
### Pie chart for In addition, you will be expected to produce the following three pie charts:
#% of Total Fares by City Type
#% of Total Rides by City Type
#% of Total Drivers by City Type
```


```python
## Preparing data set for piecharts
city_ride.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
group_by_citytype=city_ride.groupby('type')
```


```python
fares_by_citytype=group_by_citytype["fare"].sum()
rides_by_citytype=group_by_citytype["ride_id"].count()
drivers_by_citytype=group_by_citytype["driver_count"].sum()
df_city_ride=pd.DataFrame({"fare":fares_by_citytype,
                          "rides":rides_by_citytype,
                        "drivers":drivers_by_citytype
                          })
```


```python
df_city_ride
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>drivers</th>
      <th>fare</th>
      <th>rides</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>727</td>
      <td>4255.09</td>
      <td>125</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>9730</td>
      <td>20335.69</td>
      <td>657</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>64501</td>
      <td>40078.34</td>
      <td>1625</td>
    </tr>
  </tbody>
</table>
</div>




```python
######################## define function
```


```python
def plot_pie(sizes,labels,explode,colors):
     plt.pie(sizes, explode=explode, labels=labels, colors=colors,autopct="%1.1f%%", shadow=True, startangle=140)
     plt.axis("equal")
```


```python
# ###################3% of Total Fares by City Type#################
labels=["Rural","Suburban","Urban"]
```


```python
sizes=df_city_ride['fare'].round(2).tolist()
```


```python
colors = ["gold","lightcoral", "lightskyblue"]
```


```python
explode=(0.1,0.1,0)
```


```python
#plt.pie(sizes_fares, explode=explode, labels=labels, colors=colors,autopct="%1.1f%%", shadow=True, startangle=140)
plot_pie(sizes,labels,explode,colors)
#Title
plt.title("% of Total Fares by City Type")
```




    Text(0.5,1,'% of Total Fares by City Type')




```python
# Prints our pie chart to the screen
plt.show()
```


![png](README_files/README_30_0.png)



```python
##########################% of Total Rides by City Type########################
#labels defined above
sizes=df_city_ride['rides'].round(2).tolist()
```


```python
colors = ["gold","lightcoral", "lightskyblue"]
explode=(0.1,0.1,0)
```


```python
# calling function
plot_pie(sizes,labels,explode,colors)
# Titile
plt.title("% of Total Rides by City Type")
```




    Text(0.5,1,'% of Total Rides by City Type')




```python
# Prints our pie chart to the screen
plt.show()
```


![png](README_files/README_34_0.png)



```python
####################################% of Total Drivers by City Type############################
#labels defined above
```


```python
sizes=df_city_ride['drivers'].round(2).tolist()
colors = ["gold","lightcoral", "lightskyblue"]
explode=(0.1,0.1,0)
```


```python
#calling function
plot_pie(sizes,labels,explode,colors)
# Title
plt.title("% of Total Drives by City Type")
```




    Text(0.5,1,'% of Total Drives by City Type')




```python
# Prints our pie chart to the screen
plt.show()
```


![png](README_files/README_38_0.png)

