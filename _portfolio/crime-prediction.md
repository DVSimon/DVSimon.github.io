---
title: "SF Crime Prediction"
excerpt: "ML application to predict crime categories of incoming police reports using surrounding details."
header:
  image:
  teaser: assets/images/xg-boost.png
sidebar:
  - title: "Role"
    image: /assets/images/dennis-simon.jpg
    image_alt: "logo"
    text: "ML Engineer, Data Retrieval and Processing"
  - title: "Responsibilities"
    text: "Gather dataset, preprocess and model for crime categorization with XGBoost."

---

## Technologies

Python, XGBoost, pandas, sklearn, scipy, AWS


### Overview

+ This project aims to predict crime categories of incoming police reports using surrounding details.
+ Pipeline to take data, preprocess and then model for future use.
+ Predicts amongst several categories based on data from a public dataset.
+ Utilizes AWS server to perform prediction pipeline.
+ Uses XGBoost modelling for predictions.
+ [Github](https://github.com/DVSimon/XGB-SF-Crime-Categorization/blob/master/XGB%20Source%20Code/Jupyter_NB_versions/Bayview%20XGB.ipynb)
+ [Paper](https://docs.google.com/document/d/12B4P1gxNrvLgvPf2NmKznbYKxtoVjEHwOUwRQ6MS54g/edit?usp=sharing)


### Dataset

+ Dataset can be found [here](https://data.sfgov.org/Public-Safety/Police-Department-Incident-Reports-Historical-2003/tmnf-yvry).
+ Contains datasets from each various district in SF in CSV format.
+ Each dataset contains data from 2005-present with 1,000,000 rows for each.
+ Columns include: Incident number, category, description, day of week, date, time, district, resolution, x/y coordinates, address, location, police department ID

### Preprocessing
+ Read CSV
```python
df_district = pd.read_csv('/home/ubuntu/CSVs/CENTRAL_data.csv') #change this city for csv for whatever district being done
df_district = df_district.drop(columns=['pddistrict', 'incidntnum', 'pdid', 'location', 'descript'])
```
+ Convert date into day, month, year, hour columns
```python
def convert_date_to_day(dt):

   result = re.findall(r'\d{4}-(\d{2})-(\d{2})T00:00:00.000',dt)

   return result[0][1]

def convert_date_to_month(dt):

  result = re.findall(r'\d{4}-(\d{2})-(\d{2})T00:00:00.000',dt)

  return result[0][0]
```

+ Move category into seperate DF as it is to be predicted and not input.
```python
df_initial = df_initial[df_initial['SPOSTMIN'] != -999]
```
+ Merge posted and actual wait time columns since only one is ever present, if actual wait time is present then keep this as it's more important.
```python
df_x = df_district.drop(columns=['category'])
```
+ Label encode categorical columns that contain relationships (Also tried one hot encoding, worse results)
+ OHE example
```python
onehot_encoder_DoW = OneHotEncoder(sparse = False)
DoW_feature = onehot_encoder_DoW.fit_transform(DoW_feature)
```
+Label Encoding
```python
labelencoder = LabelEncoder()
labelencoder = labelencoder.fit(df_y)
labelencoded_y = labelencoder.transform(df_y)
df_x['day'] = df_x.date.apply(lambda x: convert_date_to_day(x))
df_x['month'] = df_x.date.apply(lambda x: convert_date_to_month(x))
df_x['year'] = df_x.date.apply(lambda x: convert_date_to_year(x))
df_x['hour'] = df_x.time.apply(lambda x: convert_time_to_hour(x))
df_x = df_x.drop(columns=['date', 'time'])
df_x['day'] = (df_x['day']).astype(int)
df_x['month'] = (df_x['month']).astype(int)
df_x['year'] = (df_x['year']).astype(int)
df_x['hour'] = (df_x['hour']).astype(int)
label_encoder_addr = LabelEncoder()
addr_feature = label_encoder_addr.fit_transform(df_x.address.iloc[:].values)
addr_feature = addr_feature.reshape(df_x.shape[0], 1)
onehot_encoder_addr = OneHotEncoder(sparse = False)
addr_feature = onehot_encoder_addr.fit_transform(addr_feature)
label_encoder_DoW = LabelEncoder()
DoW_feature = label_encoder_DoW.fit_transform(df_x.dayofweek.iloc[:].values)
DoW_feature = DoW_feature.reshape(df_x.shape[0], 1)
onehot_encoder_DoW = OneHotEncoder(sparse = False)
DoW_feature = onehot_encoder_DoW.fit_transform(DoW_feature)
label_encoder_res = LabelEncoder()
res_feature = label_encoder_res.fit_transform(df_x.resolution.iloc[:].values)
res_feature = res_feature.reshape(df_x.shape[0], 1)
onehot_encoder_res = OneHotEncoder(sparse = False)
res_feature = onehot_encoder_res.fit_transform(res_feature)
```
+ Place into column list to be stacked and combined to form a new pandas dataframe with label encoded features and other features combined.
```python
columns = []
columns.append(addr_feature)
columns.append(DoW_feature)
columns.append(res_feature)
columns.append(x)
columns.append(y)
columns.append(day)
columns.append(month)
columns.append(year)
columns.append(hour)
encoded_feats = column_stack(columns)
sparse_features = sparse.csr_matrix(encoded_feats)
```

### Machine Learning/XGboost

+ Train test split
```python
X_train, X_test, y_train, y_test = train_test_split(sparse_features, labelencoded_y, test_size=0.20, random_state=random_seed)
```
+ Create parameter grid to tune XGRegression hyperparameters. Values taken based off suggested values from Jason Brownlees XGBoost book where #1 Kaggle modeller suggests.  
+ Use Stratified Kfold cross validation to validate while ensuring class presence due to imbalanced labels in dataset.
+ Utilize XGBoost with input dataset to create classification model with gpu boosting.
```python
model = XGBClassifier(nthread = n_threads) #or -1
kfold = StratifiedKFold(n_splits=3, shuffle=True, random_state=random_seed)
param_grid = {'n_estimators': [120, 240, 360, 480], #random int btwn 100 and 500 - removed
              'learning_rate': stats.uniform(0.01, 0.08), #.01 + loc, range of .01+/-.08
              'max_depth': [2, 4, 6, 8], #tree depths to check
              'colsample_bytree': stats.uniform(0.3, 0.7) #btwn .1 and 1.0    
}
```
+ Use Randomized search to test hyperparater grid, optionally could use gridsearchCV but don't have processing resources to test each combination at the time.
+ Select best performing estimator from the randomized search and store model using pickle for deployment on flask server.
```python
rand_search = RandomizedSearchCV(model, param_distributions = param_grid, scoring = 'f1_micro', n_iter = 3, n_jobs=-1, verbose = 10, cv=kfold)
rand_result = rand_search.fit(X_train, y_train)
print("Best: %f using %s" % (rand_result.best_score_, rand_result.best_params_))
best_XGB_parameters = rand_result.best_estimator_
#INSERT CITY NAME FOR .DAT FILE
pickle.dump(best_XGB_parameters, open("xgb_CENTRAL.pickle.dat", 'wb')) #change pickle
```
