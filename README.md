# HDBpricepredictor

## Project Motivation 
1) Help millenials determine worth of BTOS/HDB resales.
2) Reliable way to independently estimate values without relying on black box models used by real estate companies 

## Final Analysis
|                     Method                    | Best Buffer (Distance with respect to features) |                           R2 Value                           |
| :-------------------------------------------: | :---------------------------------------------: | :----------------------------------------------------------: |
|              Multi-linear Regression          |  750 |       0.82664       | 
|                 XGBoost              |  1500 |       0.85960      | 
|                 Support Vector Regression                |  750 |       0.89098      | 
|                 Neural Network               |  750 |       0.90125     | 
|                 Random Forest                 |  1500 |       0.93902 (**best model**)       | 

## Top 3 most important features for each model
|                     MLR                  | XGBoost  |   Support Vector Regression  |       Neural Network                  | Random Forest |
| :----------------: | :------------------------------------: | :------------------: | :----------------------------------------: | :---------------:|
|  Remaining Lease Month    |  Postal code |       Postal code     |  Postal code | Flat type: 3 room |
|  Town woodlands   |  Remaining Lease Month  |    Remaining Lease Month    | Remaining Lease Month  | Remaining Lease Month  |
| Town CCK | Distance to nearest MRT station | Preschool count | Distance to nearest MRT station | Flat type: 5 room |

### MLR
#### Pros
- Linear Regression is easier to implement, interpret and very efficient to train. 
- Able to extrapolate beyond a specific data set
#### Cons
- In the context of HDB flats, there’s a high possibility that independent variables might be correlated. It becomes difficult for the model to estimate the relationship between individual independent variable and the dependent variable independently because all independent variables tend to change in unison.
- Over simplifies many real world problems. Often in a realistic setting,  independent and dependent variables don’t exhibit a linear relationship.  In such cases, this method might not be as useful

### SVM
#### Pros
- Works even with non-linear data
- Effective with  higher dimension data

#### Cons
- Non-linear data might need to be converted to higher dimension first
- SVMs are not very efficient computationally, it take a very long time to train with large dataset
- SVMs does not provide probabilistic explanation for the classification.

### XGBoost
#### Pros
- Able to handle missing values internally. XGBoost algorithm will learn what is the best direction to go to when there are missing values. Do not require much feature engineering
- Faster execution time  compared to other Machine Learning algorithm due to parallel computation.


#### Cons
- Harder to tune due to more hyperparameters. Have to decide which hyperparameters to tune accordingly which takes time as there are a lot of hyperparameters to consider 
- Prone to overfitting if parameters are not tuned properly

### Random forest
#### Pros
- Can train models with relatively small number of samples with decent results
- Low computational cost of training 
- Generates an internal unbiased estimate of the generalization error as the forest building progresses
- Has effective method for estimating missing data
- maintains accuracy when a large proportion of the data are missing



#### Cons
- Ability to learn and model non-linear and complex relationships


### Random forest
#### Pros
- Overwhelmingly strong correlation between price & some feature will cause neural network to ignore the rest of the features
- Poor Interpretability




#### Cons
- Can take a long time to train with large number of trees
- Quickly reaches a point where more samples will not improve accuracy of model
- Random forests have been observed to overfit for some datasets with noisy classification/regression tasks
- Categorical variables with different number of levels: random forests are biased in favor of those attributes with more levels




## Model codes Folder Description:
- **MLR-Final.ipynb**: code for multi-linear regression model (change buffer 750/1500 within codes accordingly)
- **Neural_Network.ipynb**: code for neural network model (change buffer 750/1500 within codes accordingly)
- **Random Forest 750.ipynb**: code for random forest model with buffer 750
- **Random Forest 1500.ipynb**: code for random forest model with buffer 1500
- **Start Jupyter Notebook.bat**: file to start jupyter notebook from windows command line
- **SVR_750.ipynb**: code for SVR model with buffer 750
- **SVR_1500.ipynb**: code for SVR model with buffer 1500
- **xgmodel.ipynb**: code for XGBoost model (change buffer 750/1500 within codes accordingly)
- **y2015_to_2017_buffer_750.csv**: data from 2015 to 2017 with facilities range within 750m buffer
- **y2015_to_2017_buffer_1500.csv**: data from 2015 to 2017 with facilities range within 1500m buffer


