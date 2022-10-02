---
title: '[python]Plant Spatial Points Tool-build data for spatial point pattern analysis'
date: 2022-08-02 21:25:52
categories: python
tags: [python, coding, code, plant ecology, tree pattern, spatial point pattern, spatial pattern, tool, github]
---
## Plant Spatial Points Tool
[![DOI](https://zenodo.org/badge/161195182.svg)](https://zenodo.org/badge/latestdoi/161195182)  
* This is a software about annotation for plant spatial points, need PostgreSQL, build application in Python 3.7  
* It can help build data for analysis about relationships in plant ecology  
* [Windows Releases](https://github.com/leeyaowen/Mapkeying_python/releases)  

<!--more-->
## PostgreSQL setting(with pgAdmin)
1. set server default
2. set password -> 2717484
3. create a database **{your database name}**
4. set a relation **{your relation name}**(e.g., plotdata)
5. set column, including **x1,y1,x2,y2,tag,sp,dbh,x3,y3**, set dbh as numeric. Otherwise, text  
6. set tag as primary key(in Constraints setting)  
7. import data(csv format, and UTF-8 is encoding)  

### About relation column names
* x1,y1,x2,y2 -> plot position  
* tag -> plant ID  
* sp -> species name  
* dbh -> diameter breast height  
* x3,y3 -> plants points position, record by this program  

### Commonly used sql commands
> in SQL Eidtor
>> `select * from plotdata` -> select all data from plotdata  
>> `select * from plotdata where x3 is null` -> select x3 data is null from plotdata  

## User guide  
1. enter the database and relation name, and lock  
2. choose area(meter)    
3. set main/sub grids, e.g., (1,5)/(2,5)/(3,3)  
4. set quadrat
5. click 'Go/Refresh' button   
6. enter target tag  
7. click on the map

### Screenshot  
![](https://raw.githubusercontent.com/leeyaowen/Plant-Spatial-Points-Tool/master/screenshot/Plant%20Spatial%20Points%20Tool.JPG)

### Citation  
<p align="left">Yao-Wen Lee (2022) Plant Spatial Points Tool. URL: https://github.com/leeyaowen/Plant-Spatial-Points-Tool DOI: 10.5281/zenodo.6931327</p>  