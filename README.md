# LSE_DA_NHS_Analytics
Repo to cover the analysis on NHS data as part of the LSE Data Analytics program 

### LSE Data Analytics Online Career Accelerator

# Course 2: Data Analytics using Python

## Assignment: Diagnostic Analysis using Python

You’ll be working with real-world data to address a problem faced by the National Health Service (NHS). The analysis will require you to utilise Python to explore the available data, create visualisations to identify trends, and extract meaningful insights to inform decision-making. 

### A note for students using this template
This Jupyter Notebook is a template you can use to complete the Course 2 assignment: Diagnostic Analysis using Python. 

Keep in mind: 
- You are **not required** to use this template to complete the assignment. 
- If you decide to use this template for your assignment, make a copy of the notebook and save it using the assignment naming convention: **LastName_FirstName_DA201_Assignment_Notebook.ipynb**.
- The workflow suggested in this template follows the Assignment Activities throughout the course.
- Refer to the guidance on the Assignment Activity pages for specific details. 
- The markup and comments in this template identify the key elements you need to complete before submitting the assignment.
- Make this notebook your own by adding your process notes and rationale using markdown, add links, screenshots, or images to support your analysis, refine or clarify the comments, and change the workflow to suit your process.
- All elements should be functional and visible in your Notebook. 
- Be sure to push your notebook to GitHub after completing each Assignment Activity.

 > ***Markdown*** Remember to change cell types to `Markdown`. You can review [Markdown basics](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) to find out how to add formatted text, links, and images to your notebook.

# 

# Assignment activity 1

### Insert proof of your GitHub repository. This can be a link or screenshot showing your repo.

# My GitHub repository link can be found below

https://github.com/KrispyPi/LSE_DA_NHS_Analytics.git

# Assignment activity 2

### Prepare your workstation

# Import the necessary libraries.
import pandas as pd
import numpy as np

# Optional - Ignore warnings.
import warnings
warnings.filterwarnings('ignore')

# Import and sense-check the actual_duration.csv data set as ad.
ad = pd.read_csv(r'/Users/christospieris/Documents/LSE Data Analytics/Course 2/actual_duration.csv')

# View the DataFrame.
print(ad.head())

# Determine whether there are missing values.


# Determine the metadata of the data set.


# Determine the descriptive statistics of the data set.


# Import and sense-check the appointments_regional.csv data set as ar.
ar = pd.read_csv(r'/Users/christospieris/Documents/LSE Data Analytics/Course 2/appointments_regional.csv')

# View the DataFrame.
print(ar.head())

# Determine whether there are missing values.


# Determine the metadata of the data set.


# Determine the descriptive statistics of the data set.


# Import and sense-check the national_categories.xlsx data set as nc.
nc = pd.read_excel(r'/Users/christospieris/Documents/LSE Data Analytics/Course 2/national_categories.xlsx')

# View the DataFrame.
print(nc.head())

# Determine whether there are missing values.


# Determine the metadata of the data set.


# Determine the descriptive statistics of the data set.


### Explore the data set

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

### Continue to explore the data and search for answers to more specific questions posed by the NHS.

**Question 1:** Between what dates were appointments scheduled? 

# View the first five rows of appointment_date for the ad DataFrame to determine the date format.


# View the first five rows of appointment_date for the nc DataFrame to determine the date format.


# Change the date format of ad['appointment_date'].


# View the DateFrame.


# Change the date format of ar['appointment_date'].


# View the DateFrame.


# Determine the minimum and maximum dates in the ad DataFrame.
# Use appropriate docstrings.


# Determine the minimum and maximum dates in the nc DataFrame.
# Use appropriate docstrings.


**Question 2:** Which service setting was the most popular for NHS North West London from 1 January to 1 June 2022?

# For each of these service settings, determine the number of records available for the period and the location. 


# View the output.


**Question 3:** Which month had the highest number of appointments?

# Number of appointments per month == sum of count_of_appointments by month.
# Use the groupby() and sort_values() functions.


**Question 4:** What was the total number of records per month?

# Total number of records per month.


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


# Aggregate on monthly level and determine the sum of records per month.


# View output.


**Service settings:**

# Plot the appointments over the available date range, and review the service settings for months.
# Create a lineplot.


**Context types:**

# Create a separate data set that can be used in future weeks. 


# View output.


# Plot the appointments over the available date range, and review the context types for months.
# Create a lineplot.


**National categories:**

# Create a separate data set that can be used in future weeks. 


# View output.


# Plot the appointments over the available date range, and review the national categories for months.
# Create a lineplot.


### Objective 2
Create four visualisations indicating the number of appointments for service setting per season. The seasons are summer (August 2021), autumn (October 2021), winter (January 2022), and spring (April 2022).

**Summer (August 2021):**

# Create a separate data set that can be used in future weeks. 


# View output.


# Look at August 2021 in more detail to allow a closer look.
# Create a lineplot.


**Autumn (October 2021):**

# Look at October 2021 in more detail to allow a closer look.
# Create a lineplot.


**Winter (January 2022):**

# Look at January 2022 in more detail to allow a closer look.
# Create a lineplot.


**Spring (April 2022):**

# Look at April 2022 in more detail to allow a closer look.
# Create a lineplot.


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


# View the DataFrame.


# Explore the metadata.


# Explore the data set.


# Would it be useful to only look at retweeted and favourite tweet messages?
# Explain your answer.


# Create a new DataFrame containing only the text.


# View the DataFrame.


# Loop through the messages, and create a list of values containing the # symbol.


# Display the first 30 records.


# Convert the series to a DataFrame in preparation for visualisation.


# Rename the columns.


# Fix the count datatype.


# View the result.


# Display records where the count is larger than 10.


# Create a Seaborn barplot indicating records with a count >10 records.


# Create the plot.


# View the barplot.


# 

# Assignment activity 6

### Investigate the main cencerns posed by the NHS. 

# Prepare your workstation.
# Load the appointments_regional.csv file.


# View the DataFrame.


# Print the min and max dates.


# Filter the data set to only look at data from 2021-08 onwards.


**Question 1:** Should the NHS start looking at increasing staff levels? 

# Create an aggregated data set to review the different features.


# View the DataFrame.


# Determine the total number of appointments per month.


# Add a new column to indicate the average utilisation of services.
# Monthly aggregate / 30 to get to a daily value.


# View the DataFrame.


# Plot sum of count of monthly visits.
# Convert the appointment_month to string data type for ease of visualisation.


# Create a lineplot with Seaborn.


# Plot monthly capacity utilisation.


# Create a lineplot.


**Question 2:** How do the healthcare professional types differ over time?

# Create a line plot to answer the question.


**Question 3:** Are there significant changes in whether or not visits are attended?

# Create a line plot to answer the question.


**Question 4:** Are there changes in terms of appointment type and the busiest months?

# Create a line plot to answer the question.


**Question 5:** Are there any trends in time between booking an appointment?

# Create a line plot to answer the question.


**Question 6:** How do the spread of service settings compare?

# Let's go back to the national category DataFrame you created in an earlier assignment activity.


# Create a new DataFrame consisting of the month of appointment and the number of appointments.

# View the DataFrame.


# Create a boxplot to investigate spread of service settings.


# Create a boxplot to investigate the service settings without GP.


# 

### Provide a summary of your findings and recommendations based on the analysis.

> Double click to insert your summary.

