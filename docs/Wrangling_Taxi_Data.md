```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from shapely.geometry import Point
import geopandas as gpd
import folium
import pickle
```


```python
taxi_data = pd.read_csv('trip_data.csv')
```


```python
taxi_data.head()
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
      <th>VendorID</th>
      <th>tpep_pickup_datetime</th>
      <th>tpep_dropoff_datetime</th>
      <th>passenger_count</th>
      <th>trip_distance</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>RateCodeID</th>
      <th>store_and_fwd_flag</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>payment_type</th>
      <th>fare_amount</th>
      <th>extra</th>
      <th>mta_tax</th>
      <th>tip_amount</th>
      <th>tolls_amount</th>
      <th>improvement_surcharge</th>
      <th>total_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2015-01-15 19:05:39</td>
      <td>2015-01-15 19:23:42</td>
      <td>1</td>
      <td>1.59</td>
      <td>-73.993896</td>
      <td>40.750111</td>
      <td>1</td>
      <td>N</td>
      <td>-73.974785</td>
      <td>40.750618</td>
      <td>1</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>3.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:53:28</td>
      <td>1</td>
      <td>3.30</td>
      <td>-74.001648</td>
      <td>40.724243</td>
      <td>1</td>
      <td>N</td>
      <td>-73.994415</td>
      <td>40.759109</td>
      <td>1</td>
      <td>14.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>2.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:43:41</td>
      <td>1</td>
      <td>1.80</td>
      <td>-73.963341</td>
      <td>40.802788</td>
      <td>1</td>
      <td>N</td>
      <td>-73.951820</td>
      <td>40.824413</td>
      <td>2</td>
      <td>9.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>10.80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:35:31</td>
      <td>1</td>
      <td>0.50</td>
      <td>-74.009087</td>
      <td>40.713818</td>
      <td>1</td>
      <td>N</td>
      <td>-74.004326</td>
      <td>40.719986</td>
      <td>2</td>
      <td>3.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>4.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:52:58</td>
      <td>1</td>
      <td>3.00</td>
      <td>-73.971176</td>
      <td>40.762428</td>
      <td>1</td>
      <td>N</td>
      <td>-74.004181</td>
      <td>40.742653</td>
      <td>2</td>
      <td>15.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>16.30</td>
    </tr>
  </tbody>
</table>
</div>




```python
taxi_data[['tpep_pickup_datetime', 'tpep_dropoff_datetime']]
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
      <th>tpep_pickup_datetime</th>
      <th>tpep_dropoff_datetime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2015-01-15 19:05:39</td>
      <td>2015-01-15 19:23:42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:53:28</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:43:41</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:35:31</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:52:58</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12748981</th>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:05:40</td>
    </tr>
    <tr>
      <th>12748982</th>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:07:26</td>
    </tr>
    <tr>
      <th>12748983</th>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:15:01</td>
    </tr>
    <tr>
      <th>12748984</th>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:17:03</td>
    </tr>
    <tr>
      <th>12748985</th>
      <td>2015-01-10 19:01:45</td>
      <td>2015-01-10 19:07:33</td>
    </tr>
  </tbody>
</table>
<p>12748986 rows × 2 columns</p>
</div>




```python
# Make time names more clear
taxi_data = taxi_data.rename(columns={
    'tpep_pickup_datetime': 'pickup_time',
    'tpep_dropoff_datetime': 'drop_off_time'
})
```


