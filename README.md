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
# Display the summary of statistics like mean, median, mode, std, and etc. 
cleaned.describe()
```
<img width="1353" alt="5" src="https://github.com/user-attachments/assets/48a99065-b75a-42e0-85ae-ca01deb58145">

``` python
# Mean, Median, and Standard Deviation of the 'STREAMS' column
meanStreams = cleaned['streams'].mean()  # Calculate the mean of the streams
medianStreams = cleaned['streams'].median()  # Calculate the median of the streams
stdStreams = cleaned['streams'].std()  # Calculate the standard deviation of the streams

print(f"Mean of streams: {meanStreams}") # Print the mean of the streams
print(f"Median of streams: {medianStreams}") # Print the median of the streams
print(f"Standard Deviation of streams: {stdStreams}") # Print the standard deviation of the streams
```
<img width="837" alt="Screenshot 2024-11-03 at 1 25 38 AM" src="https://github.com/user-attachments/assets/d548cace-487e-4477-a9f3-b48aff0d577f">

``` python
# Distribution of 'released_year' and 'artist_count'
plt.figure(figsize=(15, 5))

# Plotting the distribution of released_year
plt.subplot(1, 2, 1)  # 1 row, 2 columns, 1st subplot
plt.hist(df['released_year'], bins=99, color='#FF69B4', edgecolor='black') # Histogram for the released_year
plt.title('Distribution of Released Year') # Add title to the graph
plt.xlabel('Year') # Add x-axis label
plt.ylabel('Number of Tracks') # Add y-axis label

# Plotting the distribution of artist_count
plt.subplot(1, 2, 2) # 1 row, 2 columns, 2nd subplot
plt.hist(df['artist_count'], bins=8, color='#FFC0CB', edgecolor='black') # Histogram for artist_count
plt.title('Distribution of Artist Count') # Add title to the graph
plt.xlabel('Number of Artists') # Add x-axis label
plt.ylabel('Number of Tracks') # Add y-axis label

# Show the plots
plt.tight_layout() # Adjust layout to prevent overlap
plt.show() # Show the plots
```
<img width="1118" alt="Screenshot 2024-11-03 at 1 26 38 AM" src="https://github.com/user-attachments/assets/9afc9d4a-da2c-4bbb-9230-ecee4dee27d5">

``` python
# Function to identify outliers using the IQR (Interquartile Range) method
def identifyOutliersIqr(column):
    q1 = column.quantile(0.25) # First quartile (25th percentile)
    q3 = column.quantile(0.75) # Third quartile (75th percentile)
    iqr = q3 - q1 # Calculate the IQR
    lowerBound = q1 - 1.5 * iqr # Calculate the lower bound for outliers
    upperBound = q3 + 1.5 * iqr # Calcular the upper bound for outliers
    outliers = column[(column < lowerBound) | (column > upperBound)] # Identify values below lowerBound or above upperBound as outliers
    return outliers, len(outliers), q1, q3, iqr # Return outliers and relevant statistics

# Identify outliers for 'released_year'
outliersReleasedYear, totalOutliersYear, q1Year, q3Year, iqrYear = identifyOutliersIqr(cleaned['released_year'])
print(f"\nTotal number of outliers in Released Year: {totalOutliersYear}") # Display the total outliers for released_year

# Identify outliers for 'artist_count'
outliersArtistCount, totalOutliersArtist, q1Artist, q3Artist, iqrArtist = identifyOutliersIqr(cleaned['artist_count'])
print(f"\nTotal number of outliers in Artist Count: {totalOutliersArtist}") # Display total outliers for artist_count
```
<img width="371" alt="Screenshot 2024-11-03 at 1 27 10 AM" src="https://github.com/user-attachments/assets/39619527-e8d4-4202-8cfe-545fa5a4033c">

#### Distribution of Released Year
* The histogram shows a significant peak in tracks released in 2020, indicating a surge in music production during that time compared to previous years.

#### Distribution of Artist Count
* Most tracks are by solo artists rather than groups, suggesting that solo acts dominate the market in this dataset.

## Top Performers

* Which track has the highest number of streams? Display the top 5 most streamed tracks.
* Who are the top 5 most frequent artists based on the number of tracks in the dataset?

``` python
# Track with the highest number of streams and the top 5 most streamed tracks
# Select the 'track_name' and 'streams' columns, then find the top 5 tracks with the highest streams
topStreamedTracks = df[['track_name', 'streams']].nlargest(5, 'streams')

# Print the top 5 most streamed tracks in a pretty and tabular format
print("\nTop 5 Most Streamed Tracks:")
print(tabulate(topStreamedTracks, headers='keys', tablefmt='pretty', showindex=False, stralign='left'))
```
<img width="513" alt="Screenshot 2024-11-03 at 1 35 03 AM" src="https://github.com/user-attachments/assets/3b05fa6f-e4b3-4375-8042-d0eed5a7e825">

``` python
# Top 5 most frequent artists based on the number of tracks in the dataset
# Count the occurrences of each artist in the 'artist(s)_name' column and select the top 5
topArtists = cleaned['artist(s)_name'].value_counts().nlargest(5)
topArtistsCleaned = topArtists.reset_index()  # Convert to DataFrame
topArtistsCleaned.columns = ['artist(s)_name', 'track_count']  # Rename the columns

