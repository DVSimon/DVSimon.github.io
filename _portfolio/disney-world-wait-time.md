---
title: "Disney World Ride Wait Time Predictions"
excerpt: "ML web application to predict wait times on Disney World rides in various parks and display in angular app."
header:
  image: /assets/images/disney-wait-image.png
  teaser: assets/images/xg-boost.png
sidebar:
  - title: "Role"
    image: /assets/images/dennis-simon.jpg
    image_alt: "logo"
    text: "ML Engineer, Data Retrieval and Processing"
  - title: "Responsibilities"
    text: "Gather dataset, preprocess and model for predicting wait times of rides on future dates."

---
## Disclaimer

All work is done for educational use and is not monetized, dataset taken from public website for non-commercial use and all rights belong to Disney.

## Technologies

Python, Angular, flask, HTML, CSS, XGBoost, Pandas, sklearn, Google Maps API, MongoDB


### Overview

+ This project aims to predict wait times of various Disney World rides/amusements on future dates that can be selected by the user.  
+ The application predicts rides for the entire selected date and displays a graph of wait times throughout the user inputted day.
+ Ride to be predicted can be selected on the map (Google Maps API) or on a list of rides that can be filtered by park, recently viewed, favorited or name.
+ Uses XGBoost modelling for predictions.
+ Mobile friendly.
+ User creation enabled.
+ [Github](https://github.com/DVSimon/Disney-World-Wait-Times/blob/master/server/server.py)


### Dataset

+ Dataset can be found [here](https://touringplans.com/blog/2018/06/25/disney-world-wait-times-available-for-data-science-and-machine-learning/).
+ Contains  various rides from Disney's Holleywood Studios, Disney's Animal Kingdom, Magic Kingdom and Epcot
+ Each ride is a seperate CSV with data from 2012 to present with each containing 300,000+ rows at the least.
+ Columns include: Datatime(mm/dd/yyyy, hh:mm), posted est wait time, exact wait time(if present, then posted wait time not needed/empty)

### Preprocessing

+ Convert datetime column to month, day, year, hour, minute
```python
df_initial['Month'] = df_initial.date.str.split('/').str[0]
df_initial['Day'] = df_initial.date.str.split('/').str[1]
df_initial['Year'] = df_initial.date.str.rsplit('/', 1).str[1]
df_initial['Time_char'] = df_initial.datetime.str.split(' ').str[1]
df_initial['Hour'] = df_initial['Time_char'].str.split(':').str[0]
df_initial['Minute'] = df_initial['Time_char'].str.split(':').str[1]
df_initial = df_initial.drop(columns="Time_char")
```
+ Remove rows where park is closed(denounced by -999 posted wait time)
```python
df_initial = df_initial[df_initial['SPOSTMIN'] != -999]
```
+ Merge posted and actual wait time columns since only one is ever present, if actual wait time is present then keep this as it's more important.
```python
for index, row in df_initial.iterrows():
    if math.isnan(row['SPOSTMIN']):
        df_initial.loc[index,'SPOSTMIN'] = df_initial.loc[index, 'SACTMIN']
#     print(row['SPOSTMIN'], row['SACTMIN'])
df_initial = df_initial.drop(columns="SACTMIN")
```
+ Place estimated wait time as a label and remove from input dataframe since it is to be predicted.
```python
df_initial = df_initial.drop(columns=["DAYOFWEEK", "date", "datetime", "SPOSTMIN"])
```
+ Label encode the day of week column.  Used label encoding over One Hot Encoding because there could be a relationship between each day of the week(e.g. Mon->Tues) so removing these relationships with OHE might not be practical.
```python
label_encoder_DOW = LabelEncoder()
DoW_feature = label_encoder_DOW.fit_transform(df_initial.DAYOFWEEK.iloc[:].values)
df_initial['DayOfWeek'] = DoW_feature
```

### Machine Learning/XGboosting

+ Train test split
```python
random_seed = 5
t_s = .20
X_train, X_test, y_train, y_test = train_test_split(df_initial, df_y, test_size = t_s, random_state = random_seed)
```
+ Create parameter grid to tune XGRegression hyperparameters. Values taken based off suggested values from Jason Brownlees XGBoost book where #1 Kaggle modeller suggests.  
+ Use Kfold cross validation to validate.
+ Utilize XGBoost with input dataset to create regression model with gpu boosting.
```python
param_grid = {'n_estimators': [100, 200, 300], #random int btwn 100 and 500 - removed
              'learning_rate': stats.uniform(0.01, 0.08), #.01 + loc, range of .01+/-.08
              'max_depth': [2, 4, 6, 8], #tree depths to check
              'colsample_bytree': stats.uniform(0.3, 0.7) #btwn .1 and 1.0    
}
kfold = KFold(n_splits=3, shuffle=True, random_state=random_seed)
model = XGBRegressor(tree_method='gpu_hist')
```
+ Use Randomized search to test hyperparater grid, optionally could use gridsearchCV but don't have processing resources to test each combination at the time.
+ Select best performing estimator from the randomized search and store model using pickle for deployment on flask server.
```python
rand_search = RandomizedSearchCV(model, param_distributions = param_grid, scoring = 'explained_variance', n_iter = 3, verbose = 10, cv=kfold)
rand_result = rand_search.fit(X_train, y_train)
print("Best: %f using %s" % (rand_result.best_score_, rand_result.best_params_))
best_XGB_estimator = rand_result.best_estimator_
pickle.dump(best_XGB_estimator, open("xgb_dwarves.pkl", 'wb'))
```

### Flask server
+ Basic flask + CORS for initialization.
```python
app = Flask(__name__)
cors = CORS(app, resources={r"/api/*": {"origins": "*"}})
```
+ Retrieve pickled models from directory
```python
models = {'Alien Swirling Saucers':pickle.load(open('xgb_alien_saucers.pkl','rb')),
"Rock 'n' Roller Coaster":pickle.load(open('xgb_rock_n_rollercoaster.pkl','rb')),
"Slinky Dog Dash":pickle.load(open('xgb_slinky_dog.pkl','rb')),
"Toy Story Mania!":pickle.load(open('xgb_toy_story_mania.pkl','rb')),
"Avatar Flight of Passage":pickle.load(open('xgb_flight_of_passage.pkl','rb')),
"DINOSAUR":pickle.load(open('xgb_dinosaur.pkl','rb')),
"Expedition Everest":pickle.load(open('xgb_expedition_everest.pkl','rb')),
"Kilimanjaro Safaris":pickle.load(open('xgb_kilimanjaro_safaris.pkl','rb')),
"Na'vi River Journey":pickle.load(open('xgb_navi_river.pkl','rb')),
"Pirates of the Caribbean":pickle.load(open('xgb_pirates_of_caribbean.pkl','rb')),
"Seven Dwarfs Mine Train":pickle.load(open('xgb_dwarves.pkl','rb')),
"Spaceship Earth":pickle.load(open('xgb_spaceship_earth.pkl','rb')),
"Splash Mountain ":pickle.load(open('xgb_splash_mountain.pkl','rb')),
"Soarin":pickle.load(open('xgb_soarin.pkl','rb'))
}
```
+ Select model from models dictionary based on user selected model.
```python
model = models[request_json['ride']]
```
+ Get user input data (month/day/year) from selected calendar date, each hour of day for inputting to model.
```python
month= int(request_json['month'])
day= int(request_json['day'])
year= int(request_json['year'])
hours = [8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,0]
minute= 0
dayofweek= int(request_json['dayofweek'])
```
+ For each hour of the selected date predict the wait times and store in a list for sending back to web app.
```python
for hour in hours:
    data = [[month, day, year, hour, minute, dayofweek]]
    input_df = pd.DataFrame(data, columns =['Month','Day','Year','Hour','Minute','DayOfWeek'])
    prediction = model.predict(input_df)
wait_times[hour] = int(prediction[0])
```
+ Send predictions back to user for display.
```python
return jsonify([
      {
        'name': '8am',
        'value': wait_times[8]
      },
      {
        'name': '9am',
        'value': wait_times[9]
      },
      {
        'name': '10am',
        'value': wait_times[10]
      },
      {
        'name': '11am',
        'value': wait_times[11]
      },
      {
        'name': '12pm',
        'value': wait_times[12]
      },
      {
        'name': '1pm',
        'value': wait_times[13]
      },
      {
        'name': '2pm',
        'value': wait_times[14]
      },
      {
        'name': '3pm',
        'value': wait_times[15]
      },
      {
        'name': '4pm',
        'value': wait_times[16]
      },
      {
        'name': '5pm',
        'value': wait_times[17]
      },
      {
        'name': '6pm',
        'value': wait_times[18]
      },
      {
        'name': '7pm',
        'value': wait_times[19]
      },
      {
        'name': '8pm',
        'value': wait_times[20]
      },
      {
        'name': '9pm',
        'value': wait_times[21]
      },
      {
        'name': '10pm',
        'value': wait_times[22]
      },
      {
        'name': '11pm',
        'value': wait_times[23]
      },
      {
        'name': '12am',
        'value': wait_times[0]
      }
])
```
