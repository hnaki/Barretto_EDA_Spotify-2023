# Barretto_EDA_Spotify-2023

Name: Barretto, Aron Caleb R.

Section: 2ECE-D

Date: 11/02/2024

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


**Temporal Trends**

To check the number of tracks released every year, the code below was used:

```ruby
tracks_per_year = spot_df['released_year'].value_counts().sort_index()
plt.figure(figsize=(12, 5))
sns.lineplot(x=tracks_per_year.index, y=tracks_per_year.values)
plt.title('Number of Tracks Released per Year')
plt.xlabel('Year')
plt.ylabel('Number of Tracks')
plt.show()
```

Showing a line graph as an output:

![image](https://github.com/user-attachments/assets/dfb19d36-bd16-49ad-83cb-88bd83e25c3b)


The graph shows the number of tracks that were released during the years. There was small increases of the number of tracks during the 1960's to 2000's that were famous for 2023. Then it increased and in the 2020's there was a huge number of tracks that were released that were famous in 2023.

Now to check if the number of tracks released per month have any patterns, the code below was used:

```ruby
monthly_releases = spot_df['released_month'].value_counts().sort_index()
plt.figure(figsize=(10, 6))
sns.barplot(x=monthly_releases.index, y=monthly_releases.values, hue=monthly_releases.index, dodge=False, legend=False)
plt.title('Number of Tracks Released Per Month')
plt.xlabel('Month')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show()
```

This will show a bar graph with the number of tracks released per month, with the month numbers starting from 1 to 12 (January-December):

![image](https://github.com/user-attachments/assets/2db9c5ed-b963-491c-b85e-702aa909697a)

January has the most released tracks that were streamed the most in 2023, second is May, and August has the least number of track releases. Every start of the year, artists tend to release their new song tracks as a fresh batch of tracks for the people to listen to.


**Genre and Music Characteristics**

To determine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%, the code below was used:

```ruby
correlation_matrix = spot_df[['streams', 'bpm', 'danceability_%', 'energy_%']].corr()
sns.heatmap(correlation_matrix, annot=True, cmap='viridis')
plt.title('Correlation Between Streams and Musical Attributes')
plt.show()
```

This will show a heat map that with the correlation below:

![image](https://github.com/user-attachments/assets/00fdcecd-223b-46fc-9c6e-01b0f8c40446)


The correlation between the streams and bpm has a negative correlation with -0.0024. This suggests that there is no relationship on the number of streams and the bpm of the track. 

The correlation between the streams and danceability_% has a weak negative correlation with -0.11. This suggests that it is a weak negative correlation, implying a light tendency for more danceable tracks to have fewer streams, but the effect is minimal and likely not significant.

The correlation between the streams and the energy_% has a a negative correlation with -0.026. This suggests that there is no relationship between the number of streams and how it gives energy to the listeners.


We can also determine the correlation between danceability_% and energy_% from the given heat map. There is a positive weak relationship between the two with 0.2. This implies that the more the track is danceable, the more energy levels are produced, however, based on the code below:

```ruby
plt.figure(figsize=(10, 6))
sns.regplot(x='danceability_%', y='energy_%', data=spot_df, scatter_kws={'alpha':0.5}, line_kws={'color': 'red'})
plt.title('Correlation Between Danceability and Energy')
plt.xlabel('Danceability (%)')
plt.ylabel('Energy (%)')
plt.show()
```

This will show why the correlation has a low value:

![image](https://github.com/user-attachments/assets/00ebd050-dfc5-4446-8666-db9dae3d2557)

The blue dots represent the tracks that are scattered around the graph, the more dispersed the dots are, the lesser the correlation between the two are. 

To determine the correlation with valence_& and acousticness_%, another scatter plot was used:

```ruby
plt.figure(figsize=(10, 6))
sns.regplot(x='valence_%', y='acousticness_%', data=spot_df, scatter_kws={'alpha':0.5}, line_kws={'color': 'red'})
plt.title('Correlation Between Valence and Acousticness')
plt.xlabel('Valence (%)')
plt.ylabel('Acousticness (%)')
plt.show()
```

![image](https://github.com/user-attachments/assets/53b2108f-1746-45a3-a4e7-77e105330c67)


The graph implies that the more valence_% of the songs are, the less acousticness_% is. This weak inverse relationship might indicate that tracks with high positivity or valence are less likely to be acoustic, potentially because more upbeat or happy tracks are produced with electronic or synthesized elements.


 **Platform Popularity**

 In order to compare the values of spotify playlists, spotify charts, and apple playlists, the code below was used:
 
```ruby
platforms = ['in_spotify_playlists', 'in_spotify_charts', 'in_apple_playlists']
platform_counts = spot_df[platforms].sum()
platform_counts.plot(kind='bar', title='Track Counts Across Platforms', color='black')
plt.ylabel('Count')
plt.show()
```

This show a graph with the count of 1 million and noticing the graph, the spotify charts has no visible bar due to how little the combined values of all the tracks. Spotify playlists have the most value of most famous tracks nearly 5 million tracks were saved by users.

![image](https://github.com/user-attachments/assets/9b998e05-fa3f-4396-819a-586b9b7dd163)


 **Advanced Analysis**

 To check what key and mode patterns have in the streams column, the code below was used:

```ruby
key_mode_analysis = spot_df.groupby(['key', 'mode'])['streams'].mean().unstack()
key_mode_analysis.plot(kind='bar', title='Average Streams by Key and Mode')
plt.ylabel('Average Streams')
plt.xticks(rotation=45)
plt.show()
```
This will show the different values of streams with the key or mode used for the track:

![image](https://github.com/user-attachments/assets/3a69396a-242b-4277-883b-c4a46d5ce047)

Looking at the graph, with the count of 100 million streams; the key of E and with a mode of a major has the most number of average streams that are in the most streamed songs of 2023. On the other hand, the key of F# and a mode of a minor has the most number of average streams. Only 3 keys (A, B, and F#) with the mode of a minor has more streams than the major mode. This means that a major mode would make a track more streamed.


Now to determine the most frequently appearing artists appearing in playlists and in charts, I separated the graph to check the difference by using the codes below:

```ruby
spot_df['in_deezer_playlists'] = pd.to_numeric(spot_df['in_deezer_playlists'], errors='coerce').astype(int)

playlist_columns = ['in_spotify_playlists', 'in_apple_playlists', 'in_deezer_playlists']

artist_playlist_counts = spot_df.groupby('artist(s)_name')[playlist_columns].sum()

artist_playlist_counts['total_playlist_appearances'] = artist_playlist_counts.sum(axis=1)

top_playlist_artists = artist_playlist_counts.sort_values(by='total_playlist_appearances', ascending=False).head(10)

plt.figure(figsize=(12, 8))
sns.barplot(y=top_playlist_artists.index, x=top_playlist_artists['total_playlist_appearances'], hue=top_playlist_artists.index, dodge=False, legend=False)
plt.title('Top 10 Artists by Playlist Appearances')
plt.xlabel('Total Appearances in Playlists')
plt.ylabel('Artist')
plt.show()
```

For playlists, I converted the 'in_deezer_playlists' due to having a different data type than the rest. Then I grouped the data inside the platforms. After that, I grouped the artists and the sum playlist appearances of each artist across the platforms. Then I calculate the total sum playlists appearances and sorted it from greatest to least. I only picked the top 10 artists for better visualization purposes.

Plotting it in the graph would look like this:

![image](https://github.com/user-attachments/assets/3900ed4c-918d-4be4-8a29-c20c274260e9)


Similar to the charts, an addition of 'in_shazam_charts' was introduced, and was converted. However there are no values in some tracks, so I decided to disregard those without values with just 0. Then using the same code above but instead changing the playlists to the charts:

```ruby
spot_df['in_shazam_charts'] = pd.to_numeric(spot_df['in_shazam_charts'], errors='coerce').fillna(0).astype(int)

chart_columns = ['in_deezer_charts', 'in_shazam_charts', 'in_spotify_charts', 'in_apple_charts']

artist_chart_counts = spot_df.groupby('artist(s)_name')[chart_columns].sum()

artist_chart_counts['total_chart_appearances'] = artist_chart_counts.sum(axis=1)

top_chart_artists = artist_chart_counts.sort_values(by='total_chart_appearances', ascending=False).head(10)

plt.figure(figsize=(12, 8))
sns.barplot(y=top_chart_artists.index, x=top_chart_artists['total_chart_appearances'], hue=top_chart_artists.index, dodge=False, legend=False)
plt.title('Top 10 Artists by Chart Appearances')
plt.xlabel('Total Appearances in Charts')
plt.ylabel('Artist(s)')
plt.show()
```

Which will display the output below:

![image](https://github.com/user-attachments/assets/9a75cd03-8ffd-4c12-8ca3-24cd011bc17b)


Looking at the most frequent artists, I only noticed 3 artists who are in both graphs, which is Taylor Swift, The Weekend, and Ed Sheeran. However, there are more streams in playlists rather in charts. This means that users tend to listen more streams in the playlists rather than using charts.


### CONCLUSION

In order for a track to be popular, it must have the following according to the results:

1. The track must be released every start of the year.
2. The track depends on how you want to be more upbeat to be danceable, or less valence to be more acoustic for the listeners.
3. Upload the tracks in playlists rather than in charts.
4. Recent tracks makes it more popular. Forgotten tracks are tend to be less popular.
5. Single artists tend to get more famous tracks.
6. Using major mode of tracks gets more famous tracks.


### Final Comments

This project was the biggest thing I have done in my coding experience. If data analysts do this every time, I salute to their hardwork, diligence, and all the efforts they do to create an analysis to help applications to improve for further advancements. It wasn't really easy as expected due to how much coding and comments, but I felt relieved after finishing it all.


### Updates:

  v2.0 - Answered Platform Popularity, Advanced Analysis, and Concluded this Project

  v.1.85 - Answered Temporal Trends and Genre and Music Characteristics

  v1.8 - Answered the Top Performers

  v1.7 - Answered the Basic Descriptive Statistics

  v1.4 - Answered the Overview of the Dataset

  v1.0 - Added the Application Definition,Project Description, General Guidelines, and Guide Questions for the Project
      










