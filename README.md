# cyclistic
How does a bike share company navigate speedy success?

1.	Introduction

    a.	Company

    In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime. 
Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members. 
Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, the director of marketing believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, the director of marketing believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs. 
The director of marketing has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. The director of marketing and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.

    b.	Goals

    •	Find differences between annual members and casual riders

    •	Find out why casual riders would buy a membership

    •	Find out how could digital media could affect their marketing tactics

2.	Analysis

    a.	Dataset

    •	Source of data

    Cyclistic historical data in found in: https://divvy-tripdata.s3.amazonaws.com/index.html is stored in .csv format, and data correspond to year 2022. The data has been made available by City of Chicago under this license: https://ride.divvybikes.com/data-license-agreement.

    •	Table

    The data files are arranged by months. Each file contains 13 columns:
    
    ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id, start_lat, start_lng, end_lat, end_lng, member_casual.
    
    In order to store all the data from the different files, a new table called cyclistic will be created in one of MySQL databases. Data is imported to the new table from the 12 .csv files to create a single table.

    •	Explore and clean data

    There are 5,667,717 trips in total and no one of them in duplicated. However, 61 trips in December are dated in 2023 which compared to the total it does not represent an important amount, therefore, those trips are deleted. 
    For 100 trips ending date is minor than starting date which could represent an error in the system or in the way data is collected but, still is not a big number that could affect the result of the analysis, hence those trips are deleted as well.
    Approximately 833,038 trips have no values for columns related to location. That is a representative number that could affect the results of the analysis, so, the company is reached out to find out why this is happening so often. They answer that this happens when a trip does not start nor end at a specific station and they collect data this way to anonymize the trips. Consequently, trips are not deleted since there are no errors in the data. 
    Finally, the column member_casual contains two possible options: member or casual. Each of this words contains 6 characters, however, upon checking the length of each value shows that there are 7 characters. To fix this the whole column is updated to trim the values.

    b.	Differences between riders

    •	Rides

    Data is already clean. Moving on, simple trends are explored.
    
    ![image](https://user-images.githubusercontent.com/118465839/227656150-960e368e-2a43-42bc-91a0-2996619ab51f.png)
    
    ![image](https://user-images.githubusercontent.com/118465839/227656233-b48d29c1-f018-4d22-83ce-fbbe48e89f80.png)
    
    ![image](https://user-images.githubusercontent.com/118465839/227656250-53d44d86-826d-4281-8867-662559a9d121.png)

    From the charts above it notorious that the busiest months are from May to October; Saturday is the busiest day of the week; and 15-19 are the busiest hours, however, in the last chart there are two peaks: the biggest one is from 15-19, but there another one between 7-9.
    Digging a little further some aggregations are performed in the dataset:
 
     ![image](https://user-images.githubusercontent.com/118465839/227656295-53e06515-6362-41cc-9be9-0cbc350d0335.png)

    This chart is indexed to January. It immediately shows a great increment for casual users in July. The chart shows a season with great usage of casual riders, it starts from April to October. February, and December represent the months with lower usage percent variation compared to January. The trend for members is quite different. There is an increase in usage from April to October as well, however, is not a drastic variation. 
    Members have a more stable usage during the year while casual riders have a clear seasonality. 
 
     ![image](https://user-images.githubusercontent.com/118465839/227656398-db002952-9b89-4fb9-8a2a-8752e53c7a29.png)
 
    This chart is a comparison from the previous month. This chart reveals the uptrends and downtrends. 1st and 2nd quarter show an uptrend while 3th and 4th quarter show a downtrend. March has the biggest increase from the previous month. Once more, casual riders have the most drastic changes while members show more conservative changes over the months. 
 
     ![image](https://user-images.githubusercontent.com/118465839/227656429-261848ee-03e0-4c98-a198-efe513a4a58a.png)

    The first part of this graph shows the difference in number of rides between members and casual riders in terms of rides. The second and third part of this graph display the ratio between member and casual rides and the percentage it represents respectively. January, February, November, and December show the greatest difference in ride between casual and member riders. In the other hand, from May to August the difference is minimum as the last part of the graph shows. 
  
      ![image](https://user-images.githubusercontent.com/118465839/227656460-a70f399f-88d3-41bf-bbf9-921f0e03fc43.png)
      
      ![image](https://user-images.githubusercontent.com/118465839/227656526-817f262f-595b-4068-b251-b5d0abea62d0.png)
  
    Two more charts are shown here, however, there are not important insights from them, other than confirming monthly trends and the moving average of rides over the months. 
    A seasonality has been revealed with the previous charts, now is time to dig into daily usage of rides.
 
     ![image](https://user-images.githubusercontent.com/118465839/227656562-03779448-495c-4645-8853-aca6399e9423.png)
     
     ![image](https://user-images.githubusercontent.com/118465839/227656583-1b7ccd01-27e5-4bd1-88a2-4786ef77a5ba.png)

    For casual riders, Sunday and Saturday are the busiest day. For members, week days seem to be more or less stable. 
 
     ![image](https://user-images.githubusercontent.com/118465839/227656613-b5eed0e9-5a8c-4732-860a-2da2a60ab651.png)
     
     ![image](https://user-images.githubusercontent.com/118465839/227656639-1799ecdc-bd0c-4c84-a94d-097d6d9b6526.png)

    An interesting difference when analyzing time of the day is that for members the chart shows two peaks while the chart for casual riders shows only one. 

    •	Stations

    The dataset contains data about locations where the rides started and where they ended. A chart of the top 5 start stations and the top 3 destinations from each one of them is selected. The chart is broken down by member and casual rider as well as rideable type.
 
     ![image](https://user-images.githubusercontent.com/118465839/227656678-63591699-326c-4f23-b6f0-7ea6655e5a4c.png)

    Upon checking the chart there are some noticeable things: one of them is null values for electric_bike, which as is already known is not an actual problem; another one is that members y casual riders do not share any of the most popular starting stations. 
 
     ![image](https://user-images.githubusercontent.com/118465839/227656697-9622e58a-83ea-4f3a-b2c7-bbf5306c5118.png)

    Member use bikes in two main areas while casuals only use bikes in the northern side in the map. 
    Lastly, an average of the distance from where the ride started to where it ended is shown in the last chart of this section. It does not represent the distance of the ride but the distance from started to ended point.
 
    ![image](https://user-images.githubusercontent.com/118465839/227656720-f2d95872-e690-492d-a6b6-f7944c811cdf.png)

    Casual rides, except for electric bikes, have the longest average distance. 

    •	Ride length

    The analysis is also performed by ride length. The first chart shows the general distribution of rides by ride length. Mean, mode, and median are displayed as reference lines. 
 
    ![image](https://user-images.githubusercontent.com/118465839/227656774-d9a6126a-867c-415c-937e-57ed9330508e.png)

    Data was distributed by 12 tiles where the tile 1 has the shortest rides and the 12 tile has the longest ones. As it is shown in the chart, the trend for casual rides is up as the rides grow longer while for member happens the opposite.
 
    ![image](https://user-images.githubusercontent.com/118465839/227656811-3c9b7b87-957f-4c3a-9cd7-6ea58ff800a3.png)

    The last chart for this section shows min and max ride length by top 5 starting stations. Mode, mean, and median are display as reference lines. Members usage is closer to the average while casual rides are way longer than the averages which is consistent with the fact that casual riders have the option of full-day passes, so they want to make the most out of each ride.
 
    ![image](https://user-images.githubusercontent.com/118465839/227656880-88c90c0d-e5f7-41f4-9ba2-46fdfe829da5.png)

    •	Wrap up

    After performing the analysis, we can summarize the insights into the following table which shows the differences between the busiest of each category for members and casual riders: 
    | Casual |	Heading	| Member |
    |--------|----------|--------|
    | Mar-Nov|  Month   | Mar-Nov|
    |Weekend |Day       |	Weekday|
    |15-18	 |Hour      |(peak)	7-9, 17-19|
    |North	 |Stations	|North and South|
    |1.05	   |Distance (km)|	0.68|
    |Long rides|Length	|Short rides|

    c.	Turning casuals into members

    Another goal for this analysis it to find out why a casual rider would buy an annual membership, therefore in this section are shown the similarities between members and casual riders since is more likely for a casual rider sharing the same pattern of rides with a member to buy a membership.

    •	Casual riders using the service in January, February, and December, roughly represent 1% of the yearly rides each month, that means that there are around 55 thousand riders (if each one of them was made by a different user) that can be turned into members since they are more likely living in the city and, therefore, they are not visitors or tourists.

    •	Even though casual use bikes mostly on weekends, there is still a great number of casual riders using the service in week days. 

    •	Member busiest hours are from 7 to 9, and from 17 to 19. Casual riders using the service in those hours share the same pattern with members, so technically it is more likely to convert them into members.

    •	Casual riders using electric bikes share similar patterns with members in distance and average length.

    d.	Digital media

    Digital media could be focused by location. Since the usage between members and casual riders is split in the map, marketing should focus in both areas. The one with majority of member usage should be the main focus for acquiring new customers since they are more likely to become members. And in the northern area the focus should be for casual riders to become members.

3.	Result

    In order to convert casual riders to members, the following strategies are recommended:

    •	Provide single-ride users with a reloadable card to track their usage and spot them to make them the priority niche

    •	Offer a discount in December, January, and February to focus on casual rides that live in the city

    •	Focus advertising in those areas where the usage of the service is more popular
