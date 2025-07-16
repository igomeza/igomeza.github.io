# Exploratory Analysis of Air Quality in New York City (1973)

This project presents a detailed Exploratory Data Analysis (EDA) of the R airquality dataset, which contains daily measurements of air quality in New York City between May and September 1973. The main objective is to understand the distribution, seasonal trends and relationships between key variables such as ozone, solar radiation, wind speed and temperature.

🎯 Project objectives
- Analyze the temporal distribution of the main air quality variables (ozone, solar radiation, wind, and temperature) over the study period.
- Identify seasonal or monthly trends in ozone levels and other variables.
- Evaluate the relationship and possible significant correlations between ozone concentration and environmental variables such as temperature, wind speed, and solar radiation.

🗃️ Dataset:

"airquality"

The airquality dataset is a built-in dataset in R that records daily air quality measurements in New York. It includes the following key variables:

- Ozone: Average ozone concentration in parts per billion (ppb) between 13:00 and 15:00 at Roosevelt Island.
- Solar.R (Solar Radiation): Solar radiation in Langleys (frequency band 4000-7700 Angstroms) between 08:00 and 12:00 hours at Central Park.
- Wind: Average wind speed in miles per hour (mph) at 07:00 and 10:00 hours at LaGuardia Airport.
- Temp: Maximum daily temperature in degrees Fahrenheit (°F) at LaGuardia Airport.
- Month: Month of observation (5 = May to 9 = September).
- Day: Day of the month of observation (1-31).

Ozone data were obtained from the New York State Department of Conservation, and meteorological data were obtained from the National Weather Service.

🛠️ Tools and libraries:

This project has been developed using RStudio and the following main libraries:

- dplyr: For data manipulation and transformation.
- tidyr: For data cleaning and restructuring.
- ggplot2: For the creation of high-quality visualizations.

🚀 Project structure and analysis:
The analysis has been structured in the following sections:

1. Introduction: Context, objectives, and description of the dataset.
   
2. Data Preparation and Exploration:
 	- Loading and Initial Inspection: Loading of libraries and the dataset, first inspections with dim(), head(), and summary().
	- Data Cleaning: Duplicate verification and missing values (NAs) management strategy. We opted for the elimination of complete rows with NAs (na.omit()) to ensure reliability in the correlation analysis, resulting in a clean dataframe (data_air_clean) with 111 observations.
	- Descriptive Statistics: Calculation of the mean, median, standard deviation, and number of observations (N) for the key variables, presented in a table.

3. Exploratory Data Analysis (EDA):
	- Ozone vs. Temperature Relationship: scatter plot showing a positive correlation; the higher the temperature, the higher the ozone tends to increase. 
	- Wind: Scatter plot showing a negative correlation; the higher the wind speed, the lower the ozone tends to decrease due to the dispersion of pollutants.
	- Solar Radiation: Scatter plot indicating a positive relationship; more solar radiation is associated with higher ozone concentrations, a key factor in ozone formation.
	- Monthly Ozone Trend: Line graph showing a significant spike in average ozone levels during July and August.
	- Monthly Ozone Distribution: Boxplots detailing the distribution of ozone by month, confirming higher levels and greater variability in summer, and the presence of outliers.
 
 📈 Conclusions and Key Findings:
- The air quality variables exhibit different behaviors, with ozone showing a skewed distribution and a notable amount of missing values.
- There is a clear seasonal trend for ozone, reaching its highest levels during the summer months (July and August), which is aligned with conditions of higher temperature and solar radiation.
- Ozone shows a positive correlation with temperature and solar radiation, and a negative correlation with wind speed.
- Despite the observed correlations, the variability in the data suggests that multiple factors influence ozone concentration, and these plots provide a solid basis for future predictive analyses.

📄 References:
- airquality dataset, RStudio: https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/airquality.html 
- Chambers, J. M., Cleveland, W. S., Kleiner, B., and Tukey, P. A. (1983). Graphical Methods for Data Analysis. Belmont, CA: Wadsworth.


Developed by: Gómez-Alonso, I.S.
Date: June 13, 2025 