```python
taxi_data
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
      <th>VendorID</th>
      <th>pickup_time</th>
      <th>drop_off_time</th>
      <th>passenger_count</th>
      <th>trip_distance</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>RateCodeID</th>
      <th>store_and_fwd_flag</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>payment_type</th>
      <th>fare_amount</th>
      <th>extra</th>
      <th>mta_tax</th>
      <th>tip_amount</th>
      <th>tolls_amount</th>
      <th>improvement_surcharge</th>
      <th>total_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2015-01-15 19:05:39</td>
      <td>2015-01-15 19:23:42</td>
      <td>1</td>
      <td>1.59</td>
      <td>-73.993896</td>
      <td>40.750111</td>
      <td>1</td>
      <td>N</td>
      <td>-73.974785</td>
      <td>40.750618</td>
      <td>1</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>3.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:53:28</td>
      <td>1</td>
      <td>3.30</td>
      <td>-74.001648</td>
      <td>40.724243</td>
      <td>1</td>
      <td>N</td>
      <td>-73.994415</td>
      <td>40.759109</td>
      <td>1</td>
      <td>14.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>2.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:43:41</td>
      <td>1</td>
      <td>1.80</td>
      <td>-73.963341</td>
      <td>40.802788</td>
      <td>1</td>
      <td>N</td>
      <td>-73.951820</td>
      <td>40.824413</td>
      <td>2</td>
      <td>9.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>10.80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:35:31</td>
      <td>1</td>
      <td>0.50</td>
      <td>-74.009087</td>
      <td>40.713818</td>
      <td>1</td>
      <td>N</td>
      <td>-74.004326</td>
      <td>40.719986</td>
      <td>2</td>
      <td>3.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>4.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:52:58</td>
      <td>1</td>
      <td>3.00</td>
      <td>-73.971176</td>
      <td>40.762428</td>
      <td>1</td>
      <td>N</td>
      <td>-74.004181</td>
      <td>40.742653</td>
      <td>2</td>
      <td>15.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>16.30</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12748981</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:05:40</td>
      <td>2</td>
      <td>1.00</td>
      <td>-73.951988</td>
      <td>40.786217</td>
      <td>1</td>
      <td>N</td>
      <td>-73.953735</td>
      <td>40.775162</td>
      <td>1</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>1.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>7.55</td>
    </tr>
    <tr>
      <th>12748982</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:07:26</td>
      <td>2</td>
      <td>0.80</td>
      <td>-73.982742</td>
      <td>40.728184</td>
      <td>1</td>
      <td>N</td>
      <td>-73.974976</td>
      <td>40.720013</td>
      <td>1</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>8.80</td>
    </tr>
    <tr>
      <th>12748983</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:15:01</td>
      <td>1</td>
      <td>3.40</td>
      <td>-73.979324</td>
      <td>40.749550</td>
      <td>1</td>
      <td>N</td>
      <td>-73.969101</td>
      <td>40.787800</td>
      <td>2</td>
      <td>13.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>14.30</td>
    </tr>
    <tr>
      <th>12748984</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:17:03</td>
      <td>1</td>
      <td>1.30</td>
      <td>-73.999565</td>
      <td>40.738483</td>
      <td>1</td>
      <td>N</td>
      <td>-73.981819</td>
      <td>40.737652</td>
      <td>1</td>
      <td>10.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>13.55</td>
    </tr>
    <tr>
      <th>12748985</th>
      <td>1</td>
      <td>2015-01-10 19:01:45</td>
      <td>2015-01-10 19:07:33</td>
      <td>1</td>
      <td>0.70</td>
      <td>-73.960350</td>
      <td>40.766399</td>
      <td>1</td>
      <td>N</td>
      <td>-73.968643</td>
      <td>40.760777</td>
      <td>2</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>6.30</td>
    </tr>
  </tbody>
</table>
<p>12748986 rows × 19 columns</p>
</div>




```python
# Convert columns to datetime format
taxi_data['pickup_time'] = pd.to_datetime(taxi_data['pickup_time'])
taxi_data['drop_off_time'] = pd.to_datetime(taxi_data['drop_off_time'])

