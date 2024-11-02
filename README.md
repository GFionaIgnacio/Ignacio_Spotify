# EDA l Spotify Songs 2023
![image](https://github.com/user-attachments/assets/1f182d92-64ab-4025-9d4d-1fd05256a86b)

## Overview
##### This repository provides a Jupyter Notebook performing exploratory data analysis (EDA) on a Spotify dataset of the most streamed songs in 2023. The objective is to uncover patterns, visualize trends, and interpret relationships between different song attributes and their popularity (measured by streams).

## Libraries Used
##### Numpy
##### Pandas
##### Matplotlib

## Dataset
##### The dataset, stored as spotify-2023.csv and sourced from Kaggle, includes detailed information about various songs, such as:
* track_name: Title of the track
* artist(s)_name: Artist(s) name(s)
* artist_count: Number of contributing artists
* released_year: Release year
* released_month: Release month
* released_day: Release day
* in_spotify_playlists: Number of Spotify playlists featuring the track
* in_spotify_charts: Chart position on Spotify
* streams: Number of Spotify streams
* in_apple_playlists: Number of Apple Music playlists featuring the track
* in_apple_charts: Chart position on Apple Music
* in_deezer_playlists: Number of Deezer playlists featuring the track
* in_deezer_charts: Chart position on Deezer
* in_shazam_charts: Chart position on Shazam
* bpm: Beats per minute
* key: Key of the song
* mode: Song mode (Major or Minor)
* danceability_%: Danceability score
* valence_%: Valence score (positivity level)
* energy_%: Energy score
* acousticness_%: Acousticness score
* instrumentalness_%: Instrumentalness score
* liveness_%: Liveness score
* speechiness_%: Speechiness score

## Guide Questions
#### Overview of Dataset
* How many rows and columns does the dataset contain?
* What are the data types of each column? Are there any missing values?

#### Basic Descriptive Statistics
* What are the mean, median, and standard deviation of the streams column?
* What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

#### Top Performers
* Which track has the highest number of streams? Display the top 5 most streamed tracks.
* Who are the top 5 most frequent artists based on the number of tracks in the dataset?

#### Temporal Trends
* Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
* Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

#### Genre and Music Characteristics
* Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
* Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

#### Platform Popularity
* How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

#### Advanced Analysis
* Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
* Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

# Code Implementation and its Output: 
``` python
# Importing necessary libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from tabulate import tabulate
from matplotlib.colors import LinearSegmentedColormap
```

## Overview of Dataset

* How many rows and columns does the dataset contain?
* What are the data types of each column? Are there any missing values?

``` python
# Load the dataset
df = pd.read_csv('spotify-2023.csv', encoding='latin1')
df
```
<img width="967" alt="1" src="https://github.com/user-attachments/assets/860973d1-5d1b-4e0d-a96c-0b6a17583277">

``` python
# Display the first few rows of the dataset using .head()
df.head()
```
<img width="978" alt="2" src="https://github.com/user-attachments/assets/e4cc42b4-c1b3-49eb-b0b1-03e3028a2c9b">

``` python
# Check the size of the dataset
df.size
```
<img width="116" alt="Screenshot 2024-11-02 at 11 16 58 PM" src="https://github.com/user-attachments/assets/b3bf43bf-8f03-4b16-8a0b-e432c4799f15">

``` python
# Check the shape of the dataset
df.shape
```
<img width="165" alt="Screenshot 2024-11-02 at 11 17 41 PM" src="https://github.com/user-attachments/assets/bb0cf5d6-af9a-4b20-a08a-f449ee612f27">

``` python
# Check the data types of the dataset in each column 
df.dtypes
```
<img width="384" alt="Screenshot 2024-11-02 at 11 19 32 PM" src="https://github.com/user-attachments/assets/9ab37272-910d-41e4-9b06-73b5eb549548">

```python
# Specify the columns that needs to be cleaned. Since streams, deezer_playlists, and shazam-_charts are all objects, we have to transform them into float by omitting the comma
columns_to_clean = ['streams', 'in_deezer_playlists', 'in_shazam_charts']

# Loop through each specified column to remove commas
for col in columns_to_clean:
    df[col] = df[col].replace(',', '', regex=True) 
```

``` python
# Count missing values in each column
df.isnull().sum()
```
<img width="346" alt="Screenshot 2024-11-02 at 11 28 33 PM" src="https://github.com/user-attachments/assets/b241fea7-1a37-4207-8d42-91160eb5df38">

``` python
# Clean and convert streams columns into numeric type (float)
# Streams column is still object; hence, we will transform them into float by using the syntax pd.to_numeric. 
df['streams'] = pd.to_numeric(df['streams'].replace(',', '', regex=True), errors='coerce') # Replace commas with empty strings, then convert to float; errors are set to 'coerce' to handle any invalid parsing
```

``` python
# After omitting the commas, the deezer_playlists and shazam_charts are still object. To fully transform it into numeric, we have to use make them into float by using the '.astype'
# Define the columns that needs a comma removal
columns_to_clean = ['in_deezer_playlists', 'in_shazam_charts']

# Remove commas from specified columns
for col in columns_to_clean:
    df[col] = df[col].replace(',', '', regex=True).astype('float')
```

``` python
# Remove duplicates based on track_name and artist(s)_name
df.drop_duplicates(subset=['track_name', 'artist(s)_name'], inplace=True)
df.dropna()
df
```
<img width="969" alt="3" src="https://github.com/user-attachments/assets/33d3c251-a6e3-4183-9bdc-93d3a5b04004">

``` python
# Count missing values in each column
df.isnull().sum()
```
<img width="351" alt="Screenshot 2024-11-02 at 11 38 32 PM" src="https://github.com/user-attachments/assets/24863fd7-7902-461e-9dbb-4ecedbcbe36c">

``` python
# Reset index. So instead of index 0 to 952, we will have 0 to 812. 
df.reset_index(drop=True, inplace=True)

# Remove any unintended columns like 'level_0' and 'index'
for col in ['level_0', 'index']:
    if col in df.columns:
        df.drop(columns=[col], inplace=True)

# Verify the final structure
print("Final column names:", df.columns)
print("\nFinal dataset shape:", df.shape)
df
```
<img width="743" alt="Screenshot 2024-11-02 at 11 48 31 PM" src="https://github.com/user-attachments/assets/c486207a-434d-44b9-9e50-57f6318389cc">
<img width="945" alt="Screenshot 2024-11-02 at 11 53 03 PM" src="https://github.com/user-attachments/assets/3991b688-4eac-40d0-991e-e31e3fe6751a">

``` python
# Assigned the result to new variable, cleaned
cleaned = df.dropna()
cleaned
```

## Basic Descriptive Statistics

* What are the mean, median, and standard deviation of the streams column?
* What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?
  
```python

```











