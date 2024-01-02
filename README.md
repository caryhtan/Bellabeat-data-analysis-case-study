# How Can a Wellness Technology Company Play It Smart? 

## A Google Data Analytics Professional Certificate Capstone Project by Cary Htan

<div align="center">
  <img width="500" height="300" src="images/lifestyle.jpg">
</div>

[Bellabeat]( https://bellabeat.com/) is a company that develops fitness products for women. Their products include smart water bottles, fashionable fitness watches and jewelry, and yoga mats. Users can access their health data collected through these devices in the Bellabeat app.

Bellabeat’s co-founders would like to analyze data from non-Bellabeat fitness devices to see how consumers are using these products. The company hopes to use these insights to help guide new marketing strategies for the company. 

## Ask

### Key stakeholders

1. Urška Sršen: Bellabeat’s cofounder and Chief Creative Officer

2. Sando Mur: Mathematician and Bellabeat’s cofounder

3. The Bellabeat marketing analytics team: a team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy.

### Bellabeat products

* Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and make healthy decisions. The Bellabeat app connects to their line of smart wellness products.

* Leaf: Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects to the Bellabeat app to track activity, sleep, and stress.

* Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your daily wellness.

* Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels.

* Bellabeat membership: Bellabeat also offers a subscription-based membership program for users. Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness based on their lifestyle and goals

### Business task

Analyze non-Bellabeat smart device data and compare with one Bellabeat product to discover insights to help guide marketing strategies for the company.

### Key Questions

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

## Prepare 

### Data source: 

FitBit Fitness Tracker Data, hosted on [Kaggle]( https://www.kaggle.com/datasets/arashnic/fitbit), consists of 18 CSV files documenting smart health data from the fitness trackers of thirty users. Data acquisition was carried out through a survey on Amazon Mechanical Turk, capturing minute-level details on physical activity, heart rate, and sleep monitoring from March 12, 2016, to May 12, 2016. As of January 2024, this dataset hasn’t been refreshed in three years and includes daily records of activity, steps, and heart rate.

### Limitations: 

* The dataset includes data from only 30 individuals, which is a small sample size that might not accurately represent the larger population.

* The information is eight years old, and the FitBit devices have likely been updated to produce more accurate results.

* The survey-based collection method can introduce inaccuracies, as participants may not always provide honest or precise answers.

* Weight data is only available for eight users and contains many blank entries, with around two-thirds of the available weight data being entered manually, which could affect its reliability.

### Additional data: 

Given the constraints of the FitBit data, supplementing the analysis with an additional data source is recommended. The Mi Band fitness tracker data, available from [Kaggle](https://www.kaggle.com/datasets/damirgadylyaev/more-than-4-years-of-steps-and-sleep-data-mi-band), offers a longitudinal dataset covering steps and sleep monitoring from April 2016 to July 2022 for an individual user of the Xiaomi Mi Band. This dataset comprises two CSV files, separately detailing step counts and sleep patterns. Including this data allows for an analysis of long-term user engagement with fitness tracking on an individual level. However, it's noted by the uploader that approximately two weeks of step data were corrupted and have been set to zero.

For detailed insights into the variables and structure of the data, refer to the [Data Dictionary and Documentation](Data%20Documentation%20and%20Data%20Dictionary.md) file.

# Process

## Data files selection
The `dailyActivity_merged.csv` file is good for looking at steps and calories burned. The `sleepDay_merged.csv` file is helpful for understanding sleep. And since fitness devices are often used to track weight, the `weightLogInfo_merged.csv` file with weight information is also useful. These files together can give a good picture of health and how people use their fitness devices.

## Softwares selection
Excel will be used for early review of data, R for transformation and exploration of the data, and Tableau for visualization of the data.

## Steps for Early Review
1) Use filters to check for and remove any empty entries in the data.
2) Change the Id field to text since you don't need to do math with it.
3) Change ActivityDate from Datetime to just Date because only dates are listed.
4) Look at the `dailyActivity_merged.csv` file and note where TotalSteps is zero and SedentaryMinutes is 1440. The calories burned changes per user, likely due to differences in weight and height. Sometimes sedentary minutes are 1440 but no calories are burned.
5) Note in the `weightLogInfo_merged` file that there are only two entries for the Fat field, so it won't be much use for insights.

## Transformation and Exploration
To see R codes: [Click here](BellaBeat_RScript.R)

1) Begin by loading the tidyverse package and the data files.
2) Verify that the data has loaded as expected.
3) Change the Id field from numbers to text.
4) Update the names of ActivityDate, SleepDay, and Date to the date format.

```markdown
activity <- activity %>%
  mutate_at(vars(Id), as.character) %>%
  mutate_at(vars(ActivityDate), as.Date, format = "%m/%d/%y") %>%
  rename("Day"="ActivityDate") 
```

5) Link the data sets together using a mix of joining methods.
6) Include a new variable to indicate the day of the week.

