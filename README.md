## NoSQL Data Modeling in Apache Cassandra

This project illustrates the data modeling NoSQL (Apache Cassandra), that is, by knowing the query first and modeling the data accordingly.

The user event data of the Sparkify music app is given in the events folder which consists of:

artist

firstName of user

gender of user

item number in session

last name of user

length of the song

level (paid or free song)

location of the user

sessionId

song title

userId

This data is then extracted, transformed and loaded into the Apache Cassandra to answer the following questions:

1. Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4

2. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182

3. Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

#### Data Modeling

Query - 1:

Given the SessionId and itemInSession, the query should return the artist, song title and song's length irrespective of userId. 

So, the primary key should be:
    
    PartitionKey: sessionId
    ClusteringKey: itemInSession, userId

This will uniquely identify each song played in the given session and item in session by userId. 

Query - 2:

Given the sessionId and userId, the query should return the artist, song title (sorted by itemInSession) and user name (first name and last name). 

So, the primary key should be:
    
    PartitionKey: userId
    ClusteringKey: sessionId, itemInSession

This will uniquely identify each song played in the given session and item in session by userId. However, the clustering key will sort the songs history of user by sessionId and then by the itemInSession, which satisfies the condition of sort by itemInSession. 

Query - 3:

Given the song_title, the query should return the user name (first name and last name). 

So, the primary key should be:
    
    PartitionKey: song_title
    ClusteringKey: first_name and last_name

This will uniquely identify each song played by user, the clustering key of first name and last name is used. Alternatively, one can also use userId alone which is unique to first name and last name.

##### Note: Even if the song is played multiple times by user, it will only update the already inserted row with the same value 