# Print the top 5 most frequent artists in a pretty and tabular format
print("\nTop 5 Most Frequent Artists:")
print(tabulate(topArtistsCleaned, headers='keys', tablefmt='pretty', showindex=False, stralign='left'))
```
<img width="261" alt="Screenshot 2024-11-03 at 1 35 31 AM" src="https://github.com/user-attachments/assets/a8b6e4cb-6475-418e-aca8-b03a51d739cc">

## Temporal Trends
* Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
* Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

``` python
# Analyzing the number of tracks released over time. 
plt.figure(figsize=(20, 3)) # Set the figure size

# Plotting the distribution of released_year
plt.subplot(1, 2, 1)  # 1 row, 2 columns, 1st subplot
plt.hist(df['released_year'], bins=120, color='#FF69B4', edgecolor='black') # Histogram for number of released per year
plt.title('Number of Tracks Released per Year') # Add title
plt.xlabel('Year') # Add x-axis label
plt.ylabel('Number of Tracks') # Add y-axis label

plt.tight_layout() # Adjust layout to prevent overlap
plt.show() # Show the plots
```
<img width="1019" alt="Screenshot 2024-11-03 at 2 00 36 AM" src="https://github.com/user-attachments/assets/f3e8839f-ff95-49bf-baa6-8622b7683878">

``` python
# Analyze the trends in the number of tracks released over time
plt.figure(figsize=(15, 8)) # Set the figure size 

# Plotting the distribution of tracks released per month
plt.subplot(1, 1, 1)  # 1 row, 1 column, 1st subplot
tracksPerMonth = df['released_month'].value_counts().sort_index()  # Count tracks per month
plt.bar(tracksPerMonth.index, tracksPerMonth.values, color='#FF69B4', edgecolor='black') # Create a bar chart of tracks per month
plt.title('Number of Tracks Released per Month') # Add a title to the graph 
plt.xlabel('Month') # Add x-axis label
plt.ylabel('Number of Tracks') # Add y-axis label

# Set x-tick labels to actual month names
monthNames = ['January', 'February', 'March', 'April', 'May', 'June',
              'July', 'August', 'September', 'October', 'November', 'December']
plt.xticks(ticks=range(1, 13), labels=monthNames, rotation=25)

plt.tight_layout()  # Adjust layout to prevent overlap
plt.show() # Show the plots
```
<img width="1138" alt="Screenshot 2024-11-03 at 2 01 17 AM" src="https://github.com/user-attachments/assets/f4201a30-4d79-45a9-8cc7-22b95065eaff">

### Genre and Music Characteristics

* Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
* Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

``` python
# Specify the columns to analyze for the correlations
columnsToAnalyze = ['streams', 'bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%']

# Calculate the correlation matrix for the specified columns
correlationMatrix = cleaned[columnsToAnalyze].corr()

# Create a custom colormap based on your perference but mine is pink
pink_cmap = LinearSegmentedColormap.from_list(
    'pink_shades', 
    ['#FFFFFF', '#FFB6C1', '#FF69B4', '#FF1493', '#D5006D']  # White, Light Pink, Barbie Pink, Bright Pink, Dark Pink
)

# Create a figure for the heatmap
plt.figure(figsize=(10, 8))  # Set the size of the figure
plt.imshow(correlationMatrix, cmap=pink_cmap, interpolation='nearest')  # Draw the heatmap using our pink colormap
plt.colorbar()  # Show a color scale on the side of the heatmap

# Set the ticks and labels for the axes to show our column names
plt.xticks(np.arange(len(correlationMatrix.columns)), correlationMatrix.columns, rotation=45)  # X-axis labels
plt.yticks(np.arange(len(correlationMatrix.columns)), correlationMatrix.columns)  # Y-axis labels

# Loop through the correlation matrix and add the correlation values to the heatmap
for (i, j), val in np.ndenumerate(correlationMatrix):
    plt.text(j, i, f"{val:.2f}", ha='center', va='center', color='black')  # Display the correlation value in each cell

plt.title('Correlation Heatmap of Musical Attributes')  # Set the title of the heatmap

# Prepare sentences describing each correlation to explain the results
correlation_sentences = []  # Create an empty list to store sentences
for i in range(len(correlationMatrix.columns)):
    for j in range(len(correlationMatrix.columns)):
        if i != j:  # Avoid self-correlation (a variable with itself)
            sentence = f"The correlation between {correlationMatrix.columns[i]} and {correlationMatrix.columns[j]} is {correlationMatrix.iloc[i, j]:.2f}."
            correlation_sentences.append(sentence)  # Add the sentence to the list

# Add the sentences below the heatmap for easy reading
plt.figtext(0.5, -0.05, "\n".join(correlation_sentences), ha='center', va='top', fontsize=10)  # Center the text below the heatmap

