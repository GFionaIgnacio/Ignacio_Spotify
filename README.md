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
# Load the dataset with a specified encoding
df = pd.read_csv('spotify-2023.csv', encoding='latin1')
df
```
<img width="967" alt="1" src="https://github.com/user-attachments/assets/860973d1-5d1b-4e0d-a96c-0b6a17583277">

``` python
# Display the first few rows of the dataset
df.head()
```
<img width="978" alt="2" src="https://github.com/user-attachments/assets/e4cc42b4-c1b3-49eb-b0b1-03e3028a2c9b">

``` python
df.size
```
<img width="116" alt="Screenshot 2024-11-02 at 11 16 58 PM" src="https://github.com/user-attachments/assets/b3bf43bf-8f03-4b16-8a0b-e432c4799f15">

``` python
df.shape
```
<img width="165" alt="Screenshot 2024-11-02 at 11 17 41 PM" src="https://github.com/user-attachments/assets/bb0cf5d6-af9a-4b20-a08a-f449ee612f27">

``` python
df.dtypes
```
<img width="384" alt="Screenshot 2024-11-02 at 11 19 32 PM" src="https://github.com/user-attachments/assets/9ab37272-910d-41e4-9b06-73b5eb549548">

```python
columns_to_clean = ['streams', 'in_deezer_playlists', 'in_shazam_charts']

# Remove commas from specified columns
for col in columns_to_clean:
    df[col] = df[col].replace(',', '', regex=True)
```

``` python
df.isnull().sum()
```
<img width="346" alt="Screenshot 2024-11-02 at 11 28 33 PM" src="https://github.com/user-attachments/assets/b241fea7-1a37-4207-8d42-91160eb5df38">

``` python
# Remove commas from 'streams' and convert to float using pd.to_numeric
df['streams'] = pd.to_numeric(df['streams'].replace(',', '', regex=True), errors='coerce')
```

``` python
# Assuming df is your DataFrame and columns_to_clean are the columns to remove commas from
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
df.isnull().sum()
```
<img width="351" alt="Screenshot 2024-11-02 at 11 38 32 PM" src="https://github.com/user-attachments/assets/24863fd7-7902-461e-9dbb-4ecedbcbe36c">










