# Exploratory Data Analysis l Spotify Songs 2023
![image](https://github.com/user-attachments/assets/1f182d92-64ab-4025-9d4d-1fd05256a86b)

## Overview
##### This repository provides a Jupyter Notebook performing exploratory data analysis (EDA) on a Spotify dataset of the most streamed songs in 2023. The objective is to uncover patterns, visualize trends, and interpret relationships between different song attributes and their popularity.

## Libraries Used
##### 1. Numpy: 
###### For numerical operations and calculations, especially for statistics.
``` python
import numpy as np
```

##### 2. Pandas: 
###### For data manipulation and cleaning.
``` python
import pandas as pd
```

##### 3. Matplotlib: 
###### For data visualization, creating charts and plots.
``` python
import matplotlib.pyplot as plt
```

##### 4. Tabulate: 
###### For generating well-structured tables in output.
``` python
from tabulate import tabulate
```

##### 5. Matplotlib.colors: 
###### For handling colors in Matplotlib visualizations.
``` python
import matplotlib.colors as mcolors
```

## Dataset
##### The dataset, stored as spotify-2023.csv and sourced from Kaggle, includes detailed information about various songs, such as:
* **_track_name:_** Title of the track
* **_artist(s)_name:_** Artist(s) name(s)
* **_artist_count:_** Number of contributing artists
* **_released_year:_** Release year
* **_released_month:_** Release month
* _**released_day:**_ Release day
* **_in_spotify_playlists:_** Number of Spotify playlists featuring the track
* **_in_spotify_charts:_** Chart position on Spotify
* **_streams:_** Number of Spotify streams
* **_in_apple_playlists:_** Number of Apple Music playlists featuring the track
* **_in_apple_charts:_** Chart position on Apple Music
* **_in_deezer_playlists:_** Number of Deezer playlists featuring the track
* **_in_deezer_charts:_** Chart position on Deezer
* **_in_shazam_charts:_** Chart position on Shazam
* **_bpm:_** Beats per minute
* **_key:_** Key of the song
* **_mode:_** Song mode (Major or Minor)
* **_danceability_%:_** Danceability score
* **_valence_%:_** Valence score (positivity level)
* **_energy_%:_** Energy score
* **_acousticness_%:_** Acousticness score
* **_instrumentalness_%:_** Instrumentalness score
* **_liveness_%:_** Liveness score
* **_speechiness_%:_** Speechiness score

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
<p align="center">
<img width="967" alt="1" src="https://github.com/user-attachments/assets/860973d1-5d1b-4e0d-a96c-0b6a17583277">
</p>

``` python
# Display the first few rows of the dataset using .head()
df.head()
```
<p align="center">
<img width="978" alt="2" src="https://github.com/user-attachments/assets/e4cc42b4-c1b3-49eb-b0b1-03e3028a2c9b">
</p>

``` python
# Check the size of the dataset
df.size
```
<p align="center">
<img width="116" alt="Screenshot 2024-11-02 at 11 16 58 PM" src="https://github.com/user-attachments/assets/b3bf43bf-8f03-4b16-8a0b-e432c4799f15">
</p>

``` python
# Check the shape of the dataset
df.shape
```
<p align="center">
<img width="165" alt="Screenshot 2024-11-02 at 11 17 41 PM" src="https://github.com/user-attachments/assets/bb0cf5d6-af9a-4b20-a08a-f449ee612f27">
</p>


``` python
# Check the data types of the dataset in each column 
df.dtypes
```
<p align="center">
<img width="384" alt="Screenshot 2024-11-02 at 11 19 32 PM" src="https://github.com/user-attachments/assets/9ab37272-910d-41e4-9b06-73b5eb549548">
</p>

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
<p align="center">
    <img width="346" alt="Screenshot 2024-11-02 at 11 28 33 PM" src="https://github.com/user-attachments/assets/b241fea7-1a37-4207-8d42-91160eb5df38">
</p>

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
<p align="center">
<img width="969" alt="3" src="https://github.com/user-attachments/assets/33d3c251-a6e3-4183-9bdc-93d3a5b04004">
</p>

``` python
# Count missing values in each column
df.isnull().sum()
```
<p align="center">
<img width="351" alt="Screenshot 2024-11-02 at 11 38 32 PM" src="https://github.com/user-attachments/assets/24863fd7-7902-461e-9dbb-4ecedbcbe36c">
</p>

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
<p align="center">
<img width="743" alt="Screenshot 2024-11-02 at 11 48 31 PM" src="https://github.com/user-attachments/assets/c486207a-434d-44b9-9e50-57f6318389cc">
<img width="945" alt="Screenshot 2024-11-02 at 11 53 03 PM" src="https://github.com/user-attachments/assets/3991b688-4eac-40d0-991e-e31e3fe6751a">
</p>

``` python
# Assigned the result to new variable, cleaned
cleaned = df.dropna()
cleaned
```
#### Dataset Dimensions
``` Initially, the dataset had 953 rows and 24 columns. After cleaning, it now contains 813 rows and 24 columns. ```
#### Column Data Types and Missing Values
```Before cleaning, most columns were int64, but streams, in_deezer_playlists, and in_shazam_charts were object data types. These were transformed into float64 for accurate analysis.```

## Basic Descriptive Statistics

* What are the mean, median, and standard deviation of the streams column?
* What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?
  
```python
# Display the summary of statistics like mean, median, mode, std, and etc. 
cleaned.describe()
```
<p align="center">
<img width="1353" alt="5" src="https://github.com/user-attachments/assets/48a99065-b75a-42e0-85ae-ca01deb58145">
</p>

``` python
# Mean, Median, and Standard Deviation of the 'STREAMS' column
meanStreams = cleaned['streams'].mean()  # Calculate the mean of the streams
medianStreams = cleaned['streams'].median()  # Calculate the median of the streams
stdStreams = cleaned['streams'].std()  # Calculate the standard deviation of the streams

print(f"Mean of streams: {meanStreams}") # Print the mean of the streams
print(f"Median of streams: {medianStreams}") # Print the median of the streams
print(f"Standard Deviation of streams: {stdStreams}") # Print the standard deviation of the streams
```
<p align="center">
<img width="837" alt="Screenshot 2024-11-03 at 1 25 38 AM" src="https://github.com/user-attachments/assets/d548cace-487e-4477-a9f3-b48aff0d577f">
</p>

``` python
# Distribution of 'released_year' and 'artist_count'
plt.figure(figsize=(15, 5)) # Set the figure size

# Plotting the distribution of released_year
plt.subplot(1, 2, 1)  # 1 row, 2 columns, 1st subplot
plt.hist(cleaned['released_year'], bins=99, color='#FF69B4', edgecolor='black') # Histogram for the released_year
plt.title('Distribution of Released Year') # Add title to the graph
plt.xlabel('Year') # Add x-axis label
plt.ylabel('Number of Tracks') # Add y-axis label

# Plotting the distribution of artist_count
plt.subplot(1, 2, 2) # 1 row, 2 columns, 2nd subplot
plt.hist(cleaned['artist_count'], bins=8, color='#FFC0CB', edgecolor='black') # Histogram for artist_count
plt.title('Distribution of Artist Count') # Add title to the graph
plt.xlabel('Number of Artists') # Add x-axis label
plt.ylabel('Number of Tracks') # Add y-axis label

# Show the plots
plt.tight_layout() # Adjust layout to prevent overlap
plt.show() # Show the plots
```
<p align="center">
    <img width="1118" alt="Screenshot 2024-11-03 at 1 26 38 AM" src="https://github.com/user-attachments/assets/9afc9d4a-da2c-4bbb-9230-ecee4dee27d5">
</p>

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
<p align="center">
    <img width="371" alt="Screenshot 2024-11-03 at 1 27 10 AM" src="https://github.com/user-attachments/assets/39619527-e8d4-4202-8cfe-545fa5a4033c">
</p>

#### Mean, Median, and Standard Deviation of Streams
```
Mean: 468922407.2521525
Median: 263453310
Standard Deviation: 523981505.32150424
```
#### Distribution of Released Year
``` The histogram shows a significant peak in tracks released in 2020 onwards, indicating a surge in music production during that time compared to previous years. ```

#### Distribution of Artist Count
``` Given that the dataset reflects streams from 2023, it’s expected that the majority of tracks were released post-2020. Additionally, tracks featuring a single artist are more prevalent, likely due to the significant representation of indie artists in the data. The analysis identified 180 outliers in the released_year column and 24 in the artist_count column.```

## Top Performers

* Which track has the highest number of streams? Display the top 5 most streamed tracks.
* Who are the top 5 most frequent artists based on the number of tracks in the dataset?

``` python
# Select the 'track_name' and 'streams' columns, then find the top 5 tracks with the highest streams
topStreamedTracks = cleaned.nlargest(5, 'streams')

# Sort the top tracks in ascending order for correct plotting
topStreamedTracks = topStreamedTracks.sort_values(by='streams', ascending=True)

# Create a horizontal bar graph
plt.figure(figsize=(10, 6))  # Set the figure size
plt.barh(topStreamedTracks['track_name'], topStreamedTracks['streams'], color='#FF69B4', edgecolor='black')  # Horizontal bar graph

# Add titles and labels
plt.title('Top 5 Most Streamed Tracks')  # Add title
plt.xlabel('Number of Streams')  # Add x-axis label
plt.ylabel('Tracks')  # Add y-axis label
plt.tight_layout()  # Adjust layout to prevent overlap of elements
plt.show()  # Display the plot
```
<p align="center">
<img width="834" alt="Screenshot 2024-11-03 at 7 10 53 PM" src="https://github.com/user-attachments/assets/26c8ccfe-f3a1-4b1b-812b-c23c92cab612">
</p>

``` python
# Printed top 5 most streamed tracks
# Select the 'track_name' and 'streams' columns, then find the top 5 tracks with the highest streams
tableTopStreamedTracks = cleaned[['track_name', 'streams']].nlargest(5, 'streams')

# Print the top 5 most streamed tracks in a pretty and tabular format
print("\nTop 5 Most Streamed Tracks:")
print(tabulate(tableTopStreamedTracks, headers='keys', tablefmt='pretty', showindex=False, stralign='left'))
```
<p align="center">
<img width="416" alt="Screenshot 2024-11-03 at 7 11 36 PM" src="https://github.com/user-attachments/assets/0af84e33-c7bf-4745-a506-98194a57ed73">
</p>

#### Highest Number of Streams
``` The track with the highest number of streams is “Shape of You,” which has an impressive 3,562,543,890 streams. It is followed by “Sunflower - Spider-Man: Into the Spider-Verse” with 2,808,096,550 streams, and “One Dance” with 2,713,922,350 streams. The complete list of the top five most streamed tracks includes “STAY (with Justin Bieber)” and “Believer,” with 2,665,343,922 and 2,594,040,133 streams, respectively. ```

``` python
# Group by artist name and count the number of tracks
artistTrackCount = cleaned['artist(s)_name'].value_counts().reset_index()

# Rename the columns for clarity
artistTrackCount.columns = ['artist(s)_name', 'track_count']

# Select the top 5 artists with the most tracks
topArtists = artistTrackCount.nlargest(5, 'track_count')

# Sort the top artists in descending order (most tracks at the top)
topArtists = topArtists.sort_values(by='track_count', ascending=True)  # Sort in ascending order for bar chart

# Create a bar graph for the top 5 artists
plt.figure(figsize=(10, 6))  # Set the figure size
plt.barh(topArtists['artist(s)_name'], topArtists['track_count'], color='#FF69B4', edgecolor='black')  # Horizontal bar graph

# Add titles and labels
plt.title('Top 5 Most Frequent Artists Based on Number of Tracks')  # Add title
plt.xlabel('Number of Tracks')  # Add x-axis label
plt.ylabel('Artists')  # Add y-axis label
plt.tight_layout()  # Adjust layout to prevent overlap of elements
plt.show()  # Display the plot
```
<p align="center">
<img width="789" alt="Screenshot 2024-11-03 at 10 34 12 PM" src="https://github.com/user-attachments/assets/196b1ace-6bfe-429e-b40e-18047f3804f0">
</p>


``` python
# Printed top 5 most frequent artists based on the number of tracks in the dataset
# Count the occurrences of each artist in the 'artist(s)_name' column and select the top 5
topArtists = cleaned['artist(s)_name'].value_counts().nlargest(5)
topArtistsCleaned = topArtists.reset_index()  # Convert to DataFrame
topArtistsCleaned.columns = ['artist(s)_name', 'track_count']  # Rename the columns

# Print the top 5 most frequent artists in a pretty and tabular format
print("\nTop 5 Most Frequent Artists:")
print(tabulate(topArtistsCleaned, headers='keys', tablefmt='pretty', showindex=False, stralign='left'))
```
<p align="center">
<img width="210" alt="Screenshot 2024-11-03 at 10 34 47 PM" src="https://github.com/user-attachments/assets/b0453a02-4b26-48cf-9230-29f5d28037b8">
</p>

#### Top 5 most frequent artists
``` Taylor Swift leads with 29 tracks, followed by SZA with 17 tracks and Bad Bunny with 16 tracks. The Weeknd has 14 tracks, while Harry Styles rounds out the top five with 12 tracks. These artists are the most represented in the dataset based on the number of their tracks. ```

## Temporal Trends
* Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
* Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

``` python
# Analyzing the number of tracks released over time. 
plt.figure(figsize=(20, 3)) # Set the figure size

# Plotting the distribution of released_year
plt.subplot(1, 2, 1)  # 1 row, 2 columns, 1st subplot
plt.hist(cleaned['released_year'], bins=120, color='#FF69B4', edgecolor='black') # Histogram for number of released per year
plt.title('Number of Tracks Released per Year') # Add title
plt.xlabel('Year') # Add x-axis label
plt.ylabel('Number of Tracks') # Add y-axis label

plt.tight_layout() # Adjust layout to prevent overlap
plt.show() # Show the plots
```
<p align="center">
<img width="1019" alt="Screenshot 2024-11-03 at 2 00 36 AM" src="https://github.com/user-attachments/assets/f3e8839f-ff95-49bf-baa6-8622b7683878">
</p>

#### Tracks Released Over Time (by Year)
``` The histogram shows the number of track releases per year, with a steady increase over time. This trend indicates a rise in music production, reaching a significant peak in 2023.```

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
<p align="center">
<img width="904" alt="Screenshot 2024-11-04 at 12 41 47 AM" src="https://github.com/user-attachments/assets/007b6283-ec92-4835-b6d2-3b9352d6ba3a">
</p>

#### Tracks Released by Month

```The bar chart reveals monthly trends, with May having the highest number of releases, followed by January. These months show noticeably more frequent releases compared to others, suggesting a potential preference for new releases at the start of the year.```

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
<p align="center">
<img width="457" alt="Screenshot 2024-11-03 at 2 09 11 AM" src="https://github.com/user-attachments/assets/8a4604ef-0543-49da-92f3-fd0ff3e34fd4">
</p>

#### Streams and Musical Attributes 
``` There’s little to no relationship between streams and attributes like BPM, danceability_%, energy_%, valence_%, and acousticness_%. Among them, danceability_% has the highest, but still weak, influence.```
#### Attribute Correlations
```Danceability_% and Energy_% have a weak positive correlation (0.16). ```
``` Danceability_% and Valence_% are somewhat related (0.39), meaning more danceable tracks tend to be more positive. ```
``` Energy_% and Acousticness_% have a moderate negative correlation (-0.55), showing that energetic tracks are less acoustic. ```

## Platform Popularity
* How do the numbers of tracks in spotify_playlists, deezer_playlist, and apple_playlists compare? Which platform seems to favor the most popular tracks?

``` python
# Count total tracks for each platform
totalSpotifyTracks = cleaned['in_spotify_playlists'].dropna().sum()  # Sum all tracks in Spotify playlists
totalDeezerTracks = cleaned['in_deezer_playlists'].dropna().sum()    # Sum all tracks in Deezer playlists
totalAppleTracks = cleaned['in_apple_playlists'].dropna().sum()      # Sum all tracks in Apple playlists
```

``` python
# Prepare data for the pie chart
platform_counts = {
    'Spotify Playlists': totalSpotifyTracks, # Total tracks for Spotify
    'Deezer Playlists': totalDeezerTracks, # Total tracks for Deezer
    'Apple Music Playlists': totalAppleTracks # Total tracks for Apple
}
```

``` python
# Define shades of choice for the pie chart, mine is pink 
pink_shades = ['#FFC0CB', '#FF69B4', '#FF1493']  # Light pink, Hot pink, Deep pink
```

``` python
# Create a pie chart
plt.figure(figsize=(8, 8))  # Set the figure size
plt.pie(platform_counts.values(), labels=platform_counts.keys(), autopct='%1.1f%%', startangle=140, colors=pink_shades)  # Create the pie chart with percentages
plt.title('Proportion of Total Tracks on Different Platforms')  # Add title to the pie chart
plt.legend() # Add legend
plt.axis('equal')  # Equal aspect ratio. This will ensure the pie chart is circular.
plt.show()  # Show the pie chart
```
<p align="center">
<img width="595" alt="Screenshot 2024-11-07 at 2 19 48 PM" src="https://github.com/user-attachments/assets/ed09054e-e490-4456-8337-a100fa5ee0fa">
</p>

#### Comparison of Track Numbers in Playlists
``` The pie chart shows that Spotify playlists have the most tracks, indicating that it favors popular music more than Deezer and Apple Music. ```

``` python
# Check the topStreamedTracks by running it again just to be sure that 'in_spotify_playlist', 'in_deezer_playlist', and 'in_apple_playlist' are all present
topStreamedTracks
```

``` python
# Count total tracks for each platform
topTotalSpotifyTracks = topStreamedTracks['in_spotify_playlists'].sum()  # Sum all tracks in Spotify playlists
topTotalDeezerTracks = topStreamedTracks['in_deezer_playlists'].sum()    # Sum all tracks in Deezer playlists
topTotalAppleTracks = topStreamedTracks['in_apple_playlists'].sum()      # Sum all tracks in Apple playlists

# Prepare data for the pie chart
platform_counts = {
    'Spotify Playlists': topTotalSpotifyTracks, # Total tracks for Spotify
    'Deezer Playlists': topTotalDeezerTracks, # Total tracks for Deezer
    'Apple Music Playlists': topTotalAppleTracks # Total tracks for Apple
}

# Define shades of choice for the pie chart, mine is pink 
pink_shades = ['#FFC0CB', '#FF69B4', '#FF1493']  # Light pink, Hot pink, Deep pink

# Create a pie chart
plt.figure(figsize=(8, 8))  # Set the figure size
plt.pie(platform_counts.values(), labels=platform_counts.keys(), autopct='%1.1f%%', startangle=140, colors=pink_shades, explode = [0.1, 0, 0], wedgeprops = {'edgecolor':'black', 'linewidth' : 0.5})  # Create the pie chart with percentages
plt.title('Proportion of Total Tracks on Different Platforms')  # Add title to the pie chart
plt.legend() # Add legend
plt.axis('equal')  # Equal aspect ratio. This will ensure the pie chart is circular.
plt.show()  # Show the pie chart
```
<p align="center">
<img width="641" alt="Screenshot 2024-11-05 at 2 47 55 AM" src="https://github.com/user-attachments/assets/187fa347-d2e4-47a2-b63b-26f75a0dd847">
</p>

#### Platform Popularity
``` Spotify is the leading platform for popular tracks, followed by Deezer and Apple Music. ```

### Advanced Analysis

* Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
* Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

``` python
# Group by 'key' and 'mode' and calculate the average streams
key_mode_analysis = cleaned.groupby(['key', 'mode'])['streams'].sum().reset_index() 

# Filter for Major and Minor modes
key_mode_analysis = key_mode_analysis[key_mode_analysis['mode'].isin(['Major', 'Minor'])]

# Calculate average streams for Major and Minor for each key
avg_streams = key_mode_analysis.groupby(['key', 'mode'])['streams'].sum().unstack().fillna(0)

# Set the colors for Major and Minor
colors = ['#FFB6C1', '#FF69B4']  # Light Pink for Major, Barbie Pink for Minor

# Plotting average streams by key and mode
plt.figure(figsize=(12, 6)) # Figure Size
avg_streams.plot(kind='bar', color=colors, edgecolor='black') # Create a bar plot

plt.title('Average Streams by Key and Mode') # Add title
plt.ylabel('Average Streams') # Add y-axis label
plt.xlabel('Key') # Add x-axis label
plt.xticks(rotation=0)  # Set x-tick labels to horizontal
plt.legend(title='Mode')  # Add a legend for Major and Minor
plt.tight_layout()  # Adjust layout to prevent overlap of elements
plt.show()  # Show the graph
```
<p align="center">
    <img width="489" alt="Screenshot 2024-11-07 at 2 24 19 PM" src="https://github.com/user-attachments/assets/39a2a90e-515f-4af8-944a-4c675e47dc16">
</p>

#### Patterns in Streams by Key and Mode
``` Tracks in the key of C# have the highest total streams for both Major and Minor modes. This suggests that either there are more tracks in C# or that tracks in this key are particularly appealing to listeners, resulting in higher streaming numbers. ```

``` python
# Calculate total playlists and charts, using .loc[] to avoid SettingWithCopyWarning
cleaned.loc[:, 'total_playlists'] = (cleaned['in_spotify_playlists'].fillna(0) + # Fill NaN values with 0 and sum Spotify playlists
                                      cleaned['in_apple_playlists'].fillna(0) + # Fill NaN values with 0 and sum Apple playlists
                                      cleaned['in_deezer_playlists'].fillna(0)) # Fill NaN values with 0 and sum Deezer playlists

cleaned.loc[:, 'total_charts'] = (cleaned['in_spotify_charts'].fillna(0) + # Fill NaN values with 0 and sum Spotify charts
                                   cleaned['in_apple_charts'].fillna(0) + # Fill NaN values with 0 and sum Apple charts
                                   cleaned['in_deezer_charts'].fillna(0) + # Fill NaN values with 0 and sum Deezer charts
                                   cleaned['in_shazam_charts'].fillna(0)) # Fill NaN values with 0 and sum Shazam charts
```

``` python
# Calculate the top 10 artists 
topArtists = cleaned['artist(s)_name'].value_counts().nlargest(10)  # Get the top 10 artists 
topArtistsCleaned = topArtists.reset_index()  # Convert to DataFrame
topArtistsCleaned.columns = ['artist(s)_name', 'track_count']  # Rename the columns

# Filter the dataset for the top 10 artists based on appearance counts
topArtistsList = topArtistsCleaned['artist(s)_name']  # List of top 10 artist names
topArtistsData = cleaned[cleaned['artist(s)_name'].isin(topArtistsList)]  # Filter the DataFrame
```

``` python
# Calculate total playlist and chart 
artistComparison = topArtistsData.groupby('artist(s)_name').agg(
    total_playlists=('total_playlists', 'sum'),  # Summation of playlists 
    total_charts=('total_charts', 'sum')  # Summation of charts
).reset_index()  # Reset the index 

# Add a column to sum playlist and chart 
artistComparison['total_appearances'] = artistComparison['total_playlists'] + artistComparison['total_charts']

# Sort the DataFrame by total appearances (descending order, that is why ascending=False)
artistComparison = artistComparison.sort_values(by='total_appearances', ascending=False)
```

``` python
# Plotting the comparison of playlists and charts
plt.figure(figsize=(13, 6))  # Set the figure size

# Set the bar width and positions
bar_width = 0.36  # Set the width depending on your preferences
index = np.arange(len(artistComparison))  # Position of artist

# Plot total playlist appearances
plt.bar(index, artistComparison['total_playlists'], bar_width, label='Playlists', color='#FF69B4', edgecolor='black')

# Plot total chart appearances, with an offset to appear beside playlists
plt.bar(index + bar_width, artistComparison['total_charts'], bar_width, label='Charts', color='#FFB6C1', edgecolor='black')

# Adding titles and labels
plt.title('Comparison of the Most Frequently Appearing Artists in Playlists vs Charts.')  # Add title 
plt.xlabel('Artists')  # Add x-axis label
plt.ylabel('Count')  # Add y-axis label
plt.xticks(index + bar_width / 2, artistComparison['artist(s)_name'], rotation=45)  # Set artist names as x-tick labels
plt.legend(title='Playlist vs Chart')  # Add Legend

plt.tight_layout()  # Adjust layout
plt.show()  # Show the graph
```
<p align="center">
    <img width="648" alt="Screenshot 2024-11-07 at 2 26 58 PM" src="https://github.com/user-attachments/assets/05e8f347-e21f-4fad-83c9-42da1fb29b07">
</p>

#### Artist and Genre Analysis
``` The analysis reveals that the top 10 artists tend to have significantly more appearances in playlists compared to charts.```

# Insights and Recommendations
#### Loading the CSV File: 
```As seen, I use encoding='latin1' in my code when loading the CSV file. Without it, the file cannot be read due to non-ASCII characters not being supported by the default encoding. To resolve this issue, options like encoding='latin1', encoding='cp1252', or 'encoding='iso-8859-1' can be used. When working with datasets that may contain special characters, it’s important to specify the correct encoding to prevent loading issues.```

#### Data Cleaning: 
```By checking df.dtypes, we can see that in_deezer_playlist, in_shazam_charts, and streams are treated as objects, despite appearing as integers in the CSV file. To fix this, we need to convert them into floats. For streams, we can use pd.to_numeric() and remove any commas. For in_deezer_playlist and in_shazam_charts, we should also remove the commas and convert them to floats. These steps will ensure the data is in the correct format for accurate analysis. Additionally, to remove any potential duplicates in track_name and artist_name, we can use .drop_duplicates(), which will provide a more accurate result.```

#### Track Popularity and Streaming Patterns
###### Top Streamed Tracks
``` In the top 5 most streamed tracks, we can identify which songs have garnered the highest engagement, which is “Shape of You” by Ed Sheeran,  reaffirming its long-standing popularity across platforms.```

###### Stream Distribution
```The mean, median, and standard deviation of streams highlight the central tendency and variability in streaming numbers. The standard deviation of streams is significantly high, which is 523,981,505, indicating that a few tracks dominate the streaming numbers, while others perform relatively lower.```

###### Outliers in Tracks
```There are 180 outliers in the released year, indicating that some tracks have significantly higher stream counts compared to others released in the same year. Some of the possible causes for these are the (1) holidays such as Christmas, (2) another reason if the artist is a well-known artist like Taylor Swift, (3) lastly if it got viral, it can be via social media like Tiktok, making an outlier.```

#### Artist Trends and Popularity
###### Top Artists
```Taylor Swift is the most frequent artist in the dataset, showcasing a strong presence in the Spotify 2023. Her songs consistently perform well, showing that artist reputation plays a key role in streaming success.```
###### Solo Artists
```Based on the dataset, it can be seen that solo artists are the most dominant, this suggests that the crowd likes listening to the individual performers rather than in a band. Actually, I even prefer listening to solo artists like Harry Styles, The Weeknd, and Niki. ```

#### Time Trends in Track Releases
###### Tracks Released Over Time
```The graph shows a significant increase in track releases in 2022, indicating a surge in post-pandemic demand.```
###### Monthly Track Trends
```May stands out as the peak month for releases, likely due to the approach of summer, while January follows as listeners seek fresh starts in the new year. Recognizing these patterns helps track industry activity and suggests intentional timing to align with high listener engagement.```

#### Correlation of Musical Attributes
```The heat map illustrates key correlations: high danceability and energy are linked with popular club tracks, positive valence (mood) correlates with track popularity, and BPM aligns with listener-preferred tempos. This analysis of correlations provides insights into trends associated with successful tracks. Hence, analyzing the correlation is indeed crucial as these can help identify trends in musical features that are associated with more successful tracks in the future.```

#### Platform Preferences
```As seen in the pie chart, Spotify leads in platform popularity, followed by Deezer and Apple Music. This trend holds for both the total number of tracks across platforms and the comparison of track numbers in playlists. Since Spotify dominates the most, it suggests there are larger users using this app and artists may prioritize releasing music and promotions in Spotify.```

#### Mode and Key Analysis
```In nearly all of the keys, the major dominates, with the minor scale only dominating twice (key B and key C#). major keys may have a more upbeat, positive sound, while tracks in minor keys might have a melancholic or serious tone. Hence, this suggests that major keys are more likely to appeal to a wider audience as it gives a lively and positive track to the public.```

#### Genre and Music Characteristics Section
```The generated correlations from the heatmap are negative, which may suggest an inverse relationship between music characteristics and stream counts. Since song streams can’t be negative, the negative correlations might be misleading or artifacts of how the correlation matrix was computed. To resolve this, consider setting negative values to zero or treating them as neutral. This makes the heatmap more meaningful. If negative correlations persist, normalizing the data first can help by scaling values consistently and reducing the impact of outliers.```

#### Music Producers and Artists
```For Music Producers and Artists, I recommend focusing on high-energy, danceable tracks with a positive mood, as these are commonly seen in successful tracks. Additionally, pay attention to release timing and the months when the music industry is most active—releasing new music during these times could help increase track visibility.```

#### Streaming Platforms
```For Streaming Platforms, it is important to track trends between streams and musical attributes like tempo and mood. These insights can be used to recommend similar songs or create curated playlists. Also, given the platform distribution, ensure popular tracks are available across multiple platforms for wider reach.```

#### Music Analysts and Marketers
```For Music Analysts and Marketers, analyzing trends in release years and months to better understand the broader music market and identify peak times for new releases. You can also conduct targeted marketing based on which platforms are most popular for specific tracks and artists.```


# References 
* Camilleri, P., & McKinney, T. (2015, October 22). Plotting a 2D heatmap. Stack Overflow. https://stackoverflow.com/questions/33282368/plotting-a-2d-heatmap
* CodersLegacy. (2023, August 8). MatPlotLib Colormap Tutorial (LinearSegmentedColormap) [Video]. YouTube. https://www.youtube.com/watch?v=k6_LetPGGMQ
* Coding Informer. (2023, May 6). How to create heatmaps using Matplotlib and Pandas [Video]. YouTube. https://www.youtube.com/watch?v=mflRx2m_-Bs
* How to create a linear colormap with color defined at specific values with matplotlib? (n.d.). Stack Overflow. https://stackoverflow.com/questions/74731282/how-to-create-a-linear-colormap-with-color-defined-at-specific-values-with-matpl
* Ishaan Sharma. (2020, September 11). python matplotlib graphs using csv files, bar, pie, line graph [Video]. YouTube. https://www.youtube.com/watch?v=spALaS5BFX8
* Moo, M. (2016, May 26). Encoding Error in Panda read_csv. Stack Overflow. https://stackoverflow.com/questions/30462807/encoding-error-in-panda-read-csv
* Peres, G. (2022, July 22). Python Pandas dataframe find missing values. Stack Overflow. https://stackoverflow.com/questions/59694988/python-pandas-dataframe-find-missing-values
* Pluta, J. (2021, May 15). How to display pretty tables in terminal with tabulate python package? Stack Overflow. https://stackoverflow.com/questions/67548514/how-to-display-pretty-tables-in-terminal-with-tabulate-python-package
* python pandas groupby() result. (2015, December 30). Stack Overflow. https://stackoverflow.com/questions/17666075/python-pandas-groupby-result
* Shah, N. (2016, February 20). How do I get the row count of a Pandas DataFrame? (P. Mortensen, Ed.). Stack Overflow. https://stackoverflow.com/questions/15943769/how-do-i-get-the-row-count-of-a-pandas-dataframe
* The AI & DS Channel. (2021b, September 21). Bar Chart | Bar Graph using python | Bar chart tutorial [Video]. YouTube. https://www.youtube.com/watch?v=9VK8quGFcSE 

<h1 align="center">
    Author
</h1>

<p align="center">
    <img width="289" alt="Screenshot 2024-11-03 at 2 42 26 AM" src="https://github.com/user-attachments/assets/2709523d-81a7-4d41-bb3f-94a4c0fa7372">
</p>

<h2 align="center">
    Ginger Fiona R. Ignacio
</h2>

<h3 align="center">
    2ECE-B
</h3>













