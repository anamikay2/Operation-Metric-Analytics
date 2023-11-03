# Operation-Metric-Analytics
show databases;
use projects;
create table job_data
(ds date,
job_id int not null,
actor_id int not null,
event varchar(15) not null,
language varchar (15) not null,
time_spent int not null,
org char(2)
);
insert into job_data(ds,job_id,actor_id,event,language,time_spent,org)
values('2020/11/30',21,1001,'skip',	'English',15,'A'),
('2020/11/30',22,1006,'transfer','Arabic',25,'B'),
('2020/11/29',23,1003,'decision','Persian',20,'C'),
('2020/11/28',23,1005,'transfer','Persian',22,'D'),
('2020/11/28',25,1002,'decision','Hindi',11,'B'),
('2020/11/27',11,1007,'decision','French',104,'D'),
('2020/11/26',23,1004,'skip','Persian',56,'A'),
('2020/11/25',20,1003,'transfer','Italian',45,'C');
select* from job_data;
#query 1
select avg(t) 'avg job reviewed per day per hour',
avg(p) as 'avg job reviewd per day per second '
from
(select 
ds,
((count(job_id)*36000)/sum(time_spent)) as t , 
((count(job_id))/sum(time_spent))as p
from 
job_data
where
month(ds)=11
group by ds) a;
 #QUERY 2
  SELECT  ROUND(COUNT(event)/SUM(time_spent),2)      AS         "Weekly Throughput"
  FROM JOB_DATA;
  SELECT ds AS dates, ROUND(COUNT(event)/SUM(time_spent),2)
  AS  'Daily Throughput'
  FROM JOB_DATA
  GROUP BY ds
  ORDER BY ds;
  #Query 3
  SELECT language AS Languages,  ROUND(100 * COUNT(*)/total, 2) 
  AS Percentage, SUB.TOTAL 
  FROM JOB_DATA
  CROSS JOIN (SELECT COUNT(*) AS total
  FROM JOB_DATA)
  AS sub GROUP BY LANGUAGE, SUB.TOTAL;
  #QUERY 4
  job_dataSELECT actor_id,  COUNT(*) AS Duplicates 
  FROM JOB_DATA
  GROUP BY actor_id  HAVING COUNT(*) > 1;
  



