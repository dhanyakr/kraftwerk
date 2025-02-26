### Required Assignment 5.1: Will the Customer Accept the Coupon?

**Context**

Imagine driving through town and a coupon is delivered to your cell phone for a restaurant near where you are driving. Would you accept that coupon and take a short detour to the restaurant? Would you accept the coupon but use it on a subsequent trip? Would you ignore the coupon entirely? What if the coupon was for a bar instead of a restaurant? What about a coffee house? Would you accept a bar coupon with a minor passenger in the car? What about if it was just you and your partner in the car? Would weather impact the rate of acceptance? What about the time of day?

Obviously, proximity to the business is a factor on whether the coupon is delivered to the driver or not, but what are the factors that determine whether a driver accepts the coupon once it is delivered to them? How would you determine whether a driver is likely to accept a coupon?

**Overview**

The goal of this project is to use what you know about visualizations and probability distributions to distinguish between customers who accepted a driving coupon versus those that did not.

**Data**

This data comes to us from the UCI Machine Learning repository and was collected via a survey on Amazon Mechanical Turk. The survey describes different driving scenarios including the destination, current time, weather, passenger, etc., and then ask the person whether he will accept the coupon if he is the driver. Answers that the user will drive there ‘right away’ or ‘later before the coupon expires’ are labeled as ‘Y = 1’ and answers ‘no, I do not want the coupon’ are labeled as ‘Y = 0’.  There are five different types of coupons -- less expensive restaurants (under \$20), coffee houses, carry out & take away, bar, and more expensive restaurants (\$20 - \$50).

**Deliverables**

Your final product should be a brief report that highlights the differences between customers who did and did not accept the coupons.  To explore the data you will utilize your knowledge of plotting, statistical summaries, and visualization using Python. You will publish your findings in a public facing github repository as your first portfolio piece.





### Data Description
Keep in mind that these values mentioned below are average values.

The attributes of this data set include:
1. User attributes
    -  Gender: male, female
    -  Age: below 21, 21 to 25, 26 to 30, etc.
    -  Marital Status: single, married partner, unmarried partner, or widowed
    -  Number of children: 0, 1, or more than 1
    -  Education: high school, bachelors degree, associates degree, or graduate degree
    -  Occupation: architecture & engineering, business & financial, etc.
    -  Annual income: less than \\$12500, \\$12500 - \\$24999, \\$25000 - \\$37499, etc.
    -  Number of times that he/she goes to a bar: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
    -  Number of times that he/she buys takeaway food: 0, less than 1, 1 to 3, 4 to 8 or greater
    than 8
    -  Number of times that he/she goes to a coffee house: 0, less than 1, 1 to 3, 4 to 8 or
    greater than 8
    -  Number of times that he/she eats at a restaurant with average expense less than \\$20 per
    person: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
    -  Number of times that he/she goes to a bar: 0, less than 1, 1 to 3, 4 to 8 or greater than 8
    

2. Contextual attributes
    - Driving destination: home, work, or no urgent destination
    - Location of user, coupon and destination: we provide a map to show the geographical
    location of the user, destination, and the venue, and we mark the distance between each
    two places with time of driving. The user can see whether the venue is in the same
    direction as the destination.
    - Weather: sunny, rainy, or snowy
    - Temperature: 30F, 55F, or 80F
    - Time: 10AM, 2PM, or 6PM
    - Passenger: alone, partner, kid(s), or friend(s)


3. Coupon attributes
    - time before it expires: 2 hours or one day


```python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import numpy as np
import plotly.express as px
```

### Problems

Use the prompts below to get started with your data analysis.  

1. Read in the `coupons.csv` file.





```python
data = pd.read_csv('data/coupons.csv')
```