# Display the data types to confirm the changes
taxi_data.dtypes
```




    VendorID                          int64
    pickup_time              datetime64[ns]
    drop_off_time            datetime64[ns]
    passenger_count                   int64
    trip_distance                   float64
    pickup_longitude                float64
    pickup_latitude                 float64
    RateCodeID                        int64
    store_and_fwd_flag               object
    dropoff_longitude               float64
    dropoff_latitude                float64
    payment_type                      int64
    fare_amount                     float64
    extra                           float64
    mta_tax                         float64
    tip_amount                      float64
    tolls_amount                    float64
    improvement_surcharge           float64
    total_amount                    float64
    dtype: object




```python
taxi_data['pickup_time']
```




    0          2015-01-15 19:05:39
    1          2015-01-10 20:33:38
    2          2015-01-10 20:33:38
    3          2015-01-10 20:33:39
    4          2015-01-10 20:33:39
                       ...        
    12748981   2015-01-10 19:01:44
    12748982   2015-01-10 19:01:44
    12748983   2015-01-10 19:01:44
    12748984   2015-01-10 19:01:44
    12748985   2015-01-10 19:01:45
    Name: pickup_time, Length: 12748986, dtype: datetime64[ns]




```python
taxi_data.isna().sum()
```




    VendorID                 0
    pickup_time              0
    drop_off_time            0
    passenger_count          0
    trip_distance            0
    pickup_longitude         0
    pickup_latitude          0
    RateCodeID               0
    store_and_fwd_flag       0
    dropoff_longitude        0
    dropoff_latitude         0
    payment_type             0
    fare_amount              0
    extra                    0
    mta_tax                  0
    tip_amount               0
    tolls_amount             0
    improvement_surcharge    3
    total_amount             0
    dtype: int64




```python
# Drop three duplcates
taxi_data.dropna()
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
      <th>VendorID</th>
      <th>pickup_time</th>
      <th>drop_off_time</th>
      <th>passenger_count</th>
      <th>trip_distance</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>RateCodeID</th>
      <th>store_and_fwd_flag</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>payment_type</th>
      <th>fare_amount</th>
      <th>extra</th>
      <th>mta_tax</th>
      <th>tip_amount</th>
      <th>tolls_amount</th>
      <th>improvement_surcharge</th>
      <th>total_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2015-01-15 19:05:39</td>
      <td>2015-01-15 19:23:42</td>
      <td>1</td>
      <td>1.59</td>
      <td>-73.993896</td>
      <td>40.750111</td>
      <td>1</td>
      <td>N</td>
      <td>-73.974785</td>
      <td>40.750618</td>
      <td>1</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>3.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:53:28</td>
      <td>1</td>
      <td>3.30</td>
      <td>-74.001648</td>
      <td>40.724243</td>
      <td>1</td>
      <td>N</td>
      <td>-73.994415</td>
      <td>40.759109</td>
      <td>1</td>
      <td>14.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>2.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:43:41</td>
      <td>1</td>
      <td>1.80</td>
      <td>-73.963341</td>
      <td>40.802788</td>
      <td>1</td>
      <td>N</td>
      <td>-73.951820</td>
      <td>40.824413</td>
      <td>2</td>
      <td>9.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>10.80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:35:31</td>
      <td>1</td>
      <td>0.50</td>
      <td>-74.009087</td>
      <td>40.713818</td>
      <td>1</td>
      <td>N</td>
      <td>-74.004326</td>
      <td>40.719986</td>
      <td>2</td>
      <td>3.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>4.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:52:58</td>
      <td>1</td>
      <td>3.00</td>
      <td>-73.971176</td>
      <td>40.762428</td>
      <td>1</td>
      <td>N</td>
      <td>-74.004181</td>
      <td>40.742653</td>
      <td>2</td>
      <td>15.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>16.30</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12748981</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:05:40</td>
      <td>2</td>
      <td>1.00</td>
      <td>-73.951988</td>
      <td>40.786217</td>
      <td>1</td>
      <td>N</td>
      <td>-73.953735</td>
      <td>40.775162</td>
      <td>1</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>1.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>7.55</td>
    </tr>
    <tr>
      <th>12748982</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:07:26</td>
      <td>2</td>
      <td>0.80</td>
      <td>-73.982742</td>
      <td>40.728184</td>
      <td>1</td>
      <td>N</td>
      <td>-73.974976</td>
      <td>40.720013</td>
      <td>1</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>8.80</td>
    </tr>
    <tr>
      <th>12748983</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:15:01</td>
      <td>1</td>
      <td>3.40</td>
      <td>-73.979324</td>
      <td>40.749550</td>
      <td>1</td>
      <td>N</td>
      <td>-73.969101</td>
      <td>40.787800</td>
      <td>2</td>
      <td>13.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>14.30</td>
    </tr>
    <tr>
      <th>12748984</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:17:03</td>
      <td>1</td>
      <td>1.30</td>
      <td>-73.999565</td>
      <td>40.738483</td>
      <td>1</td>
      <td>N</td>
      <td>-73.981819</td>
      <td>40.737652</td>
      <td>1</td>
      <td>10.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>13.55</td>
    </tr>
    <tr>
      <th>12748985</th>
      <td>1</td>
      <td>2015-01-10 19:01:45</td>
      <td>2015-01-10 19:07:33</td>
      <td>1</td>
      <td>0.70</td>
      <td>-73.960350</td>
      <td>40.766399</td>
      <td>1</td>
      <td>N</td>
      <td>-73.968643</td>
      <td>40.760777</td>
      <td>2</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>6.30</td>
    </tr>
  </tbody>
