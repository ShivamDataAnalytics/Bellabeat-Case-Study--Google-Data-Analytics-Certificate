# Bellabeat-Case-Study--Google-Data-Analytics-Certificate

Author : Shivam Bajpai 

The case study follows the six step data analysis process:

❓ Ask
💻 Prepare
🛠 Process
📊 Analyze
📋 Share
🧗‍♀️ Act

Scenario

Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women. 
The company has 5 focus products: bellabeat app, leaf, time, spring and bellabeat membership. Bellabeat is a successful small company, 
but they have the potential to become a larger player in the global smart device market. Our team have been asked to analyze smart device data to gain insight into how consumers are using their smart devices. 
The insights we discover will then help guide marketing strategy for the company.


1. Ask

BUSINESS TASK: Analyze Fitbit data to gain insight and help guide marketing strategy for Bellabeat to grow as a global player.

Primary stakeholders: Urška Sršen and Sando Mur, executive team members.

Secondary stakeholders: Bellabeat marketing analytics team.

2. Prepare

Data Source: 30 participants FitBit Fitness Tracker Data from Mobius: https://www.kaggle.com/arashnic/fitbit

The dataset has 18 CSV. The data also follow a ROCCC approach:

1) Reliability: The data is from 30 FitBit users who consented to the submission of personal tracker data and generated by from a distributed survey via Amazon Mechanical Turk.
2) Original: The data is from 30 FitBit users who consented to the submission of personal tracker data via Amazon Mechanical Turk.
3) Comprehensive: Data minute-level output for physical activity, heart rate, and sleep monitoring. While the data tracks many factors in the user activity and sleep, but the sample size is small and most data is recorded during certain days of the week.
4) Current: Data is from March 2016 to May 2016. Data is not current so the users habit may be different now.
5) Cited: Unknown.

The dataset has limitations:

*Only 30 user data is available. The central limit theorem general rule of n≥30 applies and we can use the t test for statstic reference. However, a larger sample size is preferred for the analysis.

*Upon further investigation with n_distinct() to check for unique user Id, the set has 33 user data from daily activity, 24 from sleep and only 8 from weight. There are 3 extra users and some users did not record their data for tracking daily activity and sleep.


*For the 8 user data for weight, 5 users manually entered their weight and 3 recorded via a connected wifi device (eg: wifi scale).

*Most data is recorded from Tuesday to Thursday, which may not be comprehensive enough to form an accurate analysis.

-- For the scope of this data analysis only a selection of the 18 data sets that were deemed relevant in addressing the business task were imported into RStudio.

First step is to install the packages required for the task coding 

install.packages("readr")
install.packages("tidyverse")
install.packages("dplyr")
install.packages("skimr")
install.packages("janitor")
install.packages("ggplot2")
install.packages("gridExtra")
install.packages("lubridate")
install.packages("plotly")
install.packages("here")



Below are the data frames prepared by loading the original data sets using read.csv() 

dailyActivity <- read_csv("dailyActivity_merged.csv")
dailySleep <- read_csv("sleepDay_merged.csv")
weightLog <- read_csv("weightLogInfo_merged.csv")
hr <- read_csv("heartrate_seconds_merged.csv")
hourlyIntensities <- read_csv("hourlyIntensities_merged.csv")
hourly_step <- read.csv("hourlySteps_merged.csv")
calories <- read.csv("dailyCalories_merged.csv")
hourlyCalories<-read.csv("hourlyCalories_merged.csv")


3.Processing the Data

Using glimpse()function we can view the data with as much information it can show .Its a transpose way of print().

glimpse(dailyActivity)

glimpse(hourlyIntensities)

glimpse(hr)

glimpse(dailySleep)

glimpse(weightLog)

glimpse(calories)

glimpse(hourly_step)

Also to quickly view only  how our data frame looks like we can use head() function to view top 5 rows




#To make all comumn names uniform and consistent , we have used clean_names function.


clean_names(dailySleep)
clean_names(dailyActivity)
clean_names(hourlyIntensities)
clean_names(hr)
clean_names(dailySleep)
clean_names(weightLog)
clean_names(calories)
clean_names(hourly_step)


4.Analyze


#Checking for empty or null values
sum(is.na(dailyActivity))
sum(is.na(hourlyIntensities))
sum(is.na(hr))
sum(is.na(dailySleep))
sum(is.na(calories))
sum(is.na(hourlyCalories))
sum(is.na(hourly_step))


Next we have checked for all the duplicates for our selected data frames 

#to check for duplicates in all the data frames 