```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>destination</th>
      <th>passanger</th>
      <th>weather</th>
      <th>temperature</th>
      <th>time</th>
      <th>coupon</th>
      <th>expiration</th>
      <th>gender</th>
      <th>age</th>
      <th>maritalStatus</th>
      <th>...</th>
      <th>CoffeeHouse</th>
      <th>CarryAway</th>
      <th>RestaurantLessThan20</th>
      <th>Restaurant20To50</th>
      <th>toCoupon_GEQ5min</th>
      <th>toCoupon_GEQ15min</th>
      <th>toCoupon_GEQ25min</th>
      <th>direction_same</th>
      <th>direction_opp</th>
      <th>Y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>No Urgent Place</td>
      <td>Alone</td>
      <td>Sunny</td>
      <td>55</td>
      <td>2PM</td>
      <td>Restaurant(&lt;20)</td>
      <td>1d</td>
      <td>Female</td>
      <td>21</td>
      <td>Unmarried partner</td>
      <td>...</td>
      <td>never</td>
      <td>NaN</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>80</td>
      <td>10AM</td>
      <td>Coffee House</td>
      <td>2h</td>
      <td>Female</td>
      <td>21</td>
      <td>Unmarried partner</td>
      <td>...</td>
      <td>never</td>
      <td>NaN</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>80</td>
      <td>10AM</td>
      <td>Carry out &amp; Take away</td>
      <td>2h</td>
      <td>Female</td>
      <td>21</td>
      <td>Unmarried partner</td>
      <td>...</td>
      <td>never</td>
      <td>NaN</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>80</td>
      <td>2PM</td>
      <td>Coffee House</td>
      <td>2h</td>
      <td>Female</td>
      <td>21</td>
      <td>Unmarried partner</td>
      <td>...</td>
      <td>never</td>
      <td>NaN</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>80</td>
      <td>2PM</td>
      <td>Coffee House</td>
      <td>1d</td>
      <td>Female</td>
      <td>21</td>
      <td>Unmarried partner</td>
      <td>...</td>
      <td>never</td>
      <td>NaN</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 26 columns</p>
</div>



2. Investigate the dataset for missing or problematic data.


```python
# checking counts and data type for each column
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 12684 entries, 0 to 12683
    Data columns (total 26 columns):
     #   Column                Non-Null Count  Dtype 
    ---  ------                --------------  ----- 
     0   destination           12684 non-null  object
     1   passanger             12684 non-null  object
     2   weather               12684 non-null  object
     3   temperature           12684 non-null  int64 
     4   time                  12684 non-null  object
     5   coupon                12684 non-null  object
     6   expiration            12684 non-null  object
     7   gender                12684 non-null  object
     8   age                   12684 non-null  object
     9   maritalStatus         12684 non-null  object
     10  has_children          12684 non-null  int64 
     11  education             12684 non-null  object
     12  occupation            12684 non-null  object
     13  income                12684 non-null  object
     14  car                   108 non-null    object
     15  Bar                   12577 non-null  object
     16  CoffeeHouse           12467 non-null  object
     17  CarryAway             12533 non-null  object
     18  RestaurantLessThan20  12554 non-null  object
     19  Restaurant20To50      12495 non-null  object
     20  toCoupon_GEQ5min      12684 non-null  int64 
     21  toCoupon_GEQ15min     12684 non-null  int64 
     22  toCoupon_GEQ25min     12684 non-null  int64 
     23  direction_same        12684 non-null  int64 
     24  direction_opp         12684 non-null  int64 
     25  Y                     12684 non-null  int64 
    dtypes: int64(8), object(18)
    memory usage: 2.5+ MB



```python

```




    (12684, 26)




```python
# data count
print(data.shape)

# checking for the presence of null values
missing_vals = data.isnull().sum()

print(type(missing_vals))
missing_vals
```

    (12684, 26)
    <class 'pandas.core.series.Series'>





    destination                 0
    passanger                   0
    weather                     0
    temperature                 0
    time                        0
    coupon                      0
    expiration                  0
    gender                      0
    age                         0
    maritalStatus               0
    has_children                0
    education                   0
    occupation                  0
    income                      0
    car                     12576
    Bar                       107
    CoffeeHouse               217
    CarryAway                 151
    RestaurantLessThan20      130
    Restaurant20To50          189
    toCoupon_GEQ5min            0
    toCoupon_GEQ15min           0
    toCoupon_GEQ25min           0
    direction_same              0
    direction_opp               0
    Y                           0
    dtype: int64




```python
# lets get the list of numerical and categorical columns
column_names = data.columns.tolist()

column_names
```




    ['destination',
     'passanger',
     'weather',
     'temperature',
     'time',
     'coupon',
     'expiration',
     'gender',
     'age',
     'maritalStatus',
     'has_children',
     'education',
     'occupation',
     'income',
     'car',
     'Bar',
     'CoffeeHouse',
     'CarryAway',
     'RestaurantLessThan20',
     'Restaurant20To50',
     'toCoupon_GEQ5min',
     'toCoupon_GEQ15min',
     'toCoupon_GEQ25min',
     'direction_same',
     'direction_opp',
     'Y']




```python
# Explore if we need these columns for our analysis
cols = ['car', 'toCoupon_GEQ5min', 'toCoupon_GEQ15min', 'toCoupon_GEQ25min', 'Y']
for col in cols:
    print(f'{col}: {data[col].unique()}')

# conclusion: We don't need these columns `'car', 'toCoupon_GEQ5min', 'toCoupon_GEQ15min', 'toCoupon_GEQ25min'`. 
# Also, the column 'Y' is recoding the acceptance of the coupon by the user
```

    car: [nan 'Scooter and motorcycle' 'crossover' 'Mazda5' 'do not drive'
     'Car that is too old to install Onstar :D']
    toCoupon_GEQ5min: [1]
    toCoupon_GEQ15min: [0 1]
    toCoupon_GEQ25min: [0 1]
    Y: [1 0]