</table>
<p>12748983 rows × 19 columns</p>
</div>




```python
taxi_data.duplicated().sum()
```




    383




```python
duplicate_rows = taxi_data[taxi_data.duplicated()]
```

### These anamolies should be removed as they have no data worth keeping


```python
duplicate_rows
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
      <th>VendorID</th>
      <th>pickup_time</th>
      <th>drop_off_time</th>
      <th>passenger_count</th>
      <th>trip_distance</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>RateCodeID</th>
      <th>store_and_fwd_flag</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>payment_type</th>
      <th>fare_amount</th>
      <th>extra</th>
      <th>mta_tax</th>
      <th>tip_amount</th>
      <th>tolls_amount</th>
      <th>improvement_surcharge</th>
      <th>total_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>248333</th>
      <td>2</td>
      <td>2015-01-05 09:39:49</td>
      <td>2015-01-05 09:39:52</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>248334</th>
      <td>2</td>
      <td>2015-01-05 09:39:49</td>
      <td>2015-01-05 09:39:52</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>300143</th>
      <td>2</td>
      <td>2015-01-05 09:39:49</td>
      <td>2015-01-05 09:39:52</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>300144</th>
      <td>2</td>
      <td>2015-01-05 09:39:49</td>
      <td>2015-01-05 09:39:52</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>300145</th>
      <td>2</td>
      <td>2015-01-05 09:39:49</td>
      <td>2015-01-05 09:39:52</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12242582</th>
      <td>2</td>
      <td>2015-01-14 13:20:13</td>
      <td>2015-01-14 13:20:33</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>2.8</td>
    </tr>
    <tr>
      <th>12242583</th>
      <td>2</td>
      <td>2015-01-14 13:20:13</td>
      <td>2015-01-14 13:20:33</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>2.8</td>
    </tr>
    <tr>
      <th>12242584</th>
      <td>2</td>
      <td>2015-01-14 13:20:13</td>
      <td>2015-01-14 13:20:33</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>2.8</td>
    </tr>
    <tr>
      <th>12242585</th>
      <td>2</td>
      <td>2015-01-14 13:20:13</td>
      <td>2015-01-14 13:20:33</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>2.8</td>
    </tr>
    <tr>
      <th>12242586</th>
      <td>2</td>
      <td>2015-01-14 13:20:13</td>
      <td>2015-01-14 13:20:33</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>N</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>2.8</td>
    </tr>
  </tbody>
</table>
<p>383 rows × 19 columns</p>
</div>




```python
taxi_data.drop_duplicates()
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
      <th>VendorID</th>
      <th>pickup_time</th>
      <th>drop_off_time</th>
      <th>passenger_count</th>
      <th>trip_distance</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>RateCodeID</th>
      <th>store_and_fwd_flag</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>payment_type</th>
      <th>fare_amount</th>
      <th>extra</th>
      <th>mta_tax</th>
      <th>tip_amount</th>
      <th>tolls_amount</th>
      <th>improvement_surcharge</th>
      <th>total_amount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2015-01-15 19:05:39</td>
      <td>2015-01-15 19:23:42</td>
      <td>1</td>
      <td>1.59</td>
      <td>-73.993896</td>
      <td>40.750111</td>
      <td>1</td>
      <td>N</td>
      <td>-73.974785</td>
      <td>40.750618</td>
      <td>1</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>3.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:53:28</td>
      <td>1</td>
      <td>3.30</td>
      <td>-74.001648</td>
      <td>40.724243</td>
      <td>1</td>
      <td>N</td>
      <td>-73.994415</td>
      <td>40.759109</td>
      <td>1</td>
      <td>14.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>2.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:43:41</td>
      <td>1</td>
      <td>1.80</td>
      <td>-73.963341</td>
      <td>40.802788</td>
      <td>1</td>
      <td>N</td>
      <td>-73.951820</td>
      <td>40.824413</td>
      <td>2</td>
      <td>9.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>10.80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:35:31</td>
      <td>1</td>
      <td>0.50</td>
      <td>-74.009087</td>
      <td>40.713818</td>
      <td>1</td>
      <td>N</td>
      <td>-74.004326</td>
      <td>40.719986</td>
      <td>2</td>
      <td>3.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>4.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:52:58</td>
      <td>1</td>
      <td>3.00</td>
      <td>-73.971176</td>
      <td>40.762428</td>
      <td>1</td>
      <td>N</td>
      <td>-74.004181</td>
      <td>40.742653</td>
      <td>2</td>
      <td>15.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>16.30</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12748981</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:05:40</td>
      <td>2</td>
      <td>1.00</td>
      <td>-73.951988</td>
      <td>40.786217</td>
      <td>1</td>
      <td>N</td>
      <td>-73.953735</td>
      <td>40.775162</td>
      <td>1</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>1.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>7.55</td>
    </tr>
    <tr>
      <th>12748982</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:07:26</td>
      <td>2</td>
      <td>0.80</td>
      <td>-73.982742</td>
      <td>40.728184</td>
      <td>1</td>
      <td>N</td>
      <td>-73.974976</td>
      <td>40.720013</td>
      <td>1</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>8.80</td>
    </tr>
    <tr>
      <th>12748983</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:15:01</td>
      <td>1</td>
      <td>3.40</td>
      <td>-73.979324</td>
      <td>40.749550</td>
      <td>1</td>
      <td>N</td>
      <td>-73.969101</td>
      <td>40.787800</td>
      <td>2</td>
      <td>13.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>14.30</td>
    </tr>
    <tr>
      <th>12748984</th>
      <td>1</td>
      <td>2015-01-10 19:01:44</td>
      <td>2015-01-10 19:17:03</td>
      <td>1</td>
      <td>1.30</td>
      <td>-73.999565</td>
      <td>40.738483</td>
      <td>1</td>
      <td>N</td>
      <td>-73.981819</td>
      <td>40.737652</td>
      <td>1</td>
      <td>10.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>2.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>13.55</td>
    </tr>
    <tr>
      <th>12748985</th>
      <td>1</td>
      <td>2015-01-10 19:01:45</td>
      <td>2015-01-10 19:07:33</td>
      <td>1</td>
      <td>0.70</td>
      <td>-73.960350</td>
      <td>40.766399</td>
      <td>1</td>
      <td>N</td>
      <td>-73.968643</td>
      <td>40.760777</td>
      <td>2</td>
      <td>5.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>6.30</td>
    </tr>
  </tbody>
