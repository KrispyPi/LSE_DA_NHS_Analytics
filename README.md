### LSE Data Analytics Online Career Accelerator

# Course 2: Data Analytics using Python

## Assignment: Diagnostic Analysis using Python





# 

# Assignment activity 1

### GitHub repository

# My GitHub repository link can be found below

https://github.com/KrispyPi/LSE_DA_NHS_Analytics.git

# Assignment activity 2

### Prepare the workstation

# Import the necessary libraries.
import pandas as pd
import numpy as np

# Optional - Ignore warnings.
import warnings
warnings.filterwarnings('ignore')

# Import and sense-check the actual_duration.csv data set as ad.
ad = pd.read_csv(r'/Users/christospieris/Documents/LSE Data Analytics/Course 2/actual_duration.csv')

# View the DataFrame.
ad.head()

# Import and sense-check the appointments_regional.csv data set as ar.
ar = pd.read_csv(r'/Users/christospieris/Documents/LSE Data Analytics/Course 2/appointments_regional.csv')

# View the DataFrame.
ar.head()

# Import and sense-check the national_categories.xlsx data set as nc.
nc = pd.read_excel(r'/Users/christospieris/Documents/LSE Data Analytics/Course 2/national_categories.xlsx')

# View the DataFrame.
nc

### Exploring the data set

**Question 1:** How many locations are there in the data set?

# Determine the number of locations
print("Number of unique locations in NHS dataset is:", ad['sub_icb_location_name'].nunique())

**Question 2:** What are the five locations with the highest number of records?



# Determine the top five locations based on record count.
print("--------------------------------------------")
print("The 5 top locations with the most records are:")
print("--------------------------------------------")
print(ad['sub_icb_location_name'].value_counts().head(5))

**Question 3:** How many service settings, context types, national categories, and appointment statuses are there?

# Determine the number of service settings.
print("--------------------------------------------")
print("Service Setting Count:")
print("--------------------------------------------")
print(nc['service_setting'].value_counts(normalize=1).mul(100).round(1).astype(str) + '%')


# Determine the number of context types.
print("--------------------------------------------")
print("Context Type Count:")
print("--------------------------------------------")
print(nc['context_type'].value_counts(normalize=1).mul(100).round(1).astype(str) + '%')

# Determine the number of national categories.
print("--------------------------------------------")
print("National Categories Count:")
print("--------------------------------------------")
print(nc['national_category'].value_counts(normalize=1).mul(100).round(1).astype(str) + '%')

# Determine the number of appointment status.
print("--------------------------------------------")
print("Appointment Status Count:")
print("--------------------------------------------")
print(ar['appointment_status'].value_counts(normalize=1).mul(100).round(1).astype(str) + '%')

# 

# Assignment activity 3

**Question 1:** Between what dates were appointments scheduled? 

# View the first five rows of appointment_date for the ad DataFrame to determine the date format.
ad.head(5)


# View the first five rows of appointment_date for the nc DataFrame to determine the date format.
nc.head(5)

# Change the date format of ad['appointment_date'].
from datetime import datetime

#ad['appointment_date'] = pd.to_datetime(ad.appointment_date)
#ad['appointment_date'] = ad['appointment_date'].dt.strftime('%d-%b-%y')
#print(ad.appointment_date)



ad['appointment_date'] = pd.to_datetime(ad.appointment_date)

print(ad['appointment_date'])
# View the DateFrame.


# Change the date format of ar['appointment_date'] OR NC?

nc['appointment_date'] = pd.to_datetime(nc.appointment_date)
nc['appointment_month']= pd.to_datetime(nc.appointment_month)
#print(nc.appointment_date)
print(nc)


#ar['appointment_month'] = pd.to_datetime(ar.appointment_month)
#ar['appointment_month'] = ar['appointment_month'].dt.strftime('%b-%y')
#print(ar.appointment_month)

# View the DateFrame.


# Determine the minimum and maximum dates in the ad DataFrame.
# Use appropriate docstrings.

print('The minimum date in the ad DataFrame is:', ad.appointment_date.min())
print('The maximum date in the ad DataFrame is:', ad.appointment_date.max())


# Determine the minimum and maximum dates in the nc DataFrame.
# Use appropriate docstrings.

print('The minimum date in the nc DataFrame is:', min(nc.appointment_date))
print('The maximum date in the nc DataFrame is:', max(nc.appointment_date))

**Question 2:** Which service setting was the most popular for NHS North West London from 1 January to 1 June 2022?