```python
# Understand the values/nature of the numerical columns
data.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>temperature</th>
      <th>has_children</th>
      <th>toCoupon_GEQ5min</th>
      <th>toCoupon_GEQ15min</th>
      <th>toCoupon_GEQ25min</th>
      <th>direction_same</th>
      <th>direction_opp</th>
      <th>Y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>12684.000000</td>
      <td>12684.000000</td>
      <td>12684.0</td>
      <td>12684.000000</td>
      <td>12684.000000</td>
      <td>12684.000000</td>
      <td>12684.000000</td>
      <td>12684.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>63.301798</td>
      <td>0.414144</td>
      <td>1.0</td>
      <td>0.561495</td>
      <td>0.119126</td>
      <td>0.214759</td>
      <td>0.785241</td>
      <td>0.568433</td>
    </tr>
    <tr>
      <th>std</th>
      <td>19.154486</td>
      <td>0.492593</td>
      <td>0.0</td>
      <td>0.496224</td>
      <td>0.323950</td>
      <td>0.410671</td>
      <td>0.410671</td>
      <td>0.495314</td>
    </tr>
    <tr>
      <th>min</th>
      <td>30.000000</td>
      <td>0.000000</td>
      <td>1.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>55.000000</td>
      <td>0.000000</td>
      <td>1.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>80.000000</td>
      <td>0.000000</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>80.000000</td>
      <td>1.000000</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>80.000000</td>
      <td>1.000000</td>
      <td>1.0</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

3. Decide what to do about your missing data -- drop, replace, other...


```python
"""
1. Based on the previous data exploration phase, figured out that we may not need these columns for the analysis 
 `'car', 'toCoupon_GEQ5min', 'toCoupon_GEQ15min', 'toCoupon_GEQ25min'`.
 
2. Since these fields have NaN values and also the no of values are less compared to the overall no of rows, we can drop them
        Bar                       107
        CoffeeHouse               217
        CarryAway                 151
        RestaurantLessThan20      130
        Restaurant20To50          189
3. The column 'Y' is recoding the acceptance of the coupon by the user, so its better to rename it to coupon_accepted
4. For better readability/ uniformity I am renaming,
    1. maritalStatus -> marital_status
    2. Bar -> bar
    3. CoffeeHouse -> coffee_house
    4. CarryAway -> carry_away
    5. RestaurantLessThan20 -> restaurant_less_than_20
    6. Restaurant20To50 -> restaurant_20_to_50
"""

data.drop(columns=['car', 'toCoupon_GEQ5min', 'toCoupon_GEQ15min', 'toCoupon_GEQ25min'], inplace=True)

data.dropna(inplace=True)

data.rename(columns={'Y':'coupon_accepted', 'maritalStatus': 'marital_status', 'Bar': 'bar', 'CoffeeHouse': 'coffee_house', 'CarryAway': 'carry_away', 'RestaurantLessThan20': 'restaurant_less_than_20', 'Restaurant20To50': 'restaurant_20_to_50' }, inplace=True)

print(data.shape)
data.isnull().sum()
```

    (12079, 22)





    destination                0
    passanger                  0
    weather                    0
    temperature                0
    time                       0
    coupon                     0
    expiration                 0
    gender                     0
    age                        0
    marital_status             0
    has_children               0
    education                  0
    occupation                 0
    income                     0
    bar                        0
    coffee_house               0
    carry_away                 0
    restaurant_less_than_20    0
    restaurant_20_to_50        0
    direction_same             0
    direction_opp              0
    coupon_accepted            0
    dtype: int64



4. What proportion of the total observations chose to accept the coupon?




```python
coupon_accepted = data['coupon_accepted'].mean()

print(f'{"Chose to accept the coupon"} : {coupon_accepted:.2%}')
```

    Chose to accept the coupon : 56.93%


5. Use a bar plot to visualize the `coupon` column.


```python
print(f'{col}: {data['coupon'].unique()}')
```

    Y: ['Restaurant(<20)' 'Coffee House' 'Bar' 'Carry out & Take away'
     'Restaurant(20-50)']



```python

```


```python
sns.countplot(data=data, x='coupon', hue= 'coupon')

plt.xticks(rotation=45)
plt.title('Distribution of Coupons')
plt.xlabel('coupon')
plt.savefig("images/distribution_of_coupons.png")
plt.show()

```


    
![png](images/distribution_of_coupons.png)
    


####  2. How about when we are traveling in the same direction


6. Use a histogram to visualize the temperature column.


```python
sns.histplot(data = data, x = 'temperature', hue='temperature')
plt.title('Distribution of Temperatures')
plt.xlabel('Temperature')
plt.ylabel('Frequency')
plt.savefig('images/distribution_of_temperatures.png')
plt.show()

```


    
![png](images/distribution_of_temperatures.png)
    



```python

```


```python

```


```python

```


```python

```

**Investigating the Bar Coupons**

Now, we will lead you through an exploration of just the bar related coupons.  

1. Create a new `DataFrame` that contains just the bar coupons.



```python
bar_coupon_data = data.query('coupon == "Bar"').copy()

print(bar_coupon_data.shape)

