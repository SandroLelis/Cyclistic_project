# Cyclistic_project                 

![cyclistic_logo](https://user-images.githubusercontent.com/115048292/201795224-d4076c36-ab15-449a-bad2-7e517ec421d2.jpg)



Since 2016, Cyclistic is a sucessful bike-sharing business. They have been provided three diferent pricing plans:

1. Single ride passes;
2. full day passes;
3. annual passes.

Customers who purchase single-ride or full-day passes are referred to as _**casual riders**_. Customers
who purchase annual memberships are _**Cyclistic members**_.

Cyclistic have founded that annual memberships are profitable than casual rides. By analysing how this customers use bikes deferently it's needed, to get insights and clarify how to convert casual riders into annual members, how they can use social media to promote their campaigns and why would casual riders change for a membership.

Cyclistic Marketing team as set a clear goal: Design marketing strategies aimed at converting casual riders into anual members. The marketing analsyt team needs to better understand how annual members and casual riders differ, and how digital media could affect their marketing tatics.

### Data sources used
This data is a Cyclistic historical bike trip data.

This dataset was collected under licence agreement by divvybikes.com/data where divvy operates the city of Chicago's.

#### Download:  [Project_track1_tripdata.csv](https://github.com/SandroLelis/Cyclistic_project/files/10008024/Project_track1_tripdata.csv)


### Documentation cleaning and manipulation of data.

The chosen tool for clean and manipulate this data was SQL using Bigquery with simple querys and spreadsheets. This data set contains 84704 rowns.


**1. Count diferent Member and Casual types customers.**

 _Number of casual is 23600. Number of members is 61109._

`select distinct count(member_casual)
from `voltaic-mantra-364014.Cyclistic_project.customers`
where member_casual = "member"`

**2. Calculate time difference during each travel. Then averange the time travel in minutes.**

_Averange  is 73 minutes for casual clients, and 21 minutes for members clients._
![Averange_duration_in_minutes_customer_type_for_all_travels](https://user-images.githubusercontent.com/115048292/208265230-7d849782-1271-4d60-981e-2066a61f5391.png)


#### Pivot (Sum)
![Image](https://user-images.githubusercontent.com/115048292/201708846-5d982aeb-2049-4cf2-9812-663854ea7bfe.png)


`select avg(duration_minutes) from
(
  select started_at,ended_at,
  datetime_diff(ended_at,started_at, minute) as duration_minutes
  from `voltaic-mantra-364014.Cyclistic_project.customers`
  where member_casual = "member"
) avg_duration`

**3. Calculate distance in meters using coordinates between different types of clients, then averange distance.**
 
_Averange distance in meters for casual clients was 3014.95 meters, while members was 3015.67 meters._

`select distinct avg(distance_in_meters) from
(
SELECT distinct member_casual,
    st_distance(st_geogpoint(start_lng, start_lat), st_geogpoint(end_lng,end_lat)) as distance_in_meters,
    from `voltaic-mantra-364014.Cyclistic_project.customers`)
    
) as avg_distance`

**4. Count the station more often used by both customers types:**

- _Casual clients used 246 times clark st & Elm St, while members use 604 times. clark st & Elm St was most used start station for both client types._ 




![Count_start_station_per_client](https://user-images.githubusercontent.com/115048292/208265095-95432b36-e3b9-434d-9daa-1ede37defa8f.png)


`select start_station_name, count(start_station_name) as number_start_rides_station from  `voltaic-mantra-364014.Cyclistic_project.customers` where member_casual = "member" group by start_station_name order by number_start_rides_station desc`

- _Casual clients had 218 times Clark St & Elm St as destination, while members had 675 times Clark St & Elm St as destination._


![Count_end_station_name](https://user-images.githubusercontent.com/115048292/208265747-ebb4f5d5-4466-4a53-a411-1a17bf33e128.png)

`select end_station_name, count(end_station_name) as number_end_rides_station from  `voltaic-mantra-364014.Cyclistic_project.customers` where member_casual = "member" group by end_station_name order by number_end_rides_station desc`



  


### Recommendations

After analyse customer behavior for 30 days rides for Cyclistic company between as called "casual" customers and "member" customer, it was verified that both customers have similar behaviors, such as averange distance, start station and end station. However, it was noticed that "casual" customers have an averange of time travel of _**seventy-three**_ minutes, it´s about _**fifhty-tow**_ minutes plus diference then "members" customers that take an averange of _**twenty-one**_ minutes, wich leads us to conclude that "casual" customers uses Cyclistic bike-share mainly for leisure while "member" use it to commute to work.  

![Image](https://user-images.githubusercontent.com/115048292/201484142-87a53e24-8795-4531-8f04-ee923f5dfc87.png)

This Viz show us a population were bubles demension are mesuared by time travel duration between casual customers and members customers.



Regarding the analysis of the data cyclistic bike-share leads to conclude with the following recomendations to solve this business problem:

- Set pricing advantages or promotions for casual clients to convert casual clients into members, their behavior are very similar but casual clients are set to use bike-share more for leisure rather then member clients as the time of using this bikes is also longer, so it will be appealing if mambership prices could be atractive for this clients;

- Set an strong e-mail marketing strategy regarding casual clients e-mails, also set an outdoor marketing strategy regarding st clark station wich is the most used for casual clients;

- Use digital media for marketing targeting bike leisure platforms with membership promotions wich are prefered for casual clients, this could impact business goal efectivly.   