# For each of these service settings, determine the number of records available for the period and the location. 
nc_NW = nc.query('sub_icb_location_name.str.contains("NHS North West London", case=False).values')
nc_NW_filtered = nc_NW[(nc_NW['appointment_date']>'2021-01-01') & (nc_NW['appointment_date']<'2022-06-01')]

# Initialise an empty list to append the result of the loop
sum_entries=[]
# Run the loop to generate the counter
for type in nc_NW_filtered['service_setting'].unique():
    counter = nc_NW_filtered[nc_NW_filtered['service_setting']== type]
    print("The number of records for", type, "is", counter ['count_of_appointments'].sum())
    sum_entries.append(counter ['count_of_appointments'].sum())
# Print the max value of the list
print("The max value is:",max(sum_entries))
# View the output.


**Question 3:** Which month had the highest number of appointments?

# Number of appointments per month == sum of count_of_appointments by month.
# Use the groupby() and sort_values() functions.

nc_to_be_grouped = nc[['appointment_month','count_of_appointments']]
nc_grouped = nc_to_be_grouped.groupby('appointment_month')
nc_grouped.sum().sort_values('count_of_appointments',ascending=0).head(1)



**Question 4:** What was the total number of records per month?

# Total number of records per month.
number_of_records_per_month = nc_grouped.sum().sort_values('count_of_appointments',ascending=0)
number_of_records_per_month

# 

# Assignment activity 4

### Create visualisations and identify possible monthly and seasonal trends in the data.

# Import the necessary libraries.
import seaborn as sns
import matplotlib.pyplot as plt

# Set figure size.
sns.set(rc={'figure.figsize':(15, 12)})

# Set the plot style as white.
sns.set_style('white')

### Objective 1
Create three visualisations indicating the number of appointments per month for service settings, context types, and national categories.

# Change the data type of the appointment month to string to allow for easier plotting.
nc['appointment_month']=nc['appointment_month'].astype('string')
nc['appointment_month']

# Aggregate on monthly level and determine the sum of records per month.
nc_to_be_grouped = nc[['appointment_month','count_of_appointments',
                       'service_setting','context_type','national_category']]
nc_grouped_by_month = nc_to_be_grouped.groupby('appointment_month').sum()
nc_grouped_by_month
# View output.


**Service settings:**

# Plot the appointments over the available date range, and review the service settings for months.
# Create a lineplot.


nc_ss_grouped_by_month = nc_to_be_grouped.groupby(['appointment_month','service_setting']).sum().reset_index()
nc_ss_grouped_by_month

sns.relplot(
    data=nc_ss_grouped_by_month, 
    x="appointment_month", y="count_of_appointments", hue="service_setting", 
    height=5, aspect=2, 
    kind="line"
)









**Context types:**

# Create a separate data set that can be used in future weeks. 
nc_ct_grouped_by_month = nc_to_be_grouped.groupby(['appointment_month','context_type']).sum().reset_index()



# View output.
nc_ct_grouped_by_month


# Plot the appointments over the available date range, and review the context types for months.
# Create a lineplot.
sns.relplot(
    data=nc_ct_grouped_by_month, 
    x="appointment_month", y="count_of_appointments", hue="context_type", 
    height=5, aspect=2, 
    kind="line"
)

**National categories:**

# Create a separate data set that can be used in future weeks. 

nc_nc_grouped_by_month = nc_to_be_grouped.groupby(['appointment_month','national_category']).sum().reset_index()
nc_nc_grouped_by_month


# View output.


# Plot the appointments over the available date range, and review the national categories for months.
# Create a lineplot.
sns.relplot(
    data=nc_nc_grouped_by_month, 
    x="appointment_month", y="count_of_appointments", hue="national_category", 
    height=5, aspect=2, 
    kind="line"
)

### Objective 2
Create four visualisations indicating the number of appointments for service setting per season. The seasons are summer (August 2021), autumn (October 2021), winter (January 2022), and spring (April 2022).

**Summer (August 2021):**

# Create a separate data set that can be used in future weeks. 

nc_to_be_grouped_by_day = nc[['appointment_date','count_of_appointments','service_setting',
                              'context_type','national_category']]
nc_ss_grouped_by_day = nc_to_be_grouped_by_day.groupby(['appointment_date','service_setting']).sum()
nc_ss_grouped_by_day

nc_ss = nc_ss_grouped_by_day.reset_index()

nc_ss_august = nc_ss.query("appointment_date >= '2021-08-01' and appointment_date < '2021-08-31'")
nc_ss_august