sum(duplicated(dailySleep))

sum(duplicated(dailyActivity))

sum(duplicated(hourlyIntensities))

sum(duplicated(hr))

sum(duplicated(dailySleep))

sum(duplicated(weightLog))

sum(duplicated(calories))

sum(duplicated(hourly_step))

sum(duplicated(hourlyCalories))

#Removed duplicates :

dailySleep <- dailySleep[!duplicated(dailySleep), ]
sum(duplicated(dailySleep))



## Summarizing key elements
#dailyActivity

dailyActivity %>%  
  select(TotalSteps, SedentaryMinutes, Calories) %>%
  summary()

# explore num of active minutes per category

dailyActivity %>%
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
  summary()

# explore dailySleep

dailySleep %>%
  select(TotalSleepRecords,TotalMinutesAsleep,TotalTimeInBed) %>%
  summary()

# explore weightLog

weightLog %>%
  select(WeightKg,WeightPounds,BMI) %>%
  summary()

# explore hourlyIntensities table( data frame)

hourlyIntensities %>%
  select(TotalIntensity,AverageIntensity) %>%
  summary()

# explore calories table( data frame)

calories %>%
  select(Calories) %>%
  summary()

# explore hourly_step table( data frame)

hourly_step %>%
  select(StepTotal) %>%
  summary()



### Proceeding for Data Visualization (Share)

#Visualizing graph between Total active minutes vs Calories burnt 

