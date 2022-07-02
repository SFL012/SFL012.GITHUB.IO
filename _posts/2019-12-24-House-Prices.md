---
layout: post
title: "Machine Learning - House Prices"
description: "Predicted house prices by using the k-nearest neighbors algorithm."
categories: [DataScience]
tags: [Machine Learning, Python]
redirect_from:
  - /2019/12/24/
---

## Machine Learning


### House Prices
The house prices in Douglas County are not just driven by having nearby grocery stores. Rather, they are driven by access to the right quantity, mix, and quality of community features. To further interpret the [real estate data](https://www.douglas.co.us/assessor/data-downloads) from Douglas County, I extracted, transformed, and loaded (ETL) the raw transaction-level and property-level information. Then, I created efficient features/variables from raw data via data mining techniques. Finally, I Predicted the house prices in Douglas County by applying the k-nearest neighbors algorithm (k-NN) to have powerful insights into the real estate data.

### Extract, Transform, Load (ETL)

- Data Loading

```python
from pandas import read_csv
from numpy import int64

trn_src = read_csv(
    'TrainingData.csv',
    encoding='ISO-8859-1',
    
    keep_default_na=False,
    na_values=[
        'NaN', 
        '',
    ],
    
    parse_dates=[
        'transaction_date',
    ],
    
    dtype={
        'address_number': str,
        'unit_no': str,
        'land_economic_area_code': str,
        'neighborhood_code': str,
    },
)

data = trn_src.copy()
```

- None Value

```python
from numpy import nan
data['remodeled_year'].replace(0, nan, inplace=True)
data['average_story_height'].replace([0, 1, 2, 3], nan, inplace=True)
data['no_of_bedroom'].replace(0, nan, inplace=True)
```

- Boolean Column

```python
flag = {
    'Y': True,
    'N': False,
}

data['vacant_flag'] = data['vacant_flag'].map(flag)
data['walkout_basement_flag'] = data['walkout_basement_flag'].map(flag)
```

- Data Saving

```python
from pickle import dump

with open('etl.pickle', 'wb') as f:
    dump(data, f)
```

### Feature Engineering 

- Data Loading

```python
from pickle import load

with open('etl.pickle', 'rb') as f:
    data = load(f)
```

- CPI

```python
from pandas import read_csv

cpi_src = read_csv('CPI.csv', index_col='Year')
def get_cpi(arr):
    date = arr['transaction_date']
    mo = cpi_src.columns[date.month - 1]
    return cpi_src.loc[date.year, mo]

data['cpi'] = data.apply(get_cpi, axis=1)

data[['transaction_date', 'cpi']].tail()
```

- CPI Adjuestment

```python
def adj_price(arr):
    return arr['sale_price'] / arr['cpi']

data['cpi_adj_price'] = data.apply(adj_price, axis=1)

data[['transaction_date', 'cpi', 'sale_price', 'cpi_adj_price']].tail()
```

- Wear and Tear

```python
def get_abrasion(arr):
    trans_year = arr['transaction_date'].year
    
    trans_month = arr['transaction_date'].month
    if trans_month > 6:
        trans_year += 1
    
    last_modeled = max(arr['built_year'], arr['remodeled_year'])
    
    return trans_year - last_modeled

data['abrasion'] = data.apply(get_abrasion, axis=1)

data[['transaction_date', 'built_year', 'remodeled_year', 'abrasion']].head()
```

- Room Size

```python
data['room_size'] = data['built_as_sf'] / data['no_of_bedroom']

data[['built_as_sf', 'no_of_bedroom', 'room_size']].tail()
```

- House Location

```python
from functools import lru_cache
from uszipcode import SearchEngine

search = SearchEngine()

@lru_cache(maxsize=2048)
def zip_info(zipcode):
    info = search.by_zipcode(zipcode)
    return info

def get_lat(arr):
    info = zip_info(arr['location_zip_code'])
    return info.lat

def get_lng(arr):
    info = zip_info(arr['location_zip_code'])
    return info.lng

data['lat'] = data.apply(get_lat, axis=1)
data['lng'] = data.apply(get_lng, axis=1)

data[['location_zip_code', 'lat', 'lng']].tail()
```

- Local Median Income

```python
from functools import lru_cache
from uszipcode import SearchEngine

search = SearchEngine()

@lru_cache(maxsize=2048)
def zip_info(zipcode):
    info = search.by_zipcode(zipcode)
    return info

def get_household_income(arr):
    info = zip_info(arr['location_zip_code'])
    return info.median_household_income

data['area_income'] = data.apply(get_household_income, axis=1)

data[['location_zip_code', 'city_name', 'area_income']].head()
```

- Data Saving

```python
from pickle import dump

with open('fe.pickle', 'wb') as f:
    dump(data, f)
```

### The k-nearest neighbors algorithm (k-NN)

- Data Loading

```python
from pickle import load

with open('fe.pickle', 'rb') as f:
    src = load(f)
```

- Feature Selection

```python
feats = [
    'abrasion',
    'lat',
    'lng',
    #'area_income',
    
    'total_net_acres',
    'built_as_sf',
    'total_garage_sf',
    
    'room_size',
    #'no_of_bedroom',
    'no_of_bathroom',    
]
```

```python
def fill_median(data, col):
    data[col].fillna(data[col].median(), inplace=True)

data = src.copy()
for col in feats:
    fill_median(data, col)
    
data_trn = data[data['predict']==0][['cpi_adj_price'] + feats]
```

- Training & Validation

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler().fit(data[feats])

X = scaler.transform(data_trn[feats])
y = data_trn[['cpi_adj_price']]
```

```python
from sklearn.model_selection import train_test_split

X_trn, X_vld, y_trn, y_vld = train_test_split(
    X, y, test_size=0.01, random_state=666)
```

- Hyper-parameter Selection

```python
from sklearn.neighbors import KNeighborsRegressor

def adj_r2(m, X, y):
    return 1 - ( (1 - m.score(X, y)) * (X.shape[0] - 1) / (X.shape[0] - X.shape[1] - 1) )

best_k, best_r2 = 0, 0
for k in range(1, 10, 2):
    m = KNeighborsRegressor(n_neighbors=k).fit(X_trn, y_trn)
    
    r2 = adj_r2(m, X_vld, y_vld)
    if best_r2 < r2:
        best_k, best_r2 = k, r2

print(f'Adjuested R^2: {best_r2}')
```

- Price Prediction

```python
data_tst = data[data['predict']==1]
X_tst = scaler.transform(data_tst[feats])

m = KNeighborsRegressor(n_neighbors=best_k).fit(X, y)
p = m.predict(X_tst)

data_tst.loc[:, 'cpi_adj_price'] = p.T.tolist()[0]
prediction = data_tst.assign(sale_price=lambda arr: arr.cpi_adj_price * arr.cpi)

prediction[feats + ['cpi', 'cpi_adj_price', 'sale_price']].head(10)
```
### Result
According to the results, I could add more effective features/variables to the model. Since some variables are more significant than the others in the model, I could also use the weighted K-NN model or apply neural networks to optimize the results.
