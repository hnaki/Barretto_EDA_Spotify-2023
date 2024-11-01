# Barretto_EDA_Spotify-2023


### Introduction to the Application

Millions of songs, podcasts, and other audio content from different artists around the world are available to subscribers of the music streaming service Spotify. It uses a "*freemium*" business strategy, providing a premium, ad-free edition with extra features including offline playback and better audio quality, as well as a free version with advertisements. Spotify facilitates music discovery by using algorithms to generate customized playlists and music recommendations based on user listening preferences. Additionally, the platform has social capabilities that let users make playlists together, share music, and see what their friends are listening to. Spotify is readily accessible on a variety of devices, including computers, tablets, smartphones, and even smart speakers, and is available as an app and online.

![image](https://github.com/user-attachments/assets/9039717c-5151-48b1-9d44-7cebb9090ed5)

### About the Project

In this project, a given dataset with the **Most Streamed Spotify Songs 2023** was performed with an **Exploratory Data Anlysis** (EDA) and used **Data Visualization** to represent the correlation between the dataset values. The given dataset was retrieved by this website:(https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023). The purpose of this project is to examine, picture, and to expound the data for insights.

### _GENERAL GUIDELINES_
1. Begin by familiarizing yourself with the structure of the dataset. Check for missing values and data types, and perform an initial exploration to understand the different features available.
2. Provide summary statistics to give an overview of key metrics such as the number of streams, release dates, and musical attributes (e.g., BPM, danceability).
3. Use appropriate visualizations (e.g., bar charts, histograms, scatter plots) to uncover trends and patterns in the data. Ensure that your plots are well-labeled and easy to interpret.
4. Investigate correlations between different variables and provide insights based on your findings. Explore relationships between streams and other musical characteristics like tempo, energy, or playlists.
5. Based on your analysis, offer any insights or recommendations regarding the tracks, artists, or musical trends that could be useful for understanding what makes a track popular.


### Guide Questions
**Overview of Dataset**
- How many rows and columns does the dataset contain?
- What are the data types of each column? Are there any missing values?
  
**Basic Descriptive Statistics**
- What are the mean, median, and standard deviation of the streams column?
- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

**Top Performers**
- Which track has the highest number of streams? Display the top 5 most streamed tracks.
- Who are the top 5 most frequent artists based on the number of tracks in the dataset?
  
**Temporal Trends**
- Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?
  
**Genre and Music Characteristics**
- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?
  
 **Platform Popularity**
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?
  
 **Advanced Analysis**
- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.


## DOCUMENTATION

**Overview of Dataset**

In order to check how many rows and columns are in the dataset, the dataset must be first loaded into the jupyter notebook. The dataset can be downloaded to a .csv file and was uploaded to the correct folder where the jupyter notebook is also located. After that, the code written below was used to load the .csv file.

```ruby
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

spot_df = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')
spot_df
```

This will load the dataset into the notebook:

![image](https://github.com/user-attachments/assets/fdb38417-c8eb-4b6b-8009-6851c37efcfa)


![image](https://github.com/user-attachments/assets/427cf3ef-8fb6-45e1-aa0d-4eaaff266126)


Using the code **encoding='ISO-8859-1'** will prevent loading errors due to special characters like **#**. There are 953 rows and 24 columns inside the dataset. The columns serves as the categories while the rows serves as the entries, or the songs that were listed in the dataset.

In order to determine the types of categories of the dataset, the code was written below:

```ruby
spot_df.info()
```

This will show the information of the categories of the dataset:

![image](https://github.com/user-attachments/assets/83c27c04-1e06-4db7-a661-d13e1b9d1f4b)


There are 24 different categories shown, like artist(s)_name, bpm, streams, etc.... While there are only two types of data which is an object and an integer. The integer serves as the number values while the object serves as the alphabet characters.

Using the code below will determine if there are any missing values inside the dataset.

```ruby
missing_data = spot_df.isnull().sum()
missing_data
```

According to the output:

![image](https://github.com/user-attachments/assets/8632046a-310b-4863-9c9c-0f2a2b7cdccc)

There are two categories with missing values. 50 outputs are missing in the category **in_shazam_charts** and 95 in the category **key**.


**Basic Descriptive Statistics**


To check the mean and standard deviation of the streams column, using this code can first determine the description of the whole dataset.

```ruby
spot_df['streams'] = pd.to_numeric(spot_df['streams'], errors='coerce')
spot_df.describe()
```

Since the streams before was in object data type, converting it into numeric can show the data description. The output will show all the column categories, now looking for the streams column, determining the mean and standard deviation:

![image](https://github.com/user-attachments/assets/42deeccf-2aef-4a4b-a062-c413eb4f2506)

The mean is 5.141378 x 10^8 streams while the standard deviation is 5.668569 x 10^8 streams.

To determine the median, the code below was used:

```ruby
median = int(spot_df['streams'].median())
print(f'The median is {median} streams.')
```

However, there were no exact number of median streams, so I decided to check what is the closest track near the median streams using the code below:

```ruby
median_row = spot_df.loc[spot_df['streams'] == median, ['artist(s)_name', 'track_name', 'streams']]
# If no exact match, find the row closest to the median
if median_row.empty:
    sorted_spot_df = spot_df.sort_values(by='streams').reset_index(drop=True)
    closest_index = (sorted_spot_df['streams'] - median).abs().idxmin()
    median_row = sorted_spot_df.loc[[closest_index], ['artist(s)_name', 'track_name', 'streams']]
    
print(f'The closest streams to the median is the track name "Every Summertime" by NIKI.')
median_row
```

This will show the output below:

![image](https://github.com/user-attachments/assets/a440924c-d11c-4f6e-9d79-04f0fdd3e281)

The code below shows the distribution of the **released_year** and **artist_count**:

For the released_year:
```ruby
sns.set(style="whitegrid")

# Plot distribution of 'released_year'
plt.figure(figsize=(12, 6))
sns.histplot(spot_df['released_year'], bins=15, kde=True, color='skyblue')
plt.title('Distribution of Track Release Years')
plt.xlabel('Release Year')
plt.ylabel('Count')
plt.show()
```

And for the artist_count:
```ruby
# Plot distribution of 'artist_count'
plt.figure(figsize=(12, 6))
sns.histplot(spot_df['artist_count'], bins=15, kde=True, color='salmon')
plt.title('Distribution of Artist Count per Track')
plt.xlabel('Artist Count')
plt.ylabel('Count')
plt.show()
```

This will show a data visualization chart:

For the released_year:

![image](https://github.com/user-attachments/assets/52c891d1-77f5-4c7e-9ac1-1df05a2b1278)

As the years go, during 1960-1980, there was a few to little released tracks during the year that were still famous for 2023. During 2000-2020, the number of released tracks increased drastically. Around 2020 was almost the tip of how many tracks that are famous for 2023.

For the artist_count:

![image](https://github.com/user-attachments/assets/aadb9dbe-4a14-4afd-9183-7026016863d5)

There are many single artists who have the famous tracks of 2023 and as the artist count increases, the trend goes downward.




**Top Performers**

To determine the top 5 most streamed tracks and their artist(s) name, the code below was used:

```ruby
top_tracks = spot_df.nlargest(5, 'streams')[['artist(s)_name','track_name', 'streams',]]
top_tracks
```

Which will display the output:

![image](https://github.com/user-attachments/assets/4fcde69a-1db1-4a94-a898-a01670c2957f)

The Weekend with the track **Blinding Lights** has the most streams with 3.703895 x 10^9 streams last 2023, followed by Shape of You by Ed Sheeran, Someone you Loved by Lewis Capaldi, Dance Monkey by Tones and I, and Sunflower - Spider-Man: Into the Spider-Verse by Post Malone and Swae Lee.


To determine the top 5 most frequent artists based on the number of tracks in the dataset, the code below was used:

```ruby
top_artists = spot_df['artist(s)_name'].value_counts().head(5)
top_artists
```

Which will display the output:

![image](https://github.com/user-attachments/assets/cb93fc7d-f456-4151-bf4b-ab1496211400)

Taylor Swift has the most number of tracks in the dataset with 34 tracks, followed by the Weekend, Bad Bunny, SZA, and Harry Styles.











### Updates:

  v1.8 - Answered the Top Performers

  v1.7 - Answered the Basic Descriptive Statistics

  v1.4 - Answered the Overview of the Dataset

  v1.0 - Added the Application Definition,Project Description, General Guidelines, and Guide Questions for the Project
      