dailyActivity %>% 
  mutate(TotalActiveMinutes = VeryActiveMinutes+FairlyActiveMinutes+LightlyActiveMinutes) %>% 
  ggplot()+geom_point(mapping=aes(x=TotalActiveMinutes, y=Calories))+
  geom_smooth(mapping=aes(x=TotalActiveMinutes, y=Calories))+
  labs(title="Total Active Time vs Calories Burned")
  
  
  ![Rplot(Total Active time vs Calories burned)](https://user-images.githubusercontent.com/116336536/197337700-b54ab491-6ddf-486c-a49b-6a6f4e40a134.png)


#Very Intense Activity vs Calories Burned

dailyActivity %>% 
  group_by(Ymd) %>% 
  summarize(MeanVeryActive=mean(VeryActiveMinutes), MeanCalories=mean(Calories)) %>% 
  ggplot()+geom_line(mapping=aes(x=MeanVeryActive, y=MeanCalories))+
  labs(title="Very Intense Activity vs Calories Burned")
  
  ![Very Intense Activity vs Calories Burned](https://user-images.githubusercontent.com/116336536/197337772-581723b1-78a4-43c7-b6c4-3a49899a97b5.png)


# Moderately Intense Activity vs Calories Burned per day

dailyActivity %>% 
  group_by(Ymd) %>% 
  summarize(MeanFairlyActive=mean(FairlyActiveMinutes), MeanCalories=mean(Calories)) %>% 
  ggplot()+geom_line(mapping=aes(x=MeanFairlyActive, y=MeanCalories))+
  labs(title="Moderate Activity vs Calories Burned")
  
  ![Moderate Activity vs Calories Burned](https://user-images.githubusercontent.com/116336536/197337806-ae832278-cc8a-4e06-bf7a-f406ecc8bafa.png)


# Lightly Intense Activity vs Calories Burned per day

dailyActivity %>% 
  group_by(Ymd) %>% 
  summarize(MeanLightlyActive=mean(LightlyActiveMinutes), MeanCalories=mean(Calories)) %>% 
  ggplot()+geom_line(mapping=aes(x=MeanLightlyActive, y=MeanCalories))+
  labs(title="Light Activity vs Calories Burned")
  
  ![Light Activity vs Calories Burned](https://user-images.githubusercontent.com/116336536/197337851-f060c643-3aea-47f2-9835-c4698db72ff5.png)


# Total Distance vs Calories burned
dailyActivity %>%  
  group_by(Ymd,Id) %>% 
  summarize(TotalCalories=sum(Calories), distance=sum(TotalDistance)) %>% 
  ggplot()+ geom_point(aes(x=distance, y=TotalCalories, color=Ymd))+
  geom_smooth(aes(x=distance, y=TotalCalories))+
  labs(title="Total Distance vs Calories Burned", subtitle ="Sample of the Thirty Three Participants")
  
  ![Total Distance vs Calories Burned](https://user-images.githubusercontent.com/116336536/197337875-92308043-64cc-4d80-a192-0163e3ece1f4.png)

  
  
  # visualizing the relationship between sleep and calories burned:
dailyActivitywithSleep %>% 
  group_by(Id) %>% 
  drop_na() %>% 
  summarize(total_calories=sum(Calories), total_sleep=sum(TotalMinutesAsleep)) %>% 
  ggplot()+geom_point(aes(x=total_calories, y=total_sleep))+
  geom_smooth(aes(x=total_calories, y=total_sleep))
  
  ![relationship between sleep and calories burned](https://user-images.githubusercontent.com/116336536/197337922-78bebcf0-d36c-4497-85cb-4b429fc3522e.png)

  
  #From above visualization we find, the more calories one burned the longer they slept. For better sleep, the CDC recommends getting some exercise because being physically active during the day helps one fall asleep easily in the night.


# visualizing the relationship between the heart rate and Total Steps

heart_rate %>% 
  drop_na() %>% 
  ggplot()+ geom_point(aes(x=mean_heartrate, y=TotalSteps))+
  geom_smooth(aes(x=mean_heartrate, y=TotalSteps))+
  labs(title="Mean Heart Rate vs Total Steps", subtitle="Relationship between the Mean Heart Rate and Total Steps")
  
  
  ![MeanHeartRate vs TotalSteps](https://user-images.githubusercontent.com/116336536/197337946-bce2a5b0-4c68-4d69-a6cd-30828b1cdcdb.png)


#The American Heart Association defines normal resting adult human heart rate as 60 to 100 beats per minute, and tachycardia, high heart rate, at over 100 beats per minute at rest. Exercise strengthens one's heart. 
#During exercise, the heart pumps more blood with each heart beat and does not need to pump as hard - which lowers heart rate. This plot is according to the scientific facts .

#evaluating the relationship between heart Rate evaluation and sleep levels. We will combine the sleep data and the summarized heart rate evaluation data using the merge() function:

sleep_levels = merge(x=dailySleep, y=heart_rate_evaluation, by=c("Id", "Ymd"), all=TRUE)

# visualize the relationship between the heart rate and sleeping time

sleep_levels %>% 
  drop_na() %>% 
  ggplot()+ geom_point(aes(x=mean_heartrate, y=TotalMinutesAsleep))+
  geom_smooth(aes(x=mean_heartrate, y=TotalMinutesAsleep))+
  labs(title="Mean Heart Rate vs Total Sleeping Time", subtitle="Relationship between the Mean Heart Rate and Total Sleeping Time")
  
  ![Mean Heart Rate vs Total Sleeping Time](https://user-images.githubusercontent.com/116336536/197337977-796e47f4-ced3-4e22-80aa-a45978cd3b0d.png)

  
#From the above visualization we find that that the lower the heart rate, the more the sleeping time. According to the American Heart Association, one's heart beat may fall to 60 beats per minute during sleep. This is normal. Notably, participants with high heart rates got less sleeping time.

# Trends Identify

The average Total Steps per day for participants was 8053, over 2000 steps below the necessary minimum.
In a month, participants spent 79% of their time sedentary.
The participants averaged 25.19 BMI, which is overweight.
On average, participants slept less than the recommended 7 hours.
The participants weren’t consistent with logging/tracking their data each day, and some didn’t log/track their sleep or weight (only 24 unique users for sleep and eight for weight — where only two of these eight were made up the majority of the inputs).
Participants didn’t lose weight, improve BMI or sleep quality, or increase exercise levels.


# Recommendations (Act)


Bellabeat should enable in-app tournaments against friends or users in the same city/state to encourage continuous tracking.
Bellabeat could offer rewards or points redeemable for merchandise, discounts on future products, in-app features, or raffle tickets.
Bellabeat may give extra incentives (i.e., points) from Friday-Monday when many people lose interest and motivation.
Bellabeat goods should offer a TDEE calculator so consumers may input their sex, age, weight, height, and other information for accurate results.

Marketing recommendations to expand globally:

🔢 Obtain more data for an accurate analysis, encouraging users to use a wifi-connected scale instead of manual weight entries.
🚲 Educational healthy style campaign encourages users to have short active exercises during the week, longer during the weekends, especially on Sunday where we see the lowest steps and most sedentary minutes.
🎁 Educational healthy style campaign can pair with a point-award incentive system. Users completing the whole week's exercise will receive Bellabeat points on products/memberships.
🏃‍♂️ The product, such as Leaf wellness tracker, can beat or vibrate after a prolonged period of sedentary minutes, signaling the user it's time to get active! Similarly, it can also remind the user it's time to sleep after sensing a prolonged awake time in bed.
