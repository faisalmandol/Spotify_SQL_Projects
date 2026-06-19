# Spotify_SQL_Projects 

![Spotify_Logo](https://github.com/faisalmandol/Spotify_SQL_Projects/blob/main/spotify_logo.png)

# Advance SQL Project - Spotify DataSets
# Schema
```sql
drop table if exists spotify;
create table spotify (
       Artist varchar(46),
	   Track varchar (200),
	   Album varchar (200),
	   Album_type varchar(11),
	   Danceability	float,
	   Energy float,	
	   Loudness	float,
	   Speechiness float,
	   Acousticness float,
	   Instrumentalness float,
	   Liveness	float,
	   Valence float,	
	   Tempo float,
	   Duration_min float,
	   Title varchar(200),
	   Channel varchar(200),
	   Views float,
	   Likes bigint,
	   Comments bigint,
	   Licensed boolean,
	   official_video boolean,
	   Stream bigint,
	   EnergyLiveness float,
	   most_playedon varchar(50)
);
```
# See the Full DataSet 
```sql
select * from spotify;
```
# EDA 
```sql
select count(*)
from spotify;
```
```sql
select count(distinct Artist) 
from spotify;
```
```sql
select count(distinct Album) 
from spotify;
```
```sql
select distinct Album_type 
from spotify;
```
```sql
select max(Duration_min) 
from spotify;
```
```sql
select min(Duration_min)
from spotify;
```
```sql
select * 
from spotify
where Duration_min = 0;
```
```sql
delete from spotify
where Duration_min = 0;
```
```sql
select min(Duration_min)
from spotify;
```

```sql
select distinct channel 
from spotify;
```

# Data Analysis - Easy Category -------


1) Retrieve the names of all tracks that have more than 1 billion streams.
2) List all albums along with their respective artists.
3) Get the total number of comments for tracks where licensed = TRUE.
4) Find all tracks that belong to the album type single.
5) Count the total number of tracks by each artist.


# 1) Retrieve the names of all tracks that have more than 1 billion streams. 
```sql
select track, stream
from spotify
where stream > 1000000000;
```

# 2) List all albums along with their respective artists.

```sql
select distinct album, artist
from spotify
group by album, artist;
```

# 3) Count the total number of comments for tracks where licensed = TRUE.
```sql
select  sum(comments) 
from spotify 
where licensed = True;
```

# 4) Find all tracks that belong to the album type single.
```sql
select track, album_type
from spotify 
where album_type = 'single';
```

# 5) Count the total number of tracks by each artist.
```sql
select  artist, count(track) as tracks
from spotify 
group by artist
order by tracks desc;
```

# Data Analysis -- Medium Category ------------------


1) Calculate the average danceability of tracks in each album.
2) Find the top 5 tracks with the highest energy values.
3) List all tracks along with their views and likes where official_video = TRUE.
4) For each album, calculate the total views of all associated tracks.

# 1) Calculate the average danceability of tracks in each album.
```sql
select track, album, avg(danceability) as avg_danceability
from spotify
group by track, album
order by avg_danceability desc;
```
# 2) Find the top 5 tracks with the highest energy values.
```sql
select track, max(energy)
from spotify
group by track, energy
order by energy desc
limit 5;
```
# 3) List all tracks along with their views and likes where official_video = TRUE.
```sql
select track, 
       sum(views) as total_views,
	   sum(likes) as total_likes
from spotify 
where official_video = 'TRUE'
group by track
order by total_views desc; 
```
# 4) For each album, calculate the total views of all associated tracks.
```sql
select album, sum(views) 
from spotify 
group by album; 
```
# Data Analysis -- Advanced Level --------------

# 1) Find the top 3 most-viewed tracks for each artist using window functions.
```sql
with cte as (

	 select artist, 
	        track,
			views,
			dense_rank() over(partition by artist order by views desc) as rnk
     from spotify			
)

select artist,
       track,
	   views
from cte 
where rnk <= 3;
```
# 2) Write a query to find tracks where the liveness score is above the average.
```sql
select track, 
       liveness
from spotify
where liveness > (

	  select avg(liveness)
	  from spotify
);	   
```
# 3) Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.
```sql
with cte as (

	  select album,
	         max(energy) as highest_energy,
  	         min(energy) as lowest_energy
	  from spotify
	  group by album
)

select album,
       highest_energy - lowest_energy as diff_energy
from cte
order by 2 desc;
```
# 4) Calculate the cumulative sum of likes for tracks ordered by the number of views, using window functions.
```sql
select track, 
       views,
	   likes,
	   sum(likes) over(order by views) as cumulative_likes
from spotify;	
```
# Query Optimization ------------
```sql
explain analyze
select artist, 
       track,
	   views
from spotify
where artist = 'Gorillaz' 
      and 
	  most_playedon = 'Youtube'
order by stream desc 
limit 20

create index artist_index on spotify(artist);
```
(https://github.com/faisalmandol/Spotify_SQL_Projects/blob/main/Explaining%20Index.png)
# End of the Project!
