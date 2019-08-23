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

### FMS Data

1. Download the raw data from Scania datalake in **csv** format. The raw data should be GPS points, each point can have as many features as you need, but some may be omitted in the later steps.

2. Run **data_split.py** to split the data into various seperate files on vehicle ids. Two parameters are required: **--input_dir** [The location of the raw data file]; **--output_dir** [The target directory of the output files].

3. Run **format_csv_batch.py** to format the data files we get in the last step into **Json** format, which is required by Barefoot. Two parameters are wanted: **--input_dir** [The path to the directory of csv files]; **--output_dir** [The path to the directory of json files].

4. Start the Barefoot server and run **bash_batch.py** to get the data files through the map-matching server in batch fashion. Configure the input and output directory in **params**. The returned data (route data) from map-matching server is in **Json** format.

5. Run **fuel_on_road_segment.py** to reorganise the content in the Json files we get from the last step. This includes filtering out some unqualified data and calculate the fuel consumption on each road segment. Two parameters are required: **--input_dir** [The location of the route data files]; **--output_dir** [The target directory of the output files]. 

6. Run **mongo_import.py** to import the route data into MongoDB. The dafault database and collection are called **results** and **sweden_small** separately. One parameter is required: **--input_dir** [The location of the route data files]. Another database called **vid** will also be created in this course, this is for recording the ids of vehicles we have in the database.

7. Run **normalization.py** to calculate the fuel consumption indices based on the route data we imported before.

### Vehicle Configuration Data

1. Download the vehicle configuration data from Scania datalake in **Json** format. 

2. Run ****

### Weather Data

1. Run **weather_data.py** to use the RESTful api to download weather data from SMHI. This script will also get descriptive information of weather parameters and weather stations. All the downloaded data will be pushed into MongoDB. Weather parameters and weather stations information will be put in **weather_desc** database and collections called **parameters** and **stations** respectively. The actual weather data is in **weather_data** database and each station has a separate collection named by the station id to store its own data.

2. The weather data from the same station is stored in different files in terms of the parameters. Run **weather_data_importer** to integrate these data together into a single file and push them into MongoDB. The actual weather data is in **weather_data** database and each station has a separate collection named by the station id to store its own data.

### Road Data

1. Download the map database from OpenStreetMap, import it into the PostgreSQL (refer to the instructions of Barefoot).

2. Start the PostgreSQL service in docker (refer to the instructions of Barefoot).

3. Run **roads_importer.py** to import road segments from PostgreSQL to MongoDB. The road ids will remain the same as they are in Barefoot. Configure the parameters in **params**.

3. Run **reset_fci.py** if you want to reset the fuel consumption indices in MongoDB for all the road segments.

### Get Estimated Vehicle Weights

### Extract and Integrate the Data

Run **mongo_extract.py** to match the data we mentioned above together into a single file for the convienience of machine learning works. 

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

### Visualization of Fuel Consumption Index
Visualizing the fuel consumption indices of all the road segments could be an extremely heavy workload for the map server. Thus I implemented a script that allows to 