# View output.


# Look at August 2021 in more detail to allow a closer look.
# Create a lineplot.

sns.relplot(
    data=nc_ss_august, 
    x="appointment_date", y="count_of_appointments", hue="service_setting", 
    height=5, aspect=2, 
    kind="line"
)


**Autumn (October 2021):**

# Look at October 2021 in more detail to allow a closer look.
nc_ss_october = nc_ss.query("appointment_date >= '2021-10-01' and appointment_date < '2021-10-31'")
nc_ss_october
# Create a lineplot.
sns.relplot(
    data=nc_ss_october, 
    x="appointment_date", y="count_of_appointments", hue="service_setting", 
    height=5, aspect=2, 
    kind="line"
)




**Winter (January 2022):**

# Look at January 2022 in more detail to allow a closer look.
nc_ss_january = nc_ss.query("appointment_date >= '2022-01-01' and appointment_date < '2022-01-31'")
nc_ss_january
# Create a lineplot.
sns.relplot(
    data=nc_ss_january, 
    x="appointment_date", y="count_of_appointments", hue="service_setting", 
    height=5, aspect=2, 
    kind="line"
)



**Spring (April 2022):**

# Look at April 2022 in more detail to allow a closer look.
nc_ss_april = nc_ss.query("appointment_date >= '2022-04-01' and appointment_date < '2022-04-30'")
nc_ss_april
# Create a lineplot.
sns.relplot(
    data=nc_ss_april, 
    x="appointment_date", y="count_of_appointments", hue="service_setting", 
    height=5, aspect=2, 
    kind="line",
    ci=None
)





# 

# Assignment activity 5

### Analyse tweets from Twitter with hashtags related to healthcare in the UK.

# Libraries and settings needed for analysis
import pandas as pd
import seaborn as sns

# Set figure size.
sns.set(rc={'figure.figsize':(15, 12)})

# Set the plot style as white.
sns.set_style('white')

# Maximum column width to display
pd.options.display.max_colwidth = 200

# Load the data set.

tweets = pd.read_csv(r'/Users/christospieris/Documents/LSE Data Analytics/Course 2/tweets.csv')

# View the DataFrame.

tweets.head(5)

# Explore the metadata.


# Explore the data set.
tweets.info()
tweets.describe()
tweets['tweet_retweet_count'].value_counts(ascending=0)

# Would it be useful to only look at retweeted and favourite tweet messages?
# Explain your answer.

Considering we are interested in tweets that have information of substance to convey one can make the argument that dropping tweets that have not been retweeted would be a safe way forward in reducing the sample size without loosing knowledge. The project decided to continue working on the full dataset to provide the breadth of views expressed on the platform when it comes to NHS. 

# Create a new DataFrame containing only the text.
tweets_text_extracted = tweets.select_dtypes(exclude=['int64','bool'])
tweets_text_extracted.info()
# View the DataFrame.
tweets_text_extracted

# Loop through the messages, and create a list of values containing the # symbol.
tags = []
for y in [x.split(' ') for x in tweets['tweet_full_text'].values]:
    for z in y:
        if '#' in z:
            # Change to lowercase.
            tags.append(z.lower())

# Display the first 30 records.
tags_Series = pd.Series(data=tags)
tags_Series.head(30)


# Convert the series to a DataFrame in preparation for visualisation.
tags_DataFrame = pd.DataFrame(tags_Series, columns = ['Hashtags']).reset_index()
tags_DataFrame['count'] = tags_DataFrame.groupby('Hashtags')['index'].transform('count')
tags_DataFrame=tags_DataFrame.sort_values(by=['count'], ascending=0)

tags_DataFrame
# Rename the columns.


# Fix the count datatype.
tags_DataFrame.info()

# View the result.


# Display records where the count is larger than 10.
tags_DataFrame_G10 = tags_DataFrame[tags_DataFrame.groupby('Hashtags')['count'].transform('size')>10]
tags_DataFrame_G10

# Create a Seaborn barplot indicating records with a count >10 records.
sns.barplot(tags_DataFrame_G10['count'],tags_DataFrame_G10['Hashtags'],palette="magma")
plt.show()

# 

# Assignment activity 6

### Investigate the main concerns posed by the NHS. 

# Prepare your workstation.
# Load the appointments_regional.csv file.
ar

# View the DataFrame.


# Print the min and max dates.
print("The minimum date found in the Regional Appointments data set is:",ar['appointment_month'].min())
print("The maximum date found in the Regional Appointments data set is:",ar['appointment_month'].max())


