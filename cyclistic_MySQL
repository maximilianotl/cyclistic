--Create a database

create database portfolio_projects;

--Select a database

use portfolio_projects;

--Create a new table

create table cyclistic (
       ride_id varchar (20)
     , rideable_type varchar (15)
     , started_at datetime
     , ended_at datetime
     , start_station_name varchar (100)
     , start_station_id varchar (50)
     , end_station_name varchar (100)
     , end_station_id varchar (50)
     , start_lat varchar (20)
     , start_lng varchar (20)
     , end_lat varchar (20)
     , end_lng varchar (20)
     , member_casual varchar(10) 
       );

--Import CSV files

    load data local infile 'c:\\users\\documents\\data analyst//202201-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202202-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202203-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202204-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202205-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202206-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202207-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202208-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202209-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202210-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202211-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

    load data local infile 'c:\\users\\data analyst//202212-divvy-tripdata.csv'
    into table cyclistic
  fields terminated by ','
enclosed by '"'
   lines terminated by '\n'
  ignore 1 rows;

--Clean data
--Delete trips in 2023

delete from cyclistic
 where ended_at 
  like '2023%';

--Delete trips with ending date minor than starting date

delete from cyclistic
 where timediff(ended_at, started_at)<0;

--n_tiles

set @rowindex := -1;

select n_tile
     , min(ride_length)   as lower_bound
     , max(ride_length)   as upper_bound
     , member_casual
     , rideable_type
     , count(ride_length) as count_rides
     , sec_to_time(avg(time_to_sec(ride_length))) as mean
     , (select sec_to_time(avg(time_to_sec(ride_length))) 
          from (
               select *
                    , timediff(ended_at, started_at) as ride_length
                 from cyclistic
                      ) as h
                ) as mean_all
     , (select ride_length as mode_length
          from (
               select ride_length
                    , count
                    , dense_rank() over(order by count desc) as rnk
                 from (
                      select ride_length
                           , count(*) as count
                        from (
                             select *
                                  , timediff(ended_at, started_at) as ride_length
                               from cyclistic
                             ) as aaaa
                    group by ride_length
                      ) as x
               ) as y
         where rnk = 1
       ) as mode
     , (select sec_to_time(avg(time_to_sec(g.grade))) as median
          from (
               select @rowindex:=@rowindex + 1 as rowindex
                    , aaa.ride_length as grade
                 from (
                      select *
                           , timediff(ended_at, started_at) as ride_length
                        from cyclistic
                      ) as aaa
             order by aaa.ride_length) as g
                where g.rowindex in (floor(@rowindex / 2)
                    , ceil(@rowindex / 2))
       ) as median
  from (
       select ntile(12) over (order by ride_length) as n_tile
            , member_casual
            , rideable_type
            , ride_length
         from (
              select member_casual
                   , rideable_type
                   , timediff(ended_at, started_at) as ride_length
                from cyclistic
              ) as a
       ) as aa
 group by 1,4,5;

--Stations

  select f.member_casual
       , f.rideable_type
       , f.start_station_name
       , f.start_lng
       , f.start_lat
       , f.top_start_stations
       , f.s_rides
       , f.end_station_name
       , f.end_lng
       , f.end_lat
       , f.top_end_stations
       , f.rides
       , min(g.rlength) as min_ride_length
       , max(g.rlength) as max_ride_length
       , sec_to_time(avg(time_to_sec(g.rlength))) as average_ride
       , f.distance_km
    from (
         select w.member_casual
              , w.rideable_type
              , w.start_station_name
              , w.start_lng
              , w.start_lat
              , w.top_start_stations
              , e.end_station_name
              , e.end_lng
              , e.end_lat
              , e.top_end_stations
              , e.e_rides as rides
              , round(((111.18)*(acos((sin(w.start_lat*pi()/180)*sin(e.end_lat*pi()/180))+(cos(w.start_lat*pi()/180)*cos(e.end_lat*pi()/180)*cos((e.end_lng-w.start_lng)*pi()/180)))*180/pi())),2) as distance_km
              , w.s_rides
           from (
                select *
                  from (
                       select member_casual
                            , rideable_type
                            , start_station_name
                            , start_lng, start_lat
                            , s_rides
                            , rank() over(partition by member_casual
                                                     , rideable_type 
                                              order by s_rides desc) as top_start_stations
                         from (
                              select member_casual
                                   , rideable_type
                                   , start_station_name
                                   , start_lng
                                   , start_lat
                                   , count(*) as s_rides
                                from cyclistic
                            group by 1,2,3,4,5
                              ) as a
                     group by 1,2,3,4,5,6
                       ) as aa
                 where top_start_stations in (1,2,3,4,5)
                ) as w,
 lateral (
         select *
           from (
                select *
                     , rank() over(partition by w.member_casual
                                              , w.rideable_type
                                              , w.top_start_stations 
                                       order by a.e_rides desc) as top_end_stations
                  from (
                       select end_station_name
                            , end_lng
                            , end_lat
                            , count(*) as e_rides
                         from cyclistic as c
                        where w.member_casual=c.member_casual
                          and w.rideable_type=c.rideable_type
                          and w.start_station_name = c.start_station_name
                          and w.start_lng=c.start_lng
                          and w.start_lat=c.start_lat
                     group by 1,2,3
                       ) as a
              group by 1,2,3,4
                ) as aa
   where top_end_stations in (1,2,3)
         ) as e
group by 1,2,3,4,5,7,8,9,11
         ) as f
    join (
         select *
              , timediff(ended_at, started_at) as rlength
           from cyclistic
         ) as g
      on f.member_casual=g.member_casual
     and f.rideable_type=g.rideable_type
     and f.start_station_name =g.start_station_name
     and f.start_lng=g.start_lng
     and f.start_lat=g.start_lat
     and f.end_station_name=g.end_station_name
     and f.end_lng=g.end_lng
     and f.end_lat=g.end_lat
