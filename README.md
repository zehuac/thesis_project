# Modelling of road network fuel consumption effects

The scripts in this repository can be roughly divided into two parts, one for data processing and one for the training of machine learning models. Before get started, we need to have some environments set up.

## Setting up the environments

### Barefoot

Barefoot will serve as the map-matching server in this project, refer to the [documentation](https://github.com/bmwcarit/barefoot) and follow the instructions to get it working on your computer.

### MongoDB

MongoDB will be used for data storage and management here. Follow the instructions on the offical website to set up. I also highly recommend to install [MongoDB Compass](https://www.mongodb.com/download-center/compass) which is a powerful tool to visualize the database structures and manipulate data.

### Languages
**Python 3.7** is used for data processing and building machine learning models in this project. 
If you want to modify the source code of Barefoot, **Java 1.8** is also required.

### Packages
The following packages need to be installed in Python:
pandas, sklearn, xgboost, pymongo, numpy, matplotlib, psycopg2, seaborn, scipy


## Data processing
The dataset contains data from various sources include FMS database, vehicle configuration database from Scania datalake and weather data from [SMHI](https://www.smhi.se). which will be treated seperatedly. 
To get the dataset prepared for machine learning works, run the following files sequentially.

### FMS Data

1. Download the raw data from Scania datalake in **csv** format. The raw data should be GPS points, each point can have as many features as you need, but some may be omitted in the later steps.
2. Run **data_split.py** to split the data into various seperate files on vehicle ids. Two parameters are required: **--input_dir** [The location of the raw data file]; **--output_dir** [The target directory of the output files].
3. Run **format_csv_batch.py** to format the data files we get in the last step into **Json** format, which is required by Barefoot. Two parameters are wanted: **--input_dir** [The path to the directory of csv files]; **--output_dir** [The path to the directory of json files].
4. Run ****

### Vehicle Configuration Data

### Weather Data

### Get Estimated Vehicle Weights


## Training machine learning models

### Data Analysis

Run **ml/data_analysis.py** to perform some basic analysis of the data like plotting the distributions of some features and correlation matrix and so on. 

### Support Vector Regreesion

Run **ml/ml_svr.py** to run support vector regression algorithm or perform grid search and cross validation based on it. The grid search results will be output to **ml/data/**.

### Random Forest

Run **ml/ml_rf.py** to run random forest algorithm or perform grid search and cross validation based on it. The grid search results will be output to **ml/data/**.

### Gradient Boosted Machine

Run **ml/ml_gbm.py** to run gradient boosted machine algorithm or perform grid search and cross validation based on it. The grid search results will be output to **ml/data/**.

## Others

### The Configuration File
**params** is an important file which contains default parameters for many scripts. 

### 

