# Exploratory Analysis of Air Quality in New York City (1973)

This project presents a detailed Exploratory Data Analysis (EDA) of the R airquality dataset, which contains daily measurements of air quality in New York City between May and September 1973. The main objective is to understand the distribution, seasonal trends and relationships between key variables such as ozone, solar radiation, wind speed and temperature.


🎯 Project objectives
- Analyze the temporal distribution of the main air quality variables (ozone, solar radiation, wind, and temperature) over the study period.
- Identify seasonal or monthly trends in ozone levels and other variables.
- Evaluate the relationship and possible significant correlations between ozone concentration and environmental variables such as temperature, wind speed, and solar radiation.


🗃️ Dataset:
The `airquality` dataset is a built-in dataset in R that records daily air quality measurements in New York. It includes the following key variables:

- Ozone: Average ozone concentration in parts per billion (ppb) between 13:00 and 15:00 at Roosevelt Island.
- Solar.R (Solar Radiation): Solar radiation in Langleys (frequency band 4000-7700 Angstroms) between 08:00 and 12:00 hours at Central Park.
- Wind: Average wind speed in miles per hour (mph) at 07:00 and 10:00 hours at LaGuardia Airport.
- Temp: Maximum daily temperature in degrees Fahrenheit (°F) at LaGuardia Airport.
- Month: Month of observation (5 = May to 9 = September).
- Day: Day of the month of observation (1-31).

Ozone data were obtained from the New York State Department of Conservation, and meteorological data were obtained from the National Weather Service.


**Loading and initial inspection**

```r
install.packages("dplyr") #for data manipulation and transformation
install.packages("tidyr") #for data cleaning and restructuring
install.packages("ggplot2") #for the creation of high-quality visualizations
library(dplyr)
library(tidyr)
library(ggplot2)

data_air <- airquality #assign the dataset in a variable

dim(data_air) #dataset size

head(airquality) #first rows in data
```
<img width="887" height="141" alt="head" src="https://github.com/user-attachments/assets/fac3627e-82b5-4cfd-8b8c-efdd241fc1ef" />

```r
summary(data_air) #summary of variables in data
```
<img width="593" height="113" alt="summary" src="https://github.com/user-attachments/assets/ae2188b3-f70c-4d3c-8a2a-3ec079f942f9" />

