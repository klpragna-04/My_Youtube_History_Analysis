# My YouTube History Analysis
![Youtube_Logo](Youtube_logo1.jpeg)
## Overview

This project provides a detailed analysis of my YouTube watch history, uncovering insights into viewing habits, favorite channels, and types of content consumed over the past two years. The data was extracted from an HTML file obtained via **Google Takeout**, allowing for a comprehensive examination of viewing patterns.

## Table of Contents

- [Introduction](#introduction)
- [Data Collection](#data-collection)
- [Data Extraction](#data-extraction)
- [Data Processing](#data-processing)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Visualizations](#visualizations)
- [Key Findings](#key-findings)
- [Technologies Used](#technologies-used)
- [Conclusion](#conclusion)
- [License](#license)

## Introduction

In an age of digital content consumption, understanding one's viewing habits can provide valuable insights. This project explores various aspects of my YouTube watch history from the past two years, focusing on the most-watched videos, recurring topics in video titles, active viewing times, and channel statistics.

## Data Collection

The data for this project was collected from my YouTube watch history, which was exported as an HTML file using **Google Takeout**. Google Takeout is a service that allows users to export their data from various Google services, including YouTube, providing a structured file of all interactions.

## Data Extraction

To extract useful data from the HTML file, I utilized **Beautiful Soup**, a Python library that simplifies the process of parsing HTML and XML documents.

### Steps for Data Extraction:

1. **Load the HTML File**: Open the HTML file containing the YouTube watch history.
2. **Parse the HTML**: Use Beautiful Soup to navigate the HTML tree structure.
   - **find()**: Used to locate the first occurrence of a tag.
   - **find_all()**: Used to find all occurrences of a tag and extract relevant attributes such as video titles, URLs, channel names, watch dates, and time spent watching.
3. **Store Data**: The extracted data is stored in lists, which are then converted into a Pandas DataFrame for further analysis.

```python
from bs4 import BeautifulSoup
import pandas as pd

# Load the HTML file
with open('path_to_your_file.html', 'r', encoding='utf-8') as file:
    soup = BeautifulSoup(file, 'html.parser')

# Extract data
video_data = []
for video in soup.find_all('div', class_='video-class'):
    title = video.find('h3').text
    link = video.find('a')['href']
    channel = video.find('span', class_='channel-class').text
    date = video.find('span', class_='date-class').text
    time_spent = video.find('span', class_='time-class').text
    video_data.append([title, link, channel, date, time_spent])
```

## Data Processing

After extracting the data, I created a DataFrame using **Pandas** for easier manipulation and analysis. The following processing steps were performed:

1. **Create DataFrame**: Construct a DataFrame from the extracted data.
2. **Convert Date Formats**: Convert the `date` column to a datetime format for accurate time-series analysis.
3. **Time Formatting**: Create a function to convert the formatted time spent (e.g., "1h 23m") into a total seconds format for numerical analysis.
   ```python
   import re

   def convert_time_to_seconds(time_str):
       if pd.isna(time_str):
           return 0
       time_parts = list(map(int, re.findall(r'\d+', time_str)))
       if 'h' in time_str:
           return time_parts[0] * 3600 + time_parts[1] * 60 + time_parts[2]
       elif 'm' in time_str:   
           return time_parts[0] * 60 + time_parts[1]
       else:
           return time_parts[0]
   ```

4. **Feature Engineering**: New columns were created, such as:
   - **day_of_week**: Extracted from the date for analyzing weekly patterns.
   - **hour_of_day**: Extracted from the watch time for hourly activity analysis.

## Exploratory Data Analysis (EDA)

Using **Matplotlib** and **Seaborn**, several visual analyses were conducted to gain insights into my YouTube watching habits:

1. **Activity by Day**: A bar plot was generated to visualize the most active days for watching videos.
2. **Viewing Patterns**: A heatmap was created to analyze viewing activity by hour of the day and day of the week, allowing for an understanding of peak viewing times.
3. **Top Channels and Videos**: Additional analysis was conducted to identify the channels with the most videos watched and the most viewed content.

### Sample Visualizations:
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Bar plot for most active days
day_counts = df['day_of_week'].value_counts()
plt.figure(figsize=(10, 6))
sns.barplot(x=day_counts.index, y=day_counts.values, palette='Oranges_d')
plt.title('Most Active Days for Watching Videos')
plt.xlabel('Day of Week')
plt.ylabel('Number of Videos Watched')
plt.show()
```

## Visualizations

### Heatmap of Watching Activity by Day and Hour
The heatmap shows the correlation between the day of the week and the hour of the day, helping identify the best times for video consumption.

### Bar Chart of Top Channels
A bar chart illustrating the top channels based on the number of videos watched.

## Key Findings

- **Viewing Habits**: Identified peak viewing times and days over the past two years, suggesting when content consumption is highest.
- **Content Preferences**: Discovered trends in video types and channel preferences, indicating a strong interest in specific genres.
- **Engagement**: Analyzed the total time spent watching and how it correlates with the number of videos watched across different channels.

## Technologies Used

- **Python**: The primary programming language for data analysis and visualization.
- **Beautiful Soup**: For parsing HTML and extracting data from the YouTube history file.
- **Pandas**: For data manipulation and DataFrame management.
- **Matplotlib** & **Seaborn**: For creating visualizations and charts.

## Conclusion

This project demonstrates the power of data analysis in uncovering personal consumption patterns in digital media over the past two years. The methodologies applied can be extended to other datasets for similar exploratory analysis. The insights gained can help tailor future content consumption and provide a deeper understanding of media habits.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