# Filter the data set to only look at data from 2021-08 onwards.
ar_after_August_2021 = ar.query('appointment_month>"2021-08"')
ar_after_August_2021

**Question 1:** Should the NHS start looking at increasing staff levels? 

# Create an aggregated data set to review the different features.
sns.set_context('paper')

ar_agg = pd.DataFrame(data = ar_after_August_2021, columns =['appointment_month','appointment_status',
                                 'hcp_type','appointment_mode','time_between_book_and_appointment',
                                 'count_of_appointments'])
# Aggregate by appointment status
ar_agg_grouped_by_appointment_status = ar_agg.groupby('appointment_status')['count_of_appointments'].sum().reset_index()
# Create a new column to include the % calculation
ar_agg_grouped_by_appointment_status['%'] = 100 * ar_agg_grouped_by_appointment_status['count_of_appointments'] / ar_agg_grouped_by_appointment_status['count_of_appointments'].sum()
# View the result
ar_agg_grouped_by_appointment_status
# Plot the result in a barplot
sns.barplot(ar_agg_grouped_by_appointment_status['appointment_status'],
            ar_agg_grouped_by_appointment_status['%'],palette="magma")
plt.title('Appointment Status [%]')
plt.show()

# The rest of this code block follows the same pattern as the one above
ar_agg_grouped_by_hcp_type = ar_agg.groupby('hcp_type')['count_of_appointments'].sum().reset_index()
ar_agg_grouped_by_hcp_type['%'] = 100 * ar_agg_grouped_by_appointment_status['count_of_appointments'] / ar_agg_grouped_by_appointment_status['count_of_appointments'].sum()
ar_agg_grouped_by_hcp_type

sns.barplot(ar_agg_grouped_by_hcp_type['hcp_type'],
            ar_agg_grouped_by_hcp_type['%'],palette="magma")
plt.title('HCP Type [%]')
plt.show()

# The rest of this code block follows the same pattern as the one above
ar_agg_grouped_by_appointment_mode = ar_agg.groupby('appointment_mode')['count_of_appointments'].sum().reset_index()
ar_agg_grouped_by_appointment_mode['%'] = 100 * ar_agg_grouped_by_appointment_status['count_of_appointments'] / ar_agg_grouped_by_appointment_status['count_of_appointments'].sum()
ar_agg_grouped_by_appointment_mode

sns.barplot(ar_agg_grouped_by_appointment_mode['appointment_mode'],
            ar_agg_grouped_by_appointment_mode['%'],palette="magma")
plt.title('Appointment Mode [%]')
plt.show()

# The rest of this code block follows the same pattern as the one above
ar_agg_grouped_by_time_between_book_and_appointment = ar_agg.groupby('time_between_book_and_appointment')['count_of_appointments'].sum().reset_index()
ar_agg_grouped_by_time_between_book_and_appointment['%'] = 100 * ar_agg_grouped_by_appointment_status['count_of_appointments'] / ar_agg_grouped_by_appointment_status['count_of_appointments'].sum()
ar_agg_grouped_by_time_between_book_and_appointment

sns.barplot(ar_agg_grouped_by_time_between_book_and_appointment['time_between_book_and_appointment'],
            ar_agg_grouped_by_time_between_book_and_appointment['%'],palette="magma")
plt.title('Time between Booking and Appointment [%]')
plt.show()



# Determine the total number of appointments per month.
ar_Per_Month = ar_agg.groupby('appointment_month')['count_of_appointments'].sum().reset_index()
ar_Per_Month
# Add a new column to indicate the average utilisation of services.
NHS_daily_capacity = 1200000
ar_Per_Month['avg_util (%)']=100*((ar_Per_Month['count_of_appointments']/30)/NHS_daily_capacity).round(1)

# Monthly aggregate / 30 to get to a daily value.


# View the DataFrame.
ar_Per_Month

ar_Per_Month['appointment_month']=ar_Per_Month['appointment_month'].astype('string')

# Plot sum of count of monthly visits.
graph = sns.relplot(
    data=ar_Per_Month, 
    x="appointment_month", y="count_of_appointments",
    height=5, aspect=2, 
    kind="line"
)
monthly_capacity=NHS_daily_capacity*30
x_values=ar_Per_Month['appointment_month']
monthly_capacity

plt.axhline(y=monthly_capacity,linestyle='dashed',color='r')




# Convert the appointment_month to string data type for ease of visualisation.


# Create a lineplot with Seaborn.


