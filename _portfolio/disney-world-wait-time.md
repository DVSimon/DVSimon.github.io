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

Python, Angular, flask, HTML, CSS, XGBoost, Pandas, sklearn, Google Maps API


### Overview

+ This project aims to predict wait times of various Disney World rides/amusements on future dates that can be selected by the user.  
+ The application predicts rides for the entire selected date and displays a graph of wait times throughout the user inputted day.
+ Ride to be predicted can be selected on the map (Google Maps API) or on a list of rides that can be filtered by park, recently viewed, favorited or name.

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
