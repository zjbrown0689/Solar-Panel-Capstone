[Solar Panel Capstone](https://github.com/zjbrown0689/Solar-Panel-Capstone/blob/main/reports/solar_panel_capstone_final_report.pdf)
==============================
Solar panel installations are one of the most effective ways that an individual can lower their carbon footprint, however, they often cost over $10,000 and are unaffordable for many. My goal in this project was to use solar panel installation data from the Lawrence Berkeley National Laboratory’s (LBNL) Tracking the Sun program to identify what aspects of a solar panel installation can be controlled to maximize cost efficiency for a person living in Austin Texas. After compiling the data, expanding the features, and comparing a wide range of regression models, an XGBoost regression model was developed that identified the following guidance:
* Design the solar panel installation with a relatively high inverter loading ratio
* Identify and secure any rebate or grant available
* Consider buying a solar panel model with lower conversion efficiency
* Purchase the largest configuration of solar panels possible for the house, sizing the inverter quantity to maintain that high inverter loading ratio
* Consider scheduling the installation for July or December


## 1. [Data](https://github.com/zjbrown0689/Solar-Panel-Capstone/blob/main/notebooks/data_wrangling.ipynb)
The data used in this project was provided by the LBNL through their [Tracking the Sun](https://emp.lbl.gov/tracking-the-sun) program. The data for 2020 and 2021 was downloaded as [parquet files](https://data.openei.org/s3_viewer?bucket=oedi-data-lake&prefix=tracking-the-sun%2F2021%2F) and compiled into a single dataframe broken down by state. The data was then restricted to only residential installations, leaving ~200,000 entries with ~80 features to work with.

## 2. Method
The target metric for this project is cost efficiency, which was calculated as (total_installation_price - rebates_or_grants) / system_size_dc. This feature was modeled using a wide range of regression algorithms, of which XGBoost regressor had the best root mean squared error. The model was trained on 75% of the dataset and the most important features for the model were used for the final customer guidance.

## 3. [Feature Engineering](https://github.com/zjbrown0689/Solar-Panel-Capstone/blob/main/notebooks/preprocessing.ipynb)
Features missing more than 30% of their entries were dropped from the data, and then categorical features were dummied, creating a new feature for any unique value with 30 or more occurances. The data was train-test split, missing values were imputed with the most common value for the feature, the data was scaled, and then the dataset was reduced to the 400 most important features as determined by f-regression.

## 4. [Exploratory Data Analysis](https://github.com/zjbrown0689/Solar-Panel-Capstone/blob/main/notebooks/exploratory_data_analysis.ipynb)
Exploratory data analysis (EDA) found significant correlations between many features and installation cost efficiency. Features such as inverter loading ratio, system size, and inverter quantity had negative correlations with price per KW. Module efficiency had a positive correlation with price per KW, and other features such as installation month, tilt, and azimuth showed more complicated correlations.

## 5. [Model Results](https://github.com/zjbrown0689/Solar-Panel-Capstone/blob/main/notebooks/model_development.ipynb)
After hyperparameter tuning an initial 10 models on 10% of the data, the four best were tuned a second time on 80% of the data with XGBoost regressor standing out as the best. ![Final comparison of 4 models](https://github.com/zjbrown0689/Solar-Panel-Capstone/blob/main/figures/final_comparison.png)
When trained on the full training data and tested against the 25% test set the RMSE jumped from 3 to 35 million RMSE, which was caused by three outlier points that likely had typos increasing their total installed prices from tens of thousands to millions. When those three points were removed the resulting RMSE dropped to 2.40 million. The 15 most important features of this model were then compared with EDA results to identify how they impact the installation cost efficiency.

## 6. Future Work
In the future I would like to take this work a step further and develop a recommendation tool to help make solar panel installations even more accessible for homeowners. Right now there are [some tools](https://pvwatts.nrel.gov/) available to help potential customers predict the output they could get from a solar panel array. However, I’d like to take it even further by taking the customers budget, average monthly electrical use, location, and electrical provider to recommend the size of their solar panel array, the number of inverters they should install, any rebates or grants they are eligible for, and provide the length of time it would take for the solar panels to pay for themselves. There are a lot of factors to consider when deciding whether to install solar panels or not, and I think a tool like this would make that decision much less daunting.

## 7. Credits
I would like to thank the Springboard community for assistance throughout this project helping me quickly overcome numerous hurdles. I'd also like to thank the LBNL for collecting and hosting this data for public use. Climate change is a real threat to all of us, and I feel a true calling to do what I can to help reduce carbon emissions, and having data like this available for public use is invaluable. I'd also like to thank Upom Malik for being a phenomenal mentor throughout this Springboard bootcamp and especially throughout this capstone. His guidance helped steer me through this project and really helped me develop the project and work through some major issues I encountered. 

--------

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>