bar_coupon_data.head()
```

    (1913, 22)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>destination</th>
      <th>passanger</th>
      <th>weather</th>
      <th>temperature</th>
      <th>time</th>
      <th>coupon</th>
      <th>expiration</th>
      <th>gender</th>
      <th>age</th>
      <th>marital_status</th>
      <th>...</th>
      <th>occupation</th>
      <th>income</th>
      <th>bar</th>
      <th>coffee_house</th>
      <th>carry_away</th>
      <th>restaurant_less_than_20</th>
      <th>restaurant_20_to_50</th>
      <th>direction_same</th>
      <th>direction_opp</th>
      <th>coupon_accepted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>80</td>
      <td>10AM</td>
      <td>Bar</td>
      <td>1d</td>
      <td>Male</td>
      <td>21</td>
      <td>Single</td>
      <td>...</td>
      <td>Architecture &amp; Engineering</td>
      <td>$62500 - $74999</td>
      <td>never</td>
      <td>less1</td>
      <td>4~8</td>
      <td>4~8</td>
      <td>less1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Home</td>
      <td>Alone</td>
      <td>Sunny</td>
      <td>55</td>
      <td>6PM</td>
      <td>Bar</td>
      <td>1d</td>
      <td>Male</td>
      <td>21</td>
      <td>Single</td>
      <td>...</td>
      <td>Architecture &amp; Engineering</td>
      <td>$62500 - $74999</td>
      <td>never</td>
      <td>less1</td>
      <td>4~8</td>
      <td>4~8</td>
      <td>less1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Work</td>
      <td>Alone</td>
      <td>Sunny</td>
      <td>55</td>
      <td>7AM</td>
      <td>Bar</td>
      <td>1d</td>
      <td>Male</td>
      <td>21</td>
      <td>Single</td>
      <td>...</td>
      <td>Architecture &amp; Engineering</td>
      <td>$62500 - $74999</td>
      <td>never</td>
      <td>less1</td>
      <td>4~8</td>
      <td>4~8</td>
      <td>less1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>46</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>80</td>
      <td>10AM</td>
      <td>Bar</td>
      <td>1d</td>
      <td>Male</td>
      <td>46</td>
      <td>Single</td>
      <td>...</td>
      <td>Student</td>
      <td>$12500 - $24999</td>
      <td>never</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1~3</td>
      <td>never</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Home</td>
      <td>Alone</td>
      <td>Sunny</td>
      <td>55</td>
      <td>6PM</td>
      <td>Bar</td>
      <td>1d</td>
      <td>Male</td>
      <td>46</td>
      <td>Single</td>
      <td>...</td>
      <td>Student</td>
      <td>$12500 - $24999</td>
      <td>never</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1~3</td>
      <td>never</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



2. What proportion of bar coupons were accepted?



```python
bar_coupon_accepted = bar_coupon_data['coupon_accepted'].mean()

print(f'{"Chose to accept the bar coupon"} : {bar_coupon_accepted:.2%}')
```

    Chose to accept the bar coupon : 41.19%


3. Compare the acceptance rate between those who went to a bar 3 or fewer times a month to those who went more.



```python
# add a new column to the bar_data to indicate if the person is visiting the bar 3 or fewer time a month or more
print(f'{'----------------------------------------------------------------------'}')
print(f'{'Distinct bar values'} : {bar_coupon_data['bar'].unique()}')
print(f'{'----------------------------------------------------------------------\n\n'}')

bar_coupon_data['bar_visit_3_or_less'] = bar_coupon_data['bar'].apply(lambda col: col in ['never', 'less1', '1~3'])
#bar_coupon_data.head()

bar_coupon_accepted_by_less_than_3_visits = bar_coupon_data.groupby('bar_visit_3_or_less')['coupon_accepted'].mean()

print(f'{'Those visited less than 3'}: {bar_coupon_accepted_by_less_than_3_visits[True]:.2%}')
print(f'{'Those visited more than 3'}: {bar_coupon_accepted_by_less_than_3_visits[False]:.2%}')

import plotly.io as pio

fig = px.histogram(data_frame=bar_coupon_data, x='bar_visit_3_or_less', color='coupon_accepted')
fig.write_image("images/bar_visit_3_or_less.png", format="png", width=800, height=600, scale=2)
fig.show()
```

    ----------------------------------------------------------------------
    Distinct bar values : ['never' 'less1' '1~3' 'gt8' '4~8']
    ----------------------------------------------------------------------
    
    
    Those visited less than 3: 37.27%
    Those visited more than 3: 76.17%


![png](images/bar_visit_3_or_less.png)



4. Compare the acceptance rate between drivers who go to a bar more than once a month and are over the age of 25 to the all others.  Is there a difference?



```python
# add a new column match_criteria to the bar_data
print(f'{'----------------------------------------------------------------------'}')
print(f'\033[1m{'Distinct age       '}\033[0m : {bar_coupon_data['age'].unique()}')
print(f'\033[1m{'Distinct bar values'}\033[0m : {bar_coupon_data['bar'].unique()}')
print(f'{'----------------------------------------------------------------------\n\n'}')

bar_coupon_data['bar_more_than_once_age_25'] = bar_coupon_data.apply(lambda row: (row['bar'] not in ["never","less1"]) & (row['age'] not in ["21", "below21"]), axis=1)

bar_coupon_more_than_once_age_25 = bar_coupon_data.groupby('bar_more_than_once_age_25')['coupon_accepted'].mean()

print(f'\033[1m{'Those visited bar more than once and age of 25 to all others'}\033[0m: \033[94m{bar_coupon_more_than_once_age_25[True]:.2%}\033[0m')
print(f'\033[1m{'Those visited bar less than 1 or age less than 25'}\033[0m: \033[94m{bar_coupon_more_than_once_age_25[False]:.2%}\033[0m')


fig = px.histogram(data_frame=bar_coupon_data, x='bar_more_than_once_age_25', color='coupon_accepted')
fig.write_image("images/bar_more_than_once_age_25.png", format="png", width=800, height=600, scale=2)
fig.show()

