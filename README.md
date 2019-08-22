# Modelling of road network fuel consumption effects

The code files can be roughly divided into two parts, one for data processing and one for the training of machine learning models. Before 



## Data processing
The dataset contains data from various sources include FMS database, vehicle configuration database from Scania datalake and weather data from [SMHI](https://www.smhi.se). which will be treated seperatedly. 
To get the dataset prepared for machine learning works, run the following files sequentially.

### FMS Data

1. Download the raw data from Scania datalake in csv format.
2. Run **data_split.py** to split the data into various seperate files on vehicle ids. Two parameters are required: --input_dir [The location of the raw data file]; --output_dir [The target directory of the output files]

### Vehicle Configuration Data

### Weather Data

### Get Estimated Vehicle Weights


## Training machine learning models

### Data Analysis

### Support Vector Machine

### Random Forest

### Gradient Boosted Machine