```markdown
combined_data <- sleep %>%
  right_join(activity, by=c("Id","Day")) %>%
  left_join(weight, by=c("Id", "Day")) %>%
  mutate(Weekday = weekdays(as.Date(Day, "m/%d/%Y")))
```

7) Clean the data by eliminating duplicate rows and count the number of missing and unique Id entries.

```markdown
combined_data <- combined_data[!duplicated(combined_data), ]
sum(is.na(combined_data))
n_distinct(combined_data$Id)
n_distinct(sleep$Id)
n_distinct(weight$Id)
```

The final dataset consists of 940 variables with 25 variables. In total, there are 33 unique Id entries. The counts of distinct users in dailyActivity, sleepDay, and weightLogInfo are 33, 24, and 8, respectively. The combined data contains 6893 missing values, which is expected given the limited weight data from eight users and incomplete sleep information logging from some users.

# Analyze 

## Select summary statistics and visualizations 

```
combined_data %>%
select(TotalMinutesAsleep, TotalSteps, TotalDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories, WeightKg, Fat, BMI, IsManualReport) %>%
summary()
```


![Summary Statistics](https://github.com/CoolBeansProgramming/Bellabeat-Case-Study/blob/main/Summary%20Statistics.png?raw=true "Summary Statistics")

The average user weighs 72.04 kg, has a BMI of 25.19, and spent the most time doing light activities. On average, they also slept 6.9 hours, took 7638 steps, and traveled 5.49 km per day. 

![Steps by Day](https://github.com/CoolBeansProgramming/Bellabeat-Case-Study/blob/main/Images/Total%20steps%20by%20day.png?raw=true "Steps by Day")

Users took the most steps on Sundays and the least number of steps on Fridays. As all the values are fairly high, the marketing team can conclude that users value the step feature of health fitness devices. They could also assume that the feature will be very useful for Bellabeat customers. 

![Fairly Active Minutes by Day](https://github.com/CoolBeansProgramming/Bellabeat-Case-Study/blob/main/Images/Minutes%20of%20moderate%20activity%20per%20day.png?raw=true "Fairly Active Minutes by Day")

It is interesting to see that the amount of time spent being fairly active decreased on Wednesdays and then picks back up on Thursdays. This may be due to the fact that most people are going back to work on Monday and then may get discouraged or tired by Wednesday. Wednesday may also be a popular rest day, allowing them to resume their activities on Thursday. 

![Distribution of Total Sleep Time](https://github.com/CoolBeansProgramming/Bellabeat-Case-Study/blob/main/Images/Distribution%20of%20sleep%20time.png?raw=true "Distribution of Total Sleep Time")

From the above histogram, most people slept between 312 and 563 minutes (between 5.2 and 9.4 hours). Note that this does not include the total time spent in bed resting.  

![Calories vs Time Slept](https://github.com/CoolBeansProgramming/Bellabeat-Case-Study/blob/main/Images/Total%20minutes%20Asleep%20vs%20Calories.png?raw=true "Calories vs Time Slept")

Besides a few outliers, calories were burned by those who slept between 5 and 7 hours. If only considering weight loss and calories burned, this aligns with the 5.2 to 9.4 hour sleep range, which may indicate that those who stay withint this range burn more calories.

![Logged Activity Distance by Day](https://github.com/CoolBeansProgramming/Bellabeat-Case-Study/blob/main/Images/Logged%20Activities%20Distance.png?raw=true "Logged Activity Distance by Day")

The logged feature was not used too often as there were many blanks in the data and no records were available  for Thursday and Friday. The highest days of logged distance were on the weekend or times when many people likely have free time to do physical activities. 

## Share 

Check out the daashboard on Tableau Public: [Bellabeat Dashboard](https://public.tableau.com/app/profile/paijetableau/viz/BellaBeatCaseStudy_16610415560350/Dashboard?publish=yes)

## Act

* The number of steps users took was the least on Friday which may be due to user becoming tired at the end of the week. As this is not limited to only FitBit customers, the marketing team could send notifications to users Thursday evenings and Friday and Saturday mornings encouraging users to continue being physical active throughout the day.

* Many users did not use the Logged Distance feature on the FitBit devices. This suggests that users would prefer to have their data collected automatically. The Bellabeat marketing team can decide not to a feature activity distance log function as many users seem to not use this.

* Compared to the data set size, there were very few entries for weight. Of those that were entered, about 2/3 were done manually. The individuals who did not log their weight may not have been concerned with losing weight or did not have the device needed to automatically record this data. Since many did not use the logged distance feature as well, the Bellabeat team could market weight devices like smart scales that automatically record this information.

* Other data sources, like the [Mi Band fitness tracker data (04.2016 - present)](https://www.kaggle.com/datasets/damirgadylyaev/more-than-4-years-of-steps-and-sleep-data-mi-band), could be useful for further exploration as this specific data set follows on individual over the course of six years. 

