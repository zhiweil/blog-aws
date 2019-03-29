---
title: Python machine learning
tags: [python]
keywords: python
last_updated: March 29, 2019
summary: "Python machine learning libs and tools"
sidebar: mydoc_sidebar
permalink: python-machine-learning.html
folder: python
---
## Basics
Steps of machine learning:
1. Import data - usually in CSV format
2. Clean data - remove or fix duplicate, incomplete or wrong data
3. Split data - into training/test sets
4. Create a model - choose appropriate algorithm 
5. Train model - input training set into model
6. Make predictions
7. Evaluate and Improve

## Libraries
* Numpy - provides a multi-dimensional array
* Pandas - data analysis library. It is based on data frame (like spreadsheet)
* MatPlotLib - 2D plotting library for plotting graphs
* Scikit-Learn - machine learning library which implements various algorithms

## Tools
* [jupyter](https://jupyter.org) - is an environment for writting python machine learning code.
* [Anaconda](https://www.anaconda.com/) - popular machine learning platform for python/R
* From Anaconda's download page, download the installer for python 3, install it. This installs Jupyter as well as machine learning libraries.
* run the command as follows to launch jupyter.
    ```bash
    jupyter notebook
    ```
* Get some training/testing data from website [Kaggle](https://www.kaggle.com/).

## Jupyter
* ESC key is used to enter CMD mode; ENTER key to enter EDIT mode.
* In CMD mode, press key H to get shortcut keys information.
* In CMD mode, press key A to insert cell above; or B to insert cell below.
* In CMD mocde, press key D twice to delete a cell.
* By default, jupyter runs a specific cell.
* Intellisense - input an object plus dot (.), then you press TAB to get intellisense.
* Put cursor on a property or method, press SHIFT + TAB, this gives you its signature and description.
* Click the "Run" button on UI will add a new cell for each run; CTRL + ENTER will not add new cell.

## Music Prediction Example
This example show the basic steps for a machine learning project.

```python
import pandas as pd
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
from sklearn.externals import joblib
from sklearn import tree

# import CSV data
music_data = pd.read_csv("vgsales.csv")

# clean data - convert Platform to numbers
data_all = music_data.drop(columns = ["Rank", "Name", "Publisher", "NA_Sales", "EU_Sales", "JP_Sales", "Other_Sales", "Global_Sales"])
data_all = data_all.dropna(axis = "rows")
platforms = data_all['Platform']
platform_list = list(dict.fromkeys(platforms))
platform_dict = {}
for index in range(len(platform_list)):
    platform_dict[platform_list[index]] = index
platform_num = []
for pf in platforms:
    platform_num.append(platform_dict[pf])
data_all['Platform'] = platform_num

data_all.describe()

# get training/testing data sets
data_input = data_all.drop(columns = ['Genre'])
data_output = data_all['Genre']

# split data sets into training and testing sets
x_train, x_test, y_train, y_test = train_test_split(data_input, data_output, test_size = 0.2)

# create model
model = DecisionTreeClassifier()

# train it
model.fit(x_train, y_train)

# dump the model to a file
joblib.dump(model, "music-predictor.joblib")
# the trained model can be loaded by
# model = joblib.load("music-predictor.joblib")

# make preditions
predictions = model.predict(x_test)

score = accuracy_score(y_test, predictions)

# export model to a visual format
tree.export_graphviz(model, out_file = 'music-predictor.dot',
                    feature_names=['Platform', 'Year'],
                    class_names = sorted(data_output.unique()),
                    label = 'all',
                    rounded = True,
                    filled = True)
```