</table>
<p>12748603 rows × 19 columns</p>
</div>




```python
# This column is irelevant
taxi_data.drop(columns=['store_and_fwd_flag'], inplace=True)
```


```python
# Create trip duration column
taxi_data['Trip_Duration'] =  taxi_data['drop_off_time'] - taxi_data['pickup_time']
```


```python
taxi_data['Trip_Duration'] = pd.to_timedelta(taxi_data['Trip_Duration'])
# Calculate total seconds
taxi_data['Total_Seconds'] = taxi_data['Trip_Duration'].dt.total_seconds()

# Convert seconds to minutes
taxi_data['Trip_Duration_in Minutes'] = taxi_data['Total_Seconds'] / 60

# Drop the 'Total Seconds' column if it's not needed
taxi_data = taxi_data.drop(columns=['Total_Seconds'])

```


```python
# Duplicate Column removed
taxi_data.drop(columns=['Trip_Duration'], inplace=True)
```


```python
# Mapping of numbers to payment method names
payment_method_mapping = {
    1: 'Credit card',
    2: 'Cash',
    3: 'No charge',
    4: 'Dispute',
    5: 'Unknown',
    6: 'Voided trip'
}

# Replace the numbers with the corresponding names
taxi_data['PaymentMethod'] = taxi_data['payment_type'].replace(payment_method_mapping)
```


