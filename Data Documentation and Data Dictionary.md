# Data Documentation and Data Dictionary 

## FitBit Fitness Tracker Data

The Data Dictionary is found [here](https://www.kaggle.com/datasets/arashnic/fitbit/discussion/281341)

This document outlines the data structure and types for the FitBit Fitness Tracker datasets, providing an extensive overview of the various fields and their respective meanings.

1. **Daily Activity (dailyActivity_merged.csv):** This file encapsulates daily aggregate data related to physical activities.
   - *ActivityDate*: Date in mm/dd/yyyy format.
   - *TotalSteps*: Cumulative steps taken.
   - *TotalDistance*: Total distance covered in kilometers.
   - *TrackerDistance*: Distance tracked specifically by the Fitbit device.
   - *LoggedActivitiesDistance*: Distance from manually logged activities.
   - *VeryActiveDistance*: Distance covered during high-intensity activities.
   - *ModeratelyActiveDistance*: Distance covered during moderate-intensity activities.
   - *LightActiveDistance*: Distance covered during light activities.
   - *SedentaryActiveDistance*: Distance covered during minimal activity periods.
   - *VeryActiveMinutes*: Minutes spent in high-intensity activities.
   - *FairlyActiveMinutes*: Minutes spent in moderate activities.
   - *LightlyActiveMinutes*: Minutes spent in light activities.
   - *SedentaryMinutes*: Minutes spent in minimal activity or rest.
   - *Calories*: Estimated total calories burned.

2. **Sleep Day (sleepDay_merged.csv):** This dataset contains details of sleep patterns and durations.
   - *SleepDay*: Date when the sleep was recorded.
   - *TotalSleepRecords*: Count of sleep records for the day, including naps longer than 60 minutes.
   - *TotalMinutesAsleep*: Total duration of sleep in minutes.
   - *TotalTimeInBed*: Total time spent in bed, encompassing all sleep stages and restlessness.

Note: Sleep tracking varies by device, with some supporting automatic detection and others requiring manual input. Supported models and their features are detailed in the dataset.

3. **Weight Log (weightLogInfo_merged.csv):** This file maintains records of weight and related measurements.
   - *Date*: Timestamp of the weight recording.
   - *WeightKg*: Weight in kilograms.
   - *WeightPounds*: Weight in pounds.
   - *Fat*: Recorded body fat percentage.
   - *BMI*: Body Mass Index calculated from user profile data.
   - *IsManualReport*: Indicates if the data was entered manually or synced from a connected scale.
   - *LogId*: Unique identifier for the weight log entry.

Note: Weight data can be entered through various means, including directly on the Fitbit device, through connected scales, or manually via the Fitbit app or website.