#px.histogram(data_frame=bar_coupon_data, x='bar', color='coupon_accepted', facet_row='bar_more_than_once_age_25')
```

    ----------------------------------------------------------------------
    [1mDistinct age       [0m : ['21' '46' '26' '31' '41' '50plus' '36' 'below21']
    [1mDistinct bar values[0m : ['never' 'less1' '1~3' 'gt8' '4~8']
    ----------------------------------------------------------------------
    
    
    [1mThose visited bar more than once and age of 25 to all others[0m: [94m68.98%[0m
    [1mThose visited bar less than 1 or age less than 25[0m: [94m33.77%[0m

![png](images/bar_more_than_once_age_25.png)




5. Use the same process to compare the acceptance rate between drivers who go to bars more than once a month and had passengers that were not a kid and had occupations other than farming, fishing, or forestry.



```python
print(f'{'----------------------------------------------------------------------'}')
print(f'\033[1m{'Distinct bar values    '}\033[0m : {bar_coupon_data['bar'].unique()}')
print(f'\033[1m{'Distinct passenger     '}\033[0m : {bar_coupon_data['passanger'].unique()}')
print(f'\033[1m{'Distinct occupations   '}\033[0m : {bar_coupon_data['occupation'].unique()}')

print(f'{'----------------------------------------------------------------------\n\n'}')

bar_coupon_data['acceptance_various_criteria'] = bar_coupon_data.apply(lambda row: ((row['bar'] in ['1~3', 'gt8', '4~8']) & (row['passanger'] not in ['Kid(s)']) & (row['occupation'] not in ['Farming Fishing & Forestry'])), axis=1)
bar_coupon_acceptance_various_criteria = bar_coupon_data.groupby('acceptance_various_criteria')['coupon_accepted'].mean()