```python
taxi_data.head()
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
      <th>VendorID</th>
      <th>pickup_time</th>
      <th>drop_off_time</th>
      <th>passenger_count</th>
      <th>trip_distance</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>RateCodeID</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>payment_type</th>
      <th>fare_amount</th>
      <th>extra</th>
      <th>mta_tax</th>
      <th>tip_amount</th>
      <th>tolls_amount</th>
      <th>improvement_surcharge</th>
      <th>total_amount</th>
      <th>Trip_Duration_in Minutes</th>
      <th>PaymentMethod</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2015-01-15 19:05:39</td>
      <td>2015-01-15 19:23:42</td>
      <td>1</td>
      <td>1.59</td>
      <td>-73.993896</td>
      <td>40.750111</td>
      <td>1</td>
      <td>-73.974785</td>
      <td>40.750618</td>
      <td>1</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>3.25</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.05</td>
      <td>18.050000</td>
      <td>Credit card</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:53:28</td>
      <td>1</td>
      <td>3.30</td>
      <td>-74.001648</td>
      <td>40.724243</td>
      <td>1</td>
      <td>-73.994415</td>
      <td>40.759109</td>
      <td>1</td>
      <td>14.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>2.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>17.80</td>
      <td>19.833333</td>
      <td>Credit card</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2015-01-10 20:33:38</td>
      <td>2015-01-10 20:43:41</td>
      <td>1</td>
      <td>1.80</td>
      <td>-73.963341</td>
      <td>40.802788</td>
      <td>1</td>
      <td>-73.951820</td>
      <td>40.824413</td>
      <td>2</td>
      <td>9.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>10.80</td>
      <td>10.050000</td>
      <td>Cash</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:35:31</td>
      <td>1</td>
      <td>0.50</td>
      <td>-74.009087</td>
      <td>40.713818</td>
      <td>1</td>
      <td>-74.004326</td>
      <td>40.719986</td>
      <td>2</td>
      <td>3.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>4.80</td>
      <td>1.866667</td>
      <td>Cash</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2015-01-10 20:33:39</td>
      <td>2015-01-10 20:52:58</td>
      <td>1</td>
      <td>3.00</td>
      <td>-73.971176</td>
      <td>40.762428</td>
      <td>1</td>
      <td>-74.004181</td>
      <td>40.742653</td>
      <td>2</td>
      <td>15.0</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>16.30</td>
      <td>19.316667</td>
      <td>Cash</td>
    </tr>
  </tbody>
</table>
</div>




```python
taxi_data.to_pickle('wrangling_taxi.pkl')
```


```python
taxi_data.to_csv('cleaned_taxi_data')
```


```python

```