# Plot monthly capacity utilisation.
sns.relplot(
    data=ar_Per_Month, 
    x="appointment_month", y="avg_util (%)",
    height=5, aspect=2, 
    kind="line",
    palette="magma"
)
plt.axhline(y=100,linestyle='dashed',color='r')
plt.ylim(0,100)
# Create a lineplot.


**Question 2:** How do the healthcare professional types differ over time?

# Create a line plot to answer the question.
sns.relplot(
    data=ar_agg, 
    x="appointment_month", y="count_of_appointments", hue='hcp_type',
    height=5, aspect=2, 
    kind="line",
    palette="magma"
)

**Question 3:** Are there significant changes in whether or not visits are attended?

# Create a line plot to answer the question.
sns.relplot(
    data=ar_agg, 
    x="appointment_month", y="count_of_appointments", hue='appointment_status',
    height=5, aspect=2, 
    kind="line",
    palette="magma"
)

**Question 4:** Are there changes in terms of appointment type and the busiest months?

# Create a line plot to answer the question.
sns.relplot(
    data=ar_agg, 
    x="appointment_month", y="count_of_appointments", hue='appointment_mode',
    height=5, aspect=2, 
    kind="line",
    palette="magma"
)

**Question 5:** Are there any trends in time between booking an appointment?

# Create a line plot to answer the question.
sns.relplot(
    data=ar_agg, 
    x="appointment_month", y="count_of_appointments", hue='time_between_book_and_appointment',
    height=5, aspect=2, 
    kind="line",
    palette="magma"
)

**Question 6:** How do the spread of service settings compare?

# Let's go back to the national category DataFrame you created in an earlier assignment activity.
nc.head()

# Create a new DataFrame consisting of the month of appointment and the number of appointments.
nc_grouped_by_month = nc.groupby(['appointment_month','service_setting'])['count_of_appointments'].sum().reset_index()
# View the DataFrame.
nc_grouped_by_month

# Create a boxplot to investigate spread of service settings.
sns.barplot(data = nc_grouped_by_month,
            x = 'appointment_month',
            y = 'count_of_appointments',
            hue = 'service_setting',          
            palette="magma")
plt.title('Spread of service settings')
plt.show()

# Create a boxplot to investigate the service settings without GP.
data = nc_grouped_by_month.loc[nc_grouped_by_month['service_setting']!='General Practice']
sns.barplot(data = data,
            x = 'appointment_month',
            y = 'count_of_appointments',
            hue = 'service_setting',          
            palette="magma")
plt.title('Spread of service settings')
plt.show()

# 

### Provide a summary of your findings and recommendations based on the analysis.

> Double click to insert your summary.

Key findings and recommendations

The NHS networks were well resourced to meet demand for the given period, with utilisation averaging 75%. Focus areas should be to mitigate risks were bottlenecks can occur and by refining the current reporting and data management to benefit from data driven operations especially during the busiest months of October, November.

1	Analysing historical data covering a period where the UK was affected by the pandemic, the project showed that only a fraction of the appointments booked were not attended (<10%). It is reasonable to predict that this number will only get smaller in the time after the investigated time window, considering the social norm will be restored post-pandemic. 

2	The project identified that the biggest set of appointments are served by GP. Considering this is an area of potential congestion and bottle-neck formation, it is part of this project’s recommendation that the NHS investigates for alternative ways to deliver appointments were possible. Investing in upskilling other practice staff and / or improving triage stages could be considered to mitigate this risk.

3	Face-to-face meetings were shown to be the most popular. The project suggests that NHS invests in other means to deliver consultation and at the same in educating people to take up alternative routes such as tele-health appointments. Resource management will benefit from the agility that virtual settings can offer.

4	Most appointments were made a day before they were supposed to take place. While this will be a hard requirement for certain health conditions, the project suggests that the NHS promotes longer periods to allow for efficient resource allocation to settle in a more optimal way, given that with more time both the demand and supply will be able to make arrangements and indeed be successful at the point of contact.

5	The analysis showed that the NHS resource networks were well prepared for the given period. Utilisation never came close to exceeding capacity, indicating that the NHS should focus in optimising the existing resource allocation as well as reducing the missed appointments rather than injecting head count.

6	Investing in robust reporting and data management is also an area that the NHS can benefit from improving. This was identified by revealing trends in the service setting spread, indicating that the unmapped appointments were the second largest service setting, especially during the busiest months. This means that when busy the NHS reporting suffers, when this is when data driven management will help the most in resource allocation and operational management.