group by 1,2,3,4,5,7,8,9,10,12;

--Time series
--Simple trends
--Month

  select member_casual
       , rideable_type
       , monthname(started_at) as month
       , count(*) as rides
    from cyclistic
group by 1,2,3;

--Week

  select member_casual
       , rideable_type
       , week(started_at) as week
       , count(*) as rides
    from cyclistic
group by 1,2,3;

--Day name

  select member_casual
       , rideable_type
       , dayname(started_at) as day
       , count(*) as rides
    from cyclistic
group by 1,2,3;

--Hour

  select member_casual
       , rideable_type
       , hour(started_at) as hour
       , count(*) as rides
    from cyclistic
group by 1,2,3;

--Ratios

select month
     , casual
     , member
     , (member-casual) as member_minus_casual
     , (member/casual) as member_times_of_casual
     , ((member/casual-1)*100) as member_pct_of_casual
  from (
       select month
            , sum(case when user='casual' then rides end) as casual
            , sum(case when user='member' then rides end) as member
         from (
              select monthname(started_at) as month
                   , count(*) as rides
                   , member_casual as user
                from cyclistic
            group by 1,3
              ) as a
     group by 1
       ) as aa;

--Percentages
--Monthly percentage by user, Monthly percentage by user compared with total of yearly rides, Percentage indexed to January

  select month
       , user
       , rides
       , ((rides*100)/sum(rides) over(partition by month)) as pct_total_rides
       , ((rides*100)/sum(rides) over(partition by year)) as pct_yearly
       , (rides/first_value(rides) over(partition by user order by mon)-1)*100 as pct_from_index
    from (
         select monthname(started_at) as month
              , month(started_at) as mon
              , member_casual as user
              , year(started_at) as year
              , count(*) as rides
           from cyclistic
       group by 1,2,3,4
         ) as a
order by str_to_date(month,'%m');


--Moving average
--Week

  select week
       , avg(rides) over(order by week rows between 52 preceding and current row) as moving_avg
       , count(rides) over(order by week rows between 52 preceding and current row) as records_count
    from (
         select week(started_at) as week
              , count(*) as rides
           from cyclistic
       group by 1
         ) as a;

--Month

  select month
       , avg(rides) over(order by mon rows between 11 preceding and current row) as moving_avg
       , count(rides) over(order by mon rows between 11 preceding and current row) as records_count
    from (
         select monthname(started_at) as month
              , month(started_at) as mon
              , count(*) as rides
           from cyclistic
       group by 1,2
         ) as a
order by str_to_date(month,'%m');

--Cumulative values

  select week
       , rides
       , sum(rides) over(partition by mon order by week) as month_rides
    from (
         select month(started_at) as mon
              , week(started_at) as week
              , count(*) as rides
           from cyclistic
       group by 1,2
         ) as a
order by 1;


--Month-over-Month

  select user
       , month
       , rides
       , (rides/lag(rides) over(partition by user order by mon)-1)*100 as pct_growth_from_previous
    from (
         select monthname(started_at) as month
              , month(started_at) as mon
              , member_casual as user
              , count(*) as rides
           from cyclistic
       group by 1,2,3
         ) as a
order by str_to_date(month,'%m');

--Week-over-Week

  select user
       , week
       , rides
       , (rides/lag(rides) over(partition by user order by week)-1)*100 as pct_growth_from_previous
    from (
         select  week(started_at) as week
               , member_casual as user
               , count(*) as rides
            from cyclistic
        group by 1,2
         ) as a
order by 2;


--Variation of the number of rides by day of the week compared by month

  select user
       , number_day
       , day
       , max(case when mon=1 then rides end) as rides_1
       , max(case when mon=2 then rides end) as rides_2
       , max(case when mon=3 then rides end) as rides_3
       , max(case when mon=4 then rides end) as rides_4
       , max(case when mon=5 then rides end) as rides_5
       , max(case when mon=6 then rides end) as rides_6
       , max(case when mon=7 then rides end) as rides_7
       , max(case when mon=8 then rides end) as rides_8
       , max(case when mon=9 then rides end) as rides_9
       , max(case when mon=10 then rides end) as rides_10
       , max(case when mon=11 then rides end) as rides_11
       , max(case when mon=12 then rides end) as rides_12
    from (
         select dayname(started_at) as day
              , dayofweek(started_at) as number_day
              , month(started_at) as mon
              , member_casual as user
              , count(*) as rides
           from cyclistic
       group by 1,2,3,4
         ) as a
group by 1,2,3
order by 1,2;