plt.tight_layout()  # Adjust the layout to make room for the text
plt.show()  # Show the heat map
```
<img width="457" alt="Screenshot 2024-11-03 at 2 09 11 AM" src="https://github.com/user-attachments/assets/8a4604ef-0543-49da-92f3-fd0ff3e34fd4">

## Platform Popularity

* How do the numbers of tracks in spotify_playlists, deezer_playlist, and apple_playlists compare? Which platform seems to favor the most popular tracks?

``` python
# Create sets of track IDs for each platform
spotifyPlaylists = set(cleaned['in_spotify_playlists'].dropna().unique())
deezerPlaylists = set(cleaned['in_deezer_playlists'].dropna().unique())
applePlaylists = set(cleaned['in_apple_playlists'].dropna().unique())
```

``` python
# Count unique tracks for each platform
uniqueSpotifyPlaylists = len(spotifyPlaylists)
uniqueDeezerPlaylists = len(deezerPlaylists)
uniqueApplePlaylists = len(applePlaylists)
```

``` python
# Prepare data for the pie chart
platform_counts = {
    'Spotify Playlists': uniqueSpotifyPlaylists,
    'Deezer Playlists': uniqueDeezerPlaylists,
    'Apple Music Playlists': uniqueApplePlaylists
}
```

``` python
# Define shades of choice for the pie chart, mine is pink
pink_shades = ['#FFC0CB', '#FF69B4', '#FF1493']  # Light pink, Hot pink, Deep pink

# Create a pie chart
plt.figure(figsize=(8, 8)) # Set the figure size
plt.pie(platform_counts.values(), labels=platform_counts.keys(), autopct='%1.1f%%', startangle=140, colors=pink_shades) # Create the pie chart with percentages
plt.title('Proportion of Unique Tracks on Different Platforms') # Add title to the pie chart
plt.axis('equal')  # Equal aspect ratio ensures the pie chart is circular.
plt.show() # Display the pie chart
```
<img width="650" alt="Screenshot 2024-11-03 at 2 15 54 AM" src="https://github.com/user-attachments/assets/14a07b77-f404-49b3-b302-52386d3010ef">

### Advanced Analysis

* Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
* Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

``` python
# Identify patterns among tracks with the same key or mode
# Group by 'key' and 'mode' and calculate the average streams
key_mode_analysis = df.groupby(['key', 'mode'])['streams'].mean().reset_index() # Calculate average streams
key_mode_analysis = key_mode_analysis.sort_values(by='streams', ascending=False) # Sort results by average streams in descending order

print("Average Streams by Key and Mode:")
print(key_mode_analysis)

# Define 11 distinct shades of choice (mine is pink) for the bars
pink_colors = [
    "#FFB6C1",  # Light Pink
    "#FF69B4",  # Barbie Pink
    "#FF1493",  # Deep Pink
    "#FF007F",  # Vivid Pink
    "#D5006D",  # Slight Dark Pink
    "#C71585",  # Medium Violet Red
    "#DB7093",  # Pale Violet Red
    "#FFC0CB",  # Pink
    "#FF3399",  # Bright Pink
    "#FF66B2",  # Hot Pink
    "#FF4D99"   # Neon Pink
]

# Plotting average streams by key and mode
plt.figure(figsize=(12, 6))
# Loop through each unique key to create a bar for each mode within that key
for idx, key in enumerate(key_mode_analysis['key'].unique()):
    subset = key_mode_analysis[key_mode_analysis['key'] == key] # Get data for the current key
    # Ensure we cycle through pink_colors if there are more keys than colors
    color = pink_colors[idx % len(pink_colors)] # Select a color for the current key
    plt.bar(subset['mode'].astype(str) + f" (Key {key})", subset['streams'], color=color, edgecolor='black', label=f"Key {key}")  # Create a bar for each mode with the corresponding key

plt.title('Average Streams by Key and Mode') # Add title to the graph
plt.ylabel('Average Streams') # Add y-axis label
plt.xlabel('Key and Mode') # Add x-axis label
plt.xticks(rotation=90) # Rotate x-tick labels for better readability
plt.legend(title='Key') # Add a legend to the plot with the title 'Key'
plt.tight_layout() # Adjust layout to prevent overlap of elements
plt.show() #Show the graph 
```
<img width="240" alt="Screenshot 2024-11-03 at 2 22 25 AM" src="https://github.com/user-attachments/assets/3e9852bb-abab-4dfb-91cf-ec7e7666dfff">
<img width="1005" alt="Screenshot 2024-11-03 at 2 22 49 AM" src="https://github.com/user-attachments/assets/94c64700-16bc-4f37-84c1-44ec3db43883">

# References: 

# Author
<img width="289" alt="Screenshot 2024-11-03 at 2 42 26 AM" src="https://github.com/user-attachments/assets/5304be68-5583-47a4-bc1f-2b28ac3ceea7">

## Ginger Fiona R. Ignacio
### 2ECE-B