Key findings by variable:
 - Ozone (Ozone Concentration): 
      - Ozone concentration shows a considerable range, from a low of 1 ppb to a high of 168 ppb.
      - The mean (42.13 ppb) is notably higher than the median (31.50 ppb), suggesting a positive asymmetric distribution (skewed to the right), indicating the presence of some exceptionally high ozone values.
      - A critical aspect is the presence of 37 missing values (NA's), which represent a significant portion of the observations for this variable and will require careful consideration during the cleaning phase. 
  
 - Solar.R (Solar Radiation): 
     - Solar radiation fluctuates from 7 Langleys to 334 Langleys.
     - The mean (185.9 Langleys) and median (205.0 Langleys) are relatively close, suggesting a more symmetrical distribution than ozone, although the median is slightly higher, indicating a slight asymmetry towards lower values.
     - Seven missing values (NA's) were identified in this variable, a smaller number than in Ozone, but still in need of attention.
  
 - Wind (Wind Speed):
    - Wind speed ranges from 1.7 mph to 20.7 mph.
    - The mean (9.958 mph) and median (9.700 mph) are very similar, indicating a fairly symmetrical distribution for wind speed, with most values concentrated around the mean.
    - No missing values were observed for this variable, which simplifies its handling.
  
 - Temp (Temperature):
    - The reported temperature ranges from 56°F to 97°F.
    - The mean (77.88°F) and median (79.00°F) are very close, suggesting a relatively symmetrical temperature distribution.
    - The quartiles indicate that most of the temperatures are between 72°F and 85°F.
    - There are no missing values for the temperature variable.

 - Month and Day:
The variables *Month* and *Day* are of discrete type and act as temporal identifiers.
Month spans from month 5 (May) to month 9 (September), confirming the study period.
Day ranges from day 1 to day 31, as would be expected for a daily record.
They have no missing values, and their descriptive statistics reflect their nature as time indices.


**Data Cleaning**: 
Duplicate verification and missing values (NAs) management strategy. We opted for the elimination of complete rows with NAs (na.omit()) to ensure reliability in the correlation analysis, resulting in a clean dataframe (data_air_clean) with 111 observations.

```r
num_duplicated <- sum(duplicated(data_air)) #verify duplicated rows
print(num_duplicated)
```
This dataset has no rows with duplicate data.


```r
colSums(is.na(data_air)) #Counting NA's by column
```
Removal of entire rows (na.omit()): 
```r
data_air_clean <- na.omit(data_air) # Remove all rows containing at least one NA

dim(data_air_clean) # Check dimensions of new data frame
```
A new dataframe called *data_air_clean* was created. Out of 153 original rows, we are left with 111 rows. That's a loss of 42 rows (37 from Ozone + 5 additional rows where Solar.R had NA and Ozone did not).

```r
colSums(is.na(data_air_clean)) # Check again for presence of NA's to confirm cleanup
summary(data_air_clean) # Verify again the summary 
```
<img width="577" height="123" alt="summary_2" src="https://github.com/user-attachments/assets/02a54687-719d-4dc2-bef6-694e2bfc8837" />

**Descriptive Statistics**

```r
numeric_cols_clean <- c("Ozone", "Solar.R", "Wind", "Temp") # Define the numerical columns of interest
```

```r
descriptive_stats_clean <- data_air_clean %>% # Calculate the descriptive statistics for each column of the clean dataframe.
  select(all_of(numeric_cols_clean)) %>% # Select only the numerical columns that interest us
  summarise( # Summarize each column
    # Ozone
    Ozone_Mean = mean(Ozone, na.rm = TRUE),
    Ozone_Median = median(Ozone, na.rm = TRUE),
    Ozone_SD = sd(Ozone, na.rm = TRUE),
    Ozone_N = n(),
    
    # Solar radiation
    SolarR_Mean = mean(Solar.R, na.rm = TRUE),
    SolarR_Median = median(Solar.R, na.rm = TRUE),
    SolarR_SD = sd(Solar.R, na.rm = TRUE),
    SolarR_N = n(),

    # Wind
    Wind_Mean = mean(Wind, na.rm = TRUE),
    Wind_Median = median(Wind, na.rm = TRUE),
    Wind_SD = sd(Wind, na.rm = TRUE),
    Wind_N = n(),

    # Temperature
    Temp_Mean = mean(Temp, na.rm = TRUE),
    Temp_Median = median(Temp, na.rm = TRUE),
    Temp_SD = sd(Temp, na.rm = TRUE),
    Temp_N = n()
  ) %>%
  # Use pivot_longer to transform the table from width to length
  pivot_longer(
    cols = everything(), # Select all columns
    names_to = c("Variable", ".value"), # Split the names in ‘Variable’ and the type of statistic
    names_pattern = "(.+)_(Mean|Median|SD|N)" # regex pattern to extract the variable and the statistic
  )

print(descriptive_stats_clean) # Show the resulting table
```
<img width="913" height="108" alt="statistics" src="https://github.com/user-attachments/assets/c5dac991-f211-498b-a64e-88e4f0a4f920" />


**Exploratory Data Analysis (EDA)**

*Ozone vs. temperature relationship*
scatter plot showing a positive correlation; the higher the temperature, the higher the ozone tends to increase. 

```{r}
ggplot(data_air_clean, aes(x = Temp, y = Ozone)) +
  geom_point(alpha = 0.6, color = "darkblue") + 
  geom_smooth(method = "lm", se = FALSE, color = "red") + # Add a linear regression line
  labs(title = "Ozone vs. Temperature",
       x = "Temperature (°F)",
       y = "Ozone (ppb)") +
  theme_minimal()
```
<img width="435" height="269" alt="1 ozone_vs_temperature" src="https://github.com/user-attachments/assets/5cecd3fe-6aa3-4023-b375-794c5fefe205" />

*Ozone vs. wind relationship*
Scatter plot showing a negative correlation; the higher the wind speed, the lower the ozone tends to decrease due to the dispersion of pollutants.

```{r}
ggplot(data_air_clean, aes(x = Wind, y = Ozone)) +
  geom_point(alpha = 0.6, color = "darkgreen") +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(title = "Ozone vs. wind speed",
       x = "wind speed (mph)",
       y = "Ozone (ppb)") +
  theme_minimal()
```

<img width="435" height="269" alt="2 ozone_vs_wind-speed" src="https://github.com/user-attachments/assets/c5fe6179-1334-464b-b342-3f3074616330" />

*Ozone vs. Solar radiation*
Scatter plot indicating a positive relationship; more solar radiation is associated with higher ozone concentrations, a key factor in ozone formation.

```{r}
ggplot(data_air_clean, aes(x = Solar.R, y = Ozone)) +
  geom_point(alpha = 0.6, color = "orange") +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(title = "Ozone vs. Solar radiation",
       x = "Solar radiation (langleys)",
       y = "Ozone (ppb)") +
  theme_minimal()
```

<img width="435" height="269" alt="3 ozone_vs_solar-radiation" src="https://github.com/user-attachments/assets/2eabe35a-8352-4cd6-b06e-8b317068b458" />

*Average ozone per month*
Line graph showing a significant spike in average ozone levels during July and August.

```{r}
data_air_clean %>%
  group_by(Month) %>%
  summarise(Avg_Ozone = mean(Ozone, na.rm = TRUE)) %>%
  ggplot(aes(x = Month, y = Avg_Ozone)) +
  geom_line(color = "purple", size = 1.2) +
  geom_point(color = "purple", size = 3) +
  labs(title = "Ozone average per month",
       x = "Month",
       y = "Ozone average (ppb)") +
  scale_x_continuous(breaks = 5:9, labels = c("May", "June", "July", "August", "September")) +
  theme_minimal()
```	
<img width="435" height="269" alt="4 ozone_average_per_month" src="https://github.com/user-attachments/assets/12a02495-e322-471c-9619-b136cc5f6d55" />

*Monthly Ozone Distribution*
Boxplots detailing the distribution of ozone by month, confirming higher levels and greater variability in summer, and the presence of outliers.

```{r}
ggplot(data_air_clean, aes(x = factor(Month), y = Ozone, fill = factor(Month))) +
  geom_boxplot(na.rm = TRUE) +
  labs(title = "Ozone distribution by month",
       x = "Month",
       y = "Ozone (ppb)") +  
  scale_x_discrete(labels = c("May", "June", "July", "August", "September")) +
  theme_minimal() +
  guides(fill = "none")
```

<img width="435" height="269" alt="5 ozone_distribution_by_month" src="https://github.com/user-attachments/assets/aad3574c-6d65-4e25-9af9-f48d665dce28" />

 
 📈 Conclusions and Key Findings:
- The air quality variables exhibit different behaviors, with ozone showing a skewed distribution and a notable amount of missing values.
- There is a clear seasonal trend for ozone, reaching its highest levels during the summer months (July and August), which is aligned with conditions of higher temperature and solar radiation.
- Ozone shows a positive correlation with temperature and solar radiation, and a negative correlation with wind speed.
- Despite the observed correlations, the variability in the data suggests that multiple factors influence ozone concentration, and these plots provide a solid basis for future predictive analyses.


📄 References:
- airquality dataset, RStudio: https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/airquality.html 
- Chambers, J. M., Cleveland, W. S., Kleiner, B., and Tukey, P. A. (1983). Graphical Methods for Data Analysis. Belmont, CA: Wadsworth.

__________________________

Developed by: Gómez-Alonso, I.S.
Date: June 13, 2025 
