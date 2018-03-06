
## Unit 5 | Assignment - The Power of Plots

## Background

What good is data without a good plot to tell the story?

So, let's take what you've learned about Python Matplotlib and apply it to some real-world situations. For this assignment, you'll need to complete **1 of 2** Data Challenges. As always, it's your choice which you complete. _Perhaps_, choose the one most relevant to your future career.

## Option 1: Pyber

![Ride](Images/Ride.png)

The ride sharing bonanza continues! Seeing the success of notable players like Uber and Lyft, you've decided to join a fledgling ride sharing company of your own. In your latest capacity, you'll be acting as Chief Data Strategist for the company. In this role, you'll be expected to offer data-backed guidance on new opportunities for market differentiation.

You've since been given access to the company's complete recordset of rides. This contains information about every active driver and historic ride, including details like city, driver count, individual fares, and city type.

Your objective is to build a [Bubble Plot](https://en.wikipedia.org/wiki/Bubble_chart) that showcases the relationship between four key variables:

* Average Fare ($) Per City
* Total Number of Rides Per City
* Total Number of Drivers Per City
* City Type (Urban, Suburban, Rural)

In addition, you will be expected to produce the following three pie charts:

* % of Total Fares by City Type
* % of Total Rides by City Type
* % of Total Drivers by City Type

As final considerations:

* You must use the Pandas Library and the Jupyter Notebook.
* You must use the Matplotlib and Seaborn libraries.
* You must include a written description of three observable trends based on the data.
* You must use proper labeling of your plots, including aspects like: Plot Titles, Axes Labels, Legend Labels, Wedge Percentages, and Wedge Labels.
* Remember when making your plots to consider aesthetics!
  * You must stick to the Pyber color scheme (Gold, Light Sky Blue, and Light Coral) in producing your plot and pie charts.
  * When making your Bubble Plot, experiment with effects like `alpha`, `edgecolor`, and `linewidths`.
  * When making your Pie Chart, experiment with effects like `shadow`, `startangle`, and `explosion`.
* You must include an exported markdown version of your Notebook called  `README.md` in your GitHub repository.
* See [Example Solution](Pyber/Pyber_Example.pdf) for a reference on expected format.



```python
import matplotlib.pyplot as plt 
import numpy as np
import pandas as pd 
pd.options.display.float_format = '${:,.2f}'.format

```


```python
# Read csv file for City Data into DataFrame
city_data_path = 'raw_data/city_data.csv'

city_data_df = pd.read_csv(city_data_path)
city_data_df.head()

# 126 rows


```


```python
# Read csv file for Ride Data into DataFrame
ride_data_path = 'raw_data/ride_data.csv'

ride_data_df = pd.read_csv(ride_data_path)
ride_data_df.head()

#2375 rows
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>$38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>$17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>$44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>$6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>$6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Merge City and Ride data 
city_ride_table = pd.merge(ride_data_df, city_data_df, on="city")
city_ride_table.count()
#city_ride_table.head()
#2407

```




    city            2407
    date            2407
    fare            2407
    ride_id         2407
    driver_count    2407
    type            2407
    dtype: int64




```python
#Assign colors to their respective city type
conditions = [
    (city_data_df['type'] == 'Urban'),
    (city_data_df['type'] == 'Suburban'),
    (city_data_df['type'] == 'Rural')]
choices = ['lightcoral', 'lightskyblue', 'gold']
city_data_df['plot_color'] = np.select(conditions, choices, default='black')
print(city_data_df['plot_color'])
#city_ride_table

```

    0        lightcoral
    1        lightcoral
    2        lightcoral
    3        lightcoral
    4        lightcoral
    5        lightcoral
    6        lightcoral
    7        lightcoral
    8        lightcoral
    9        lightcoral
    10       lightcoral
    11       lightcoral
    12       lightcoral
    13       lightcoral
    14       lightcoral
    15       lightcoral
    16       lightcoral
    17       lightcoral
    18       lightcoral
    19       lightcoral
    20       lightcoral
    21       lightcoral
    22       lightcoral
    23       lightcoral
    24       lightcoral
    25       lightcoral
    26       lightcoral
    27       lightcoral
    28       lightcoral
    29       lightcoral
               ...     
    96     lightskyblue
    97     lightskyblue
    98     lightskyblue
    99     lightskyblue
    100    lightskyblue
    101    lightskyblue
    102    lightskyblue
    103    lightskyblue
    104    lightskyblue
    105    lightskyblue
    106    lightskyblue
    107    lightskyblue
    108            gold
    109            gold
    110            gold
    111            gold
    112            gold
    113            gold
    114            gold
    115            gold
    116            gold
    117            gold
    118            gold
    119            gold
    120            gold
    121            gold
    122            gold
    123            gold
    124            gold
    125            gold
    Name: plot_color, Length: 126, dtype: object
    


```python
ride_fare_group = city_ride_table.groupby("fare")
ride_fare_group.sum()
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
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>fare</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>$4.05</th>
      <td>627410504531</td>
      <td>22</td>
    </tr>
    <tr>
      <th>$4.06</th>
      <td>5772409201337</td>
      <td>55</td>
    </tr>
    <tr>
      <th>$4.07</th>
      <td>6100187302721</td>
      <td>21</td>
    </tr>
    <tr>
      <th>$4.13</th>
      <td>8152035211745</td>
      <td>9</td>
    </tr>
    <tr>
      <th>$4.15</th>
      <td>5569007322768</td>
      <td>15</td>
    </tr>
    <tr>
      <th>$4.16</th>
      <td>9927742208390</td>
      <td>15</td>
    </tr>
    <tr>
      <th>$4.20</th>
      <td>2476311410315</td>
      <td>11</td>
    </tr>
    <tr>
      <th>$4.23</th>
      <td>8839515131785</td>
      <td>12</td>
    </tr>
    <tr>
      <th>$4.26</th>
      <td>297870556755</td>
      <td>11</td>
    </tr>
    <tr>
      <th>$4.27</th>
      <td>6739162487056</td>
      <td>8</td>
    </tr>
    <tr>
      <th>$4.36</th>
      <td>74296958741</td>
      <td>25</td>
    </tr>
    <tr>
      <th>$4.43</th>
      <td>8197308953793</td>
      <td>43</td>
    </tr>
    <tr>
      <th>$4.44</th>
      <td>2111948561336</td>
      <td>49</td>
    </tr>
    <tr>
      <th>$4.49</th>
      <td>3691619246759</td>
      <td>13</td>
    </tr>
    <tr>
      <th>$4.54</th>
      <td>15684864262057</td>
      <td>77</td>
    </tr>
    <tr>
      <th>$4.56</th>
      <td>4380682523513</td>
      <td>106</td>
    </tr>
    <tr>
      <th>$4.57</th>
      <td>15504409787000</td>
      <td>134</td>
    </tr>
    <tr>
      <th>$4.69</th>
      <td>3351207691452</td>
      <td>4</td>
    </tr>
    <tr>
      <th>$4.71</th>
      <td>5262366583121</td>
      <td>9</td>
    </tr>
    <tr>
      <th>$4.74</th>
      <td>8941891433572</td>
      <td>55</td>
    </tr>
    <tr>
      <th>$4.76</th>
      <td>6963610460845</td>
      <td>11</td>
    </tr>
    <tr>
      <th>$4.79</th>
      <td>2729162668443</td>
      <td>68</td>
    </tr>
    <tr>
      <th>$4.84</th>
      <td>3612459319748</td>
      <td>9</td>
    </tr>
    <tr>
      <th>$4.85</th>
      <td>6743206927163</td>
      <td>67</td>
    </tr>
    <tr>
      <th>$4.86</th>
      <td>1976270179618</td>
      <td>65</td>
    </tr>
    <tr>
      <th>$4.90</th>
      <td>4049248134531</td>
      <td>60</td>
    </tr>
    <tr>
      <th>$4.92</th>
      <td>8534735733800</td>
      <td>41</td>
    </tr>
    <tr>
      <th>$4.94</th>
      <td>3705415349696</td>
      <td>67</td>
    </tr>
    <tr>
      <th>$4.95</th>
      <td>5910932409152</td>
      <td>37</td>
    </tr>
    <tr>
      <th>$5.07</th>
      <td>3086950951569</td>
      <td>70</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>$49.36</th>
      <td>1790101950968</td>
      <td>27</td>
    </tr>
    <tr>
      <th>$49.37</th>
      <td>8053437881027</td>
      <td>24</td>
    </tr>
    <tr>
      <th>$49.47</th>
      <td>617204620844</td>
      <td>4</td>
    </tr>
    <tr>
      <th>$49.48</th>
      <td>4491450664304</td>
      <td>27</td>
    </tr>
    <tr>
      <th>$49.59</th>
      <td>2441127691517</td>
      <td>16</td>
    </tr>
    <tr>
      <th>$49.62</th>
      <td>6045427401799</td>
      <td>7</td>
    </tr>
    <tr>
      <th>$49.68</th>
      <td>451042003656</td>
      <td>18</td>
    </tr>
    <tr>
      <th>$49.72</th>
      <td>7380786579345</td>
      <td>16</td>
    </tr>
    <tr>
      <th>$49.85</th>
      <td>9119405187566</td>
      <td>4</td>
    </tr>
    <tr>
      <th>$49.95</th>
      <td>7153762992366</td>
      <td>22</td>
    </tr>
    <tr>
      <th>$50.03</th>
      <td>9224879345166</td>
      <td>10</td>
    </tr>
    <tr>
      <th>$50.43</th>
      <td>9888063516963</td>
      <td>6</td>
    </tr>
    <tr>
      <th>$51.01</th>
      <td>612689673941</td>
      <td>10</td>
    </tr>
    <tr>
      <th>$51.09</th>
      <td>3151357214095</td>
      <td>3</td>
    </tr>
    <tr>
      <th>$51.32</th>
      <td>6841691147797</td>
      <td>9</td>
    </tr>
    <tr>
      <th>$52.99</th>
      <td>9771737428375</td>
      <td>6</td>
    </tr>
    <tr>
      <th>$53.42</th>
      <td>8515375903761</td>
      <td>9</td>
    </tr>
    <tr>
      <th>$54.73</th>
      <td>9861677486822</td>
      <td>3</td>
    </tr>
    <tr>
      <th>$55.07</th>
      <td>8000095653619</td>
      <td>10</td>
    </tr>
    <tr>
      <th>$56.02</th>
      <td>311733202150</td>
      <td>3</td>
    </tr>
    <tr>
      <th>$56.07</th>
      <td>7011959069461</td>
      <td>3</td>
    </tr>
    <tr>
      <th>$56.41</th>
      <td>1763715882352</td>
      <td>3</td>
    </tr>
    <tr>
      <th>$56.60</th>
      <td>9002881309143</td>
      <td>6</td>
    </tr>
    <tr>
      <th>$57.52</th>
      <td>7365786843443</td>
      <td>3</td>
    </tr>
    <tr>
      <th>$58.45</th>
      <td>2786024888524</td>
      <td>6</td>
    </tr>
    <tr>
      <th>$58.95</th>
      <td>3176534714830</td>
      <td>10</td>
    </tr>
    <tr>
      <th>$59.43</th>
      <td>8088329954312</td>
      <td>9</td>
    </tr>
    <tr>
      <th>$59.53</th>
      <td>5277708038641</td>
      <td>3</td>
    </tr>
    <tr>
      <th>$59.64</th>
      <td>1666847044707</td>
      <td>3</td>
    </tr>
    <tr>
      <th>$59.65</th>
      <td>241191157535</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>1836 rows × 2 columns</p>
</div>




```python
# Calculate Average Fare ($) Per City
ride_city_group = city_ride_table.groupby("city")
average_fare_per_city = ride_city_group["fare"].mean()
average_fare_per_city 



#city_ride_table['average_fare_per_city'] = average_fare_per_city
#city_ride_table.head()
```




    city
    Alvarezhaven           $23.93
    Alyssaberg             $20.61
    Anitamouth             $37.32
    Antoniomouth           $23.62
    Aprilchester           $21.98
    Arnoldview             $25.11
    Campbellport           $33.71
    Carrollbury            $36.61
    Carrollfort            $25.40
    Clarkstad              $31.05
    Conwaymouth            $34.59
    Davidtown              $22.98
    Davistown              $21.50
    East Cherylfurt        $31.42
    East Douglas           $26.17
    East Erin              $24.48
    East Jenniferchester   $32.60
    East Leslie            $33.66
    East Stephen           $39.05
    East Troybury          $33.24
    Edwardsbury            $26.88
    Erikport               $30.04
    Eriktown               $25.48
    Floresberg             $32.31
    Fosterside             $23.03
    Hernandezshire         $32.00
    Horneland              $21.48
    Jacksonfort            $32.01
    Jacobfort              $24.78
    Jasonfort              $27.83
                            ...  
    South Roy              $26.03
    South Shannonborough   $26.52
    Spencertown            $23.68
    Stevensport            $31.95
    Stewartview            $21.61
    Swansonbury            $27.46
    Thomastown             $30.31
    Tiffanyton             $28.51
    Torresshire            $24.21
    Travisville            $27.22
    Vickimouth             $21.47
    Webstertown            $29.72
    West Alexis            $19.52
    West Brandy            $24.16
    West Brittanyton       $25.44
    West Dawnfurt          $22.33
    West Evan              $27.01
    West Jefferyfurt       $21.07
    West Kevintown         $21.53
    West Oscar             $24.28
    West Pamelaborough     $33.80
    West Paulport          $33.28
    West Peter             $24.88
    West Sydneyhaven       $22.37
    West Tony              $29.61
    Williamchester         $34.28
    Williamshire           $26.99
    Wiseborough            $22.68
    Yolandafurt            $27.21
    Zimmermanmouth         $28.30
    Name: fare, Length: 125, dtype: float64




```python
# Calculate number of rides Per City
ride_city_group = ride_data_df.groupby("city")
number_of_rides_per_city = ride_city_group["ride_id"].count()
number_of_rides_per_city



#df.sort_values(by=['col1'])

```




    city
    Alvarezhaven            31
    Alyssaberg              26
    Anitamouth               9
    Antoniomouth            22
    Aprilchester            19
    Arnoldview              31
    Campbellport            15
    Carrollbury             10
    Carrollfort             29
    Clarkstad               12
    Conwaymouth             11
    Davidtown               21
    Davistown               25
    East Cherylfurt         13
    East Douglas            22
    East Erin               28
    East Jenniferchester    19
    East Leslie             11
    East Stephen            10
    East Troybury            7
    Edwardsbury             27
    Erikport                 8
    Eriktown                19
    Floresberg              10
    Fosterside              24
    Hernandezshire           9
    Horneland                4
    Jacksonfort              6
    Jacobfort               31
    Jasonfort               12
                            ..
    South Roy               22
    South Shannonborough    15
    Spencertown             26
    Stevensport              5
    Stewartview             30
    Swansonbury             34
    Thomastown              24
    Tiffanyton              13
    Torresshire             26
    Travisville             23
    Vickimouth              15
    Webstertown             16
    West Alexis             20
    West Brandy             30
    West Brittanyton        24
    West Dawnfurt           29
    West Evan               12
    West Jefferyfurt        21
    West Kevintown           7
    West Oscar              29
    West Pamelaborough      14
    West Paulport           17
    West Peter              31
    West Sydneyhaven        18
    West Tony               19
    Williamchester          11
    Williamshire            31
    Wiseborough             19
    Yolandafurt             20
    Zimmermanmouth          24
    Name: ride_id, Length: 125, dtype: int64




```python
#Create a new column that contains a numeric value for the data type that can be used in the scatter plot 
if city_data_df['type'].any() == 'Urban':
    city_data_df['type_int'] = 1
elif city_data_df['type'].any() == 'Suburban':
    city_data_df['type_int'] = 2
elif city_data_df['type'].any() == 'Rural':
    city_data_df['type_int'] = 3 

   
```


```python
# Create a bubble plot that shows the relationships between the following 
# * Average Fare ($) Per City      - x axis
# * Total Number of Rides Per City - y axis
# * Total Number of Drivers Per City - this is used to determine size of circles
# * City Type (Urban, Suburban, Rural) - color 

# bubble plot
x = number_of_rides_per_city
y = average_fare_per_city
t = city_data_df['driver_count']

z = city_data_df['type_int']

#urban = plt.scatter(x, y, s=t*4, alpha=0.5, color = city_data_df['plot_color'] , edgecolors = 'black', linewidths=2, label='Urban')
#suburban = plt.scatter(x, y, s=t*4, alpha=0.5, color = city_data_df['plot_color'], edgecolors = 'black', linewidths=2, label='Suburban')
#rural = plt.scatter(x, y, s=t*4, alpha=0.5, color = city_data_df['plot_color'], edgecolors = 'black', linewidths=2, label='Rural')


#this one works sort of 
plt.scatter(x, y, s=t*4, alpha=0.5, color = city_data_df['plot_color'], edgecolors = 'black', linewidths=2)#, lable=lable)

plt.xlim([min(number_of_rides_per_city) -2,max(number_of_rides_per_city)+2])
plt.ylim([min(average_fare_per_city)-3,max(average_fare_per_city)+3])

plt.xlabel('Total Number of Rides (Per City)') 
plt.ylabel('Average Fare ($)')
plt.title('Pyber Ride Sharing Data (2016)')

# Set our legend to where the chart thinks is best
plt.legend(loc="best")

plt.show()


```


![png](output_10_0.png)



```python
# Calculate Average Fare ($) Per City
city_ride_group = city_ride_table.groupby("type")
total_fare_per_type = city_ride_group['fare'].sum()
total_fare_per_type

```




    type
    Rural       $4,255.09
    Suburban   $20,335.69
    Urban      $40,078.34
    Name: fare, dtype: float64




```python
# Labels for the sections of our pie chart
labels = ["Rural", "Suburban", "Urban"]
# The values of each section of the pie chart
sizes = total_fare_per_type #[185, 172, 100, 110]
# The colors of each section of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]
# Tells matplotlib to seperate the "Python" section from the others
explode = (0.1, 0.1, 0)

# In[3]:

# Creates the pie chart based upon the values above
# Automatically finds the percentages of each part of the pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
autopct="%1.1f%%", shadow=True, startangle=140)