print(f'\033[1m{'Those visited bar more than once and passengers were not kids and occupation other than farming, fishing, or forestry'}\033[0m: \033[94m{bar_coupon_acceptance_various_criteria[True]:.2%}\033[0m')
print(f'\033[1m{'Those who doesn\'t match the criteria'}\033[0m: \033[94m{bar_coupon_acceptance_various_criteria[False]:.2%}\033[0m')


fig = px.histogram(data_frame=bar_coupon_data, x='acceptance_various_criteria', color='coupon_accepted')
fig.write_image("images/bar_acceptance_various_criteria.png", format="png", width=800, height=600, scale=2)
fig.show()
```

    ----------------------------------------------------------------------
    [1mDistinct bar values    [0m : ['never' 'less1' '1~3' 'gt8' '4~8']
    [1mDistinct passenger     [0m : ['Friend(s)' 'Alone' 'Kid(s)' 'Partner']
    [1mDistinct occupations   [0m : ['Architecture & Engineering' 'Student' 'Education&Training&Library'
     'Unemployed' 'Healthcare Support' 'Healthcare Practitioners & Technical'
     'Sales & Related' 'Management' 'Arts Design Entertainment Sports & Media'
     'Computer & Mathematical' 'Life Physical Social Science'
     'Personal Care & Service' 'Office & Administrative Support'
     'Construction & Extraction' 'Legal' 'Retired'
     'Community & Social Services' 'Installation Maintenance & Repair'
     'Transportation & Material Moving' 'Business & Financial'
     'Protective Service' 'Food Preparation & Serving Related'
     'Production Occupations' 'Building & Grounds Cleaning & Maintenance'
     'Farming Fishing & Forestry']
    ----------------------------------------------------------------------
    
    
    [1mThose visited bar more than once and passengers were not kids and occupation other than farming, fishing, or forestry[0m: [94m70.94%[0m
    [1mThose who doesn't match the criteria[0m: [94m29.79%[0m

![png](images/bar_acceptance_various_criteria.png)



6. Compare the acceptance rates between those drivers who:

- go to bars more than once a month, had passengers that were not a kid, and were not widowed *OR*
- go to bars more than once a month and are under the age of 30 *OR*
- go to cheap restaurants more than 4 times a month and income is less than 50K.




```python
print(f'{'----------------------------------------------------------------------'}')
print(f'\033[1m{'Distinct bar values'}\033[0m: {bar_coupon_data['bar'].unique()}')
print(f'\033[1m{'Distinct passenger'}\033[0m: {bar_coupon_data['passanger'].unique()}')
print(f'\033[1m{'Distinct age'}\033[0m: {bar_coupon_data['age'].unique()}')
print(f'\033[1m{'Distinct income'}\033[0m: {bar_coupon_data['income'].unique()}')
print(f'\033[1m{'Distinct restaurant less than 20'}\033[0m: {bar_coupon_data['restaurant_less_than_20'].unique()}')
print(f'\033[1m{'Distinct marital status'}\033[0m: {bar_coupon_data['marital_status'].unique()}')

print(f'{'----------------------------------------------------------------------\n\n'}')



bar_coupon_data['acceptance_various_criteria'] = bar_coupon_data.apply(lambda row: (
                    (row['bar'] not in ['never' 'less1']) & (row['passanger'] not in ['Alone', 'Kid(s)']) & (row['marital_status'] != 'Widowed') or 
                    (row['bar'] not in ['never' 'less1']) & (row['age'] in ['21', '26', 'below21']) or 
                    (row['restaurant_less_than_20'] in ['gt8' '4~8']) & (row['income'] in ['$12500 - $24999', '$37500 - $49999', '$25000 - $37499', 'Less than $12500'])
                    ), axis=1)


bar_coupon_accepted_by_match_criteria = bar_coupon_data.groupby('acceptance_various_criteria')['coupon_accepted'].mean()


print(f'\033[1m{'go to bars more than once a month, had passengers that were not a kid, and were not widowed OR \n go to bars more than once a month and are under the age of 30 OR \n go to cheap restaurants more than 4 times a month and income is less than 50K.  '}\033[0m : \033[94m{bar_coupon_accepted_by_match_criteria[True]:.2%}\033[0m')   
print(f'\033[1m{'Those who doesn\'t match the criteria'}\033[0m: \033[94m{bar_coupon_accepted_by_match_criteria[False]:.2%}\033[0m')

fig = px.histogram(data_frame=bar_coupon_data, x='acceptance_various_criteria', color='coupon_accepted')
fig.write_image("images/bar_acceptance_various_criteria_passanger_age_restaurant.png", format="png", width=800, height=600, scale=2)
fig.show()

```

    ----------------------------------------------------------------------
    [1mDistinct bar values[0m: ['never' 'less1' '1~3' 'gt8' '4~8']
    [1mDistinct passenger[0m: ['Friend(s)' 'Alone' 'Kid(s)' 'Partner']
    [1mDistinct age[0m: ['21' '46' '26' '31' '41' '50plus' '36' 'below21']
    [1mDistinct income[0m: ['$62500 - $74999' '$12500 - $24999' '$75000 - $87499' '$50000 - $62499'
     '$37500 - $49999' '$25000 - $37499' '$100000 or More' '$87500 - $99999'
     'Less than $12500']
    [1mDistinct restaurant less than 20[0m: ['4~8' '1~3' 'less1' 'gt8' 'never']
    [1mDistinct marital status[0m: ['Single' 'Married partner' 'Unmarried partner' 'Divorced' 'Widowed']
    ----------------------------------------------------------------------
    
    
    [1mgo to bars more than once a month, had passengers that were not a kid, and were not widowed OR 
     go to bars more than once a month and are under the age of 30 OR 
     go to cheap restaurants more than 4 times a month and income is less than 50K.  [0m : [94m47.71%[0m
    [1mThose who doesn't match the criteria[0m: [94m31.71%[0m

![png](images/bar_acceptance_various_criteria_passanger_age_restaurant.png)



7.  Based on these observations, what do you hypothesize about drivers who accepted the bar coupons?

    The drivers, who were not having kids as passengers and occupation other than farming, fishing, or forestry and age anove 25 has higher chance to accept bar coupons




## Independent Investigation

Using the bar coupon example as motivation, you are to explore one of the other coupon groups and try to determine the characteristics of passengers who accept the coupons.  

### Investigating the Carry out & Take away Coupons


```python
carry_away_coupon_data = data.query('coupon == "Carry out & Take away"').copy()

print(carry_away_coupon_data.shape)

carry_away_coupon_data.head()

```

    (2280, 22)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>destination</th>
      <th>passanger</th>
      <th>weather</th>
      <th>temperature</th>
      <th>time</th>
      <th>coupon</th>
      <th>expiration</th>
      <th>gender</th>
      <th>age</th>
      <th>marital_status</th>
      <th>...</th>
      <th>occupation</th>
      <th>income</th>
      <th>bar</th>
      <th>coffee_house</th>
      <th>carry_away</th>
      <th>restaurant_less_than_20</th>
      <th>restaurant_20_to_50</th>
      <th>direction_same</th>
      <th>direction_opp</th>
      <th>coupon_accepted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>25</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>80</td>
      <td>10AM</td>
      <td>Carry out &amp; Take away</td>
      <td>2h</td>
      <td>Male</td>
      <td>21</td>
      <td>Single</td>
      <td>...</td>
      <td>Architecture &amp; Engineering</td>
      <td>$62500 - $74999</td>
      <td>never</td>
      <td>less1</td>
      <td>4~8</td>
      <td>4~8</td>
      <td>less1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>55</td>
      <td>2PM</td>
      <td>Carry out &amp; Take away</td>
      <td>1d</td>
      <td>Male</td>
      <td>21</td>
      <td>Single</td>
      <td>...</td>
      <td>Architecture &amp; Engineering</td>
      <td>$62500 - $74999</td>
      <td>never</td>
      <td>less1</td>
      <td>4~8</td>
      <td>4~8</td>
      <td>less1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Work</td>
      <td>Alone</td>
      <td>Sunny</td>
      <td>80</td>
      <td>7AM</td>
      <td>Carry out &amp; Take away</td>
      <td>2h</td>
      <td>Male</td>
      <td>21</td>
      <td>Single</td>
      <td>...</td>
      <td>Architecture &amp; Engineering</td>
      <td>$62500 - $74999</td>
      <td>never</td>
      <td>less1</td>
      <td>4~8</td>
      <td>4~8</td>
      <td>less1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>47</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>80</td>
      <td>10AM</td>
      <td>Carry out &amp; Take away</td>
      <td>2h</td>
      <td>Male</td>
      <td>46</td>
      <td>Single</td>
      <td>...</td>
      <td>Student</td>
      <td>$12500 - $24999</td>
      <td>never</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1~3</td>
      <td>never</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>55</th>
      <td>No Urgent Place</td>
      <td>Friend(s)</td>
      <td>Sunny</td>
      <td>55</td>
      <td>2PM</td>
      <td>Carry out &amp; Take away</td>
      <td>1d</td>
      <td>Male</td>
      <td>46</td>
      <td>Single</td>
      <td>...</td>
      <td>Student</td>
      <td>$12500 - $24999</td>
      <td>never</td>
      <td>4~8</td>
      <td>1~3</td>
      <td>1~3</td>
      <td>never</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



#### 1. What proportion of carry away coupons were accepted?



```python
carry_away_coupon_accepted = carry_away_coupon_data['coupon_accepted'].mean()

print(f'{"Chose to accept the Carry away coupon"} : {carry_away_coupon_accepted:.2%}')

```

    Chose to accept the Carry away coupon : 73.77%


#### 2. Carry out & Take away Coupons Accepted Rate based on Passenger Type



```python
print(f'{'----------------------------------------------------------------------'}')
print(f'\033[1m{'Distinct passanger values'}\033[0m: {carry_away_coupon_data['passanger'].unique()}')
print(f'{'----------------------------------------------------------------------\n\n'}')

fig = px.histogram(data_frame=carry_away_coupon_data, x='coupon_accepted', color='passanger', title='Carry away Coupons Accepted Rate based on Passenger Type')
fig.show()
```

    ----------------------------------------------------------------------
    [1mDistinct passanger values[0m: ['Friend(s)' 'Alone' 'Kid(s)' 'Partner']
    ----------------------------------------------------------------------
    
    





```python
fig = px.histogram(data_frame=carry_away_coupon_data, x='coupon_accepted', color='carry_away', title='No of times Carry away Coupons Accepted')
fig.write_image("images/carry_away_coupons.png", format="png", width=800, height=600, scale=2)
fig.show()
```
![png](images/carry_away_coupons.png)





```python
fig = px.histogram(data_frame=carry_away_coupon_data, x='coupon_accepted', color='time', title='Carry Away Coupons Accepted Rate by Time')
fig.write_image("images/carry_away_coupons_time.png", format="png", width=800, height=600, scale=2)
fig.show()
```

![png](images/carry_away_coupons_time.png)




```python
# add a new column to the carry_away_coupon_data to indicate if the person never did carry away a month or more
print(f'{'----------------------------------------------------------------------'}')
print(f'{'Distinct carry_away values'} : {carry_away_coupon_data['carry_away'].unique()}')
print(f'{'----------------------------------------------------------------------\n\n'}')

carry_away_coupon_data['carry_away_visit_never'] = carry_away_coupon_data.apply(lambda row: ( (row['carry_away'] not in ['never'])), axis=1)


carry_away_coupon_accepted_never_visits = carry_away_coupon_data.groupby('carry_away_visit_never')['coupon_accepted'].mean()

print(carry_away_coupon_accepted_never_visits)

print(f'{'Those never visited '}: {carry_away_coupon_accepted_never_visits[True]:.2%}')
print(f'{'Those visited atleast once'}: {carry_away_coupon_accepted_never_visits[False]:.2%}')

fig = px.histogram(data_frame=carry_away_coupon_data, x='coupon_accepted', color='carry_away_visit_never')
fig.write_image("images/carry_away_coupons_never.png", format="png", width=800, height=600, scale=2)
fig.show()
carry_away_coupon_data.head()
carry_away_coupon_data.info()

```

    ----------------------------------------------------------------------
    Distinct carry_away values : ['4~8' '1~3' 'gt8' 'less1' 'never']
    ----------------------------------------------------------------------
    
    
    carry_away_visit_never
    False    0.785714
    True     0.737123
    Name: coupon_accepted, dtype: float64
    Those never visited : 73.71%
    Those visited atleast once: 78.57%

![png](images/carry_away_coupons_never.png)



    <class 'pandas.core.frame.DataFrame'>
    Index: 2280 entries, 25 to 12680
    Data columns (total 23 columns):
     #   Column                   Non-Null Count  Dtype 
    ---  ------                   --------------  ----- 
     0   destination              2280 non-null   object
     1   passanger                2280 non-null   object
     2   weather                  2280 non-null   object
     3   temperature              2280 non-null   int64 
     4   time                     2280 non-null   object
     5   coupon                   2280 non-null   object
     6   expiration               2280 non-null   object
     7   gender                   2280 non-null   object
     8   age                      2280 non-null   object
     9   marital_status           2280 non-null   object
     10  has_children             2280 non-null   int64 
     11  education                2280 non-null   object
     12  occupation               2280 non-null   object
     13  income                   2280 non-null   object
     14  bar                      2280 non-null   object
     15  coffee_house             2280 non-null   object
     16  carry_away               2280 non-null   object
     17  restaurant_less_than_20  2280 non-null   object
     18  restaurant_20_to_50      2280 non-null   object
     19  direction_same           2280 non-null   int64 
     20  direction_opp            2280 non-null   int64 
     21  coupon_accepted          2280 non-null   int64 
     22  carry_away_visit_never   2280 non-null   bool  
    dtypes: bool(1), int64(5), object(17)
    memory usage: 411.9+ KB



```python

```


```python
carry_away_coupon_data.head()

carry_away_coupon_data['alone_friends'] = carry_away_coupon_data.apply(lambda row: (row['passanger'] in ['Alone', 'Friend(s)']), axis=1)


carry_away_coupon_acceptance_based_passenger = carry_away_coupon_data.groupby('alone_friends')['coupon_accepted'].mean()

print(f'\033[1m{'Those accepted Carry Away coupons who are Alone or with Friend(s)'}\033[0m: \033[94m{carry_away_coupon_acceptance_based_passenger[True]:.2%}\033[0m')
print(f'\033[1m{'Those accepted Carry Away coupons who are not Alone or with Friend(s)'}\033[0m: \033[94m{carry_away_coupon_acceptance_based_passenger[False]:.2%}\033[0m')

fig = px.histogram(data_frame=carry_away_coupon_data, x='alone_friends', color='coupon_accepted', title='Carry out & Take Away Coupons Accepted Rate if the passenger is either Friend(s) or Alone')
fig.write_image("images/carry_away_coupons_alone_friends.png", format="png", width=800, height=600, scale=2)
fig.show()

```

    [1mThose accepted Carry Away coupons who are Alone or with Friend(s)[0m: [94m74.14%[0m
    [1mThose accepted Carry Away coupons who are not Alone or with Friend(s)[0m: [94m70.51%[0m

![png](images/carry_away_coupons_alone_friends.png)





```python




```

#### 3. Carry out & Take away Coupons Accepted Rate based on Expiration and Direction of Travel



```python
print(carry_away_coupon_data.groupby('expiration')['coupon_accepted'].mean())

fig = px.histogram(data_frame=carry_away_coupon_data, x='coupon_accepted', color='expiration', title='Carry out & Take away Coupons Accepted Rate based on the expiration')
fig.write_image("images/carry_away_coupons_expiration.png", format="png", width=800, height=600, scale=2)
fig.show()
```

    expiration
    1d    0.785612
    2h    0.662921
    Name: coupon_accepted, dtype: float64

![png](images/carry_away_coupons_expiration.png)





```python
print(carry_away_coupon_data.groupby('direction_same')['coupon_accepted'].mean())
fig = px.histogram(data_frame=carry_away_coupon_data, x='direction_same', color='coupon_accepted', title='Carry out & Take away Coupons Accepted Rate based on Travel in Same Direction')
fig.write_image("images/carry_away_coupons_direction_same.png", format="png", width=800, height=600, scale=2)
fig.show()

```

    direction_same
    0    0.753614
    1    0.705805
    Name: coupon_accepted, dtype: float64

![png](images/carry_away_coupons_direction_same.png)




```python
print(carry_away_coupon_data.query('direction_same==1').groupby('expiration')['coupon_accepted'].mean())
fig = px.histogram(data_frame=carry_away_coupon_data.query('direction_same==1'), x='expiration', color='coupon_accepted', title='Carry out & Take away Coupons Accepted Rate based on travel in Same Direction and Expiration')
fig.write_image("images/carry_away_coupons_direction_same_expiration.png", format="png", width=800, height=600, scale=2)
fig.show()

```

    expiration
    1d    0.771513
    2h    0.653207
    Name: coupon_accepted, dtype: float64

![png](images/carry_away_coupons_direction_same_expiration.png)




####  2. How about when we are traveling in the opposite direction



```python
print(carry_away_coupon_data.groupby('direction_opp')['coupon_accepted'].mean())
fig = px.histogram(data_frame=carry_away_coupon_data, x='direction_opp', color='coupon_accepted', title='Carry out & Take away Coupons Accepted Rate based on Travel in Opposite Direction')
fig.write_image("images/carry_away_coupons_direction_opp.png", format="png", width=800, height=600, scale=2)
fig.show()
```

    direction_opp
    0    0.705805
    1    0.753614
    Name: coupon_accepted, dtype: float64


![png](images/carry_away_coupons_direction_opp.png)



```python
print(carry_away_coupon_data.query('direction_opp==1').groupby('expiration')['coupon_accepted'].mean())
fig = px.histogram(data_frame=carry_away_coupon_data.query('direction_opp==1'), x='expiration', color='coupon_accepted', title='Carry out & Take away Coupons Accepted Rate based on travel in Opposite Direction and Expiration')
fig.write_image("images/carry_away_coupons_direction_opp_expiration.png", format="png", width=800, height=600, scale=2)
fig.show()
```

    expiration
    1d    0.790123
    2h    0.671642
    Name: coupon_accepted, dtype: float64

![png](images/carry_away_coupons_direction_opp_expiration.png)



```python

```

#### Conclusion: 
##### 1. The drivers, who travel alone have higher chances of accepting the Carry Away coupon, then who are with friends.
##### 2. The drivers, once they accept the coupon there is higher chance they gonna use it.
##### 3. at 2PM, less likely to have accept the coupons
##### 4. Coupons of expiration of 1 day have higer chance of acceptance, especially when they are travelling in opposite direction


```python

```


```python

```


```python

```