## Data Processing Folder Structure

```
$ tree
.
├── merged
│
├── Processed
│   ├── Bus interchange
│   ├── List of Preschools
│   ├── Median Rent by Town and Flat Type
│   ├── School Directory and Information
│   └── Shopping malls
│
└── Raw
    ├── Bus interchange
    ├── Distance to MRT
    ├── Hawker centres
    ├── HDB resale
    ├── List of Preschools
    ├── Median Rent by Town and Flat Type
    ├── MP14_SUBZONE
    ├── NParks Parks
    ├── Rail stations
    ├── School Directory and Information
    ├── Shopping malls
    └── SportSG Sport Facilities
```

## Description

### `Processed`

- **Bus interchange**: Contains geotagged data on all bus interchange in Singapore generated from names from `Raw/Bus interchange` by `1. Data Preparation - Bus Interchange.Rmd`
- **Shopping malls**: Contains geotagged data on all shopping malls in Singapore generated from names from `Raw/Shopping malls` by `2. Data Preparation - Malls.Rmd`
- **List of Preschools**: Contains geotagged data on all bus interchanged in Singapore generated from names from `Raw/List of Preschools` by `3. Data Preparation - Pre-School.Rmd`
- **School Directory and Information**: Contains geotagged data on all bus interchanged in Singapore generated from names from `Raw/School Directory and Information` by `4. Data Preparation - School.Rmd`
- Median Rent by Town and Flat Type: Contains the aggregated median rent of each apartment type by town generated from `Raw/Median Rent by Town and Flat Type` by `5. Data Preparation - Medan rental.Rmd`

### `merged`

Contains the final files used for ML data modelling. Files are generated from both Raw data from `Raw` as well as intermediate processed data from `Processed` generated by `7. Merging_processed_files.Rmd`

### `Raw`

Contains raw files downloaded from the sources listed below.

|                     Title                     | Format |                           Remarks                            |                            Source                            |
| :-------------------------------------------: | :----: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|            list_of_bus_interchange            |  CSV   |            Names of all the bus stop in Singapore            | https://en.wikipedia.org/wiki/List_of_bus_stations_in_Singapore#Bus_interchanges_2 |
|                 street_to_MRT                 |  CSV   |       Distance from all postal code to the nearest MRT       | https://github.com/kohjiaxuan/Predicting-HDB-Price-with-Machine-Learning/blob/master/street_to_MRT.csv |
|              hawker-centres-kml               |  KML   |               Name & location of hawker centre               |          https://data.gov.sg/dataset/hawker-centres          |
| resale-flat-prices-based-on-registration-date |  CSV   |                 Resale HDB transacted prices                 |      https://data.gov.sg/dataset/hdb-resale-price-index      |
|           pre-schools-location-kml            |  KML   |                Name & location of pre-schools                | http://cloud.csiss.gmu.edu/uddi/dataset/pre-schools-location |
|       median-rent-by-town-and-flat-type       |  CSV   |          Median Rent by Town, Flat Type Per Quarter          | https://data.gov.sg/dataset/median-rent-by-town-and-flat-type |
|              MP14_SUBZONE_WEB_PL              |  SHP   | Master plan 2014 Subzone. To find out which postal is under what town |              https://data.gov.sg/dataset?q=MP14              |
|               nparks-parks-kml                |  KML   |         Name & location of Singapore National Parks          |           https://data.gov.sg/dataset/nparks-parks           |
|       master-plan-2014-rail-station-kml       |  KML   |            Name and location of MRT, LRT stations            |  https://data.gov.sg/dataset/master-plan-2014-rail-station   |
|        general-information-of-schools         |  CSV   |               School Directory and Information               | https://data.gov.sg/dataset/school-directory-and-information?view_id=ba7c477d-a077-4303-96a1-ac1d4f25b190&resource_id=ede26d32-01af-4228-b1ed-f05c45a1d8ee |
|            list_of_shopping_malls             |  CSV   |                    Name of shopping malls                    | https://en.wikipedia.org/wiki/List_of_shopping_malls_in_Singapore |
|         sportsg-sport-facilities-kml          |  KML   |           Name and location of SportSg facilities            |     https://data.gov.sg/dataset/sportsg-sport-facilities     |