# In[4]:

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

plt.title('% of Total Fares by City Type')
# Prints our pie chart to the screen
plt.show()

```


![png](output_12_0.png)



```python
# Calculate Ridse Per City Type
city_ride_group = city_ride_table.groupby("type")
total_rides_per_type = city_ride_group['ride_id'].count()
total_rides_per_type

```




    type
    Rural        125
    Suburban     657
    Urban       1625
    Name: ride_id, dtype: int64




```python
# Labels for the sections of our pie chart
labels = ["Rural", "Suburban", "Urban"]
# The values of each section of the pie chart
sizes = total_rides_per_type
# The colors of each section of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]
# Tells matplotlib to seperate the "Python" section from the others
explode = (0.1, 0.1, 0)

# In[3]:
plt.title('% of Total Rides by City Type')
# Creates the pie chart based upon the values above
# Automatically finds the percentages of each part of the pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
autopct="%1.1f%%", shadow=True, startangle=140)

# In[4]:

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

# In[5]:

# Prints our pie chart to the screen
plt.show()

```


![png](output_14_0.png)



```python
# Calculate Total Drivers Per City Type
city_ride_group = city_ride_table.groupby("type")
total_drivers_per_type = city_ride_group['driver_count'].sum()
total_drivers_per_type
```




    type
    Rural         727
    Suburban     9730
    Urban       64501
    Name: driver_count, dtype: int64




```python
# Labels for the sections of our pie chart
labels = ["Rural", "Suburban", "Urban"]
# The values of each section of the pie chart
sizes = total_drivers_per_type
# The colors of each section of the pie chart
colors = ["gold", "lightskyblue", "lightcoral"]
# Tells matplotlib to seperate the "Python" section from the others
explode = (0.1, 0.1, 0)

# In[3]:
plt.title('% of Total Drivers by City Type')
# Creates the pie chart based upon the values above
# Automatically finds the percentages of each part of the pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
autopct="%1.1f%%", shadow=True, startangle=140)

# In[4]:

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

# In[5]:

# Prints our pie chart to the screen
plt.show()

```


![png](output_16_0.png)

