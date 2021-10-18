# 🚀 POSTGRESQL (SQL) Cheatsheet 🚀 <!-- omit in TOC -->

![image](./postgresql_logo.png)

- [Requirement](#requirement)
  - [Environment variable (Windows)](#environment-variable-windows)
- [Context explaination](#context-explaination)
  - [Ressources](#ressources)
  - [Mockaroo](#mockaroo)
- [Launch script](#launch-script)
  - [windows](#windows)
  - [mac & linux](#mac--linux)
- [Create database](#create-database)
  - [check list of db](#check-list-of-db)
- [Connect to database](#connect-to-database)
  - [url command line method](#url-command-line-method)
  - [command line method](#command-line-method)
    - [Setup pgpass file](#setup-pgpass-file)
    - [Setup pg_hba file](#setup-pg_hba-file)
- [Check](#check)
  - [list of db](#list-of-db)
  - [list dataTable and sequencies](#list-datatable-and-sequencies)
  - [show a table](#show-a-table)
  - [Activate / Desactivate extended view](#activate--desactivate-extended-view)
  - [Clear console](#clear-console)
- [SQL Basics keyword](#sql-basics-keyword)
  - [CREATE TABLE](#create-table)
  - [DELETE TABLE](#delete-table)
  - [ADD one person as a ROW (Record)](#add-one-person-as-a-row-record)
  - [SELECT](#select)
  - [ORDER BY](#order-by)
  - [DISTINCT](#distinct)
  - [WHERE CLAUSE](#where-clause)
  - [Comparison](#comparison)
  - [LIMIT and OFFSET keywords](#limit-and-offset-keywords)
  - [IN](#in)
  - [BETWEEN](#between)
  - [LIKE and ILIKE](#like-and-ilike)
  - [Group by](#group-by)
- [UPDATE / DELETE / ADD](#update--delete--add)
  - [ALTER a column](#alter-a-column)
  - [DELETE](#delete)
  - [UPDATE](#update)
- [Arithmetics operation & function](#arithmetics-operation--function)
  - [MIN / MAX / AVG / SUM](#min--max--avg--sum)
  - [arithmetique operation](#arithmetique-operation)
  - [alias AS](#alias-as)
  - [COALESCE](#coalesce)
  - [NULLIF](#nullif)
    - [Usecase handling error when divide by 0](#usecase-handling-error-when-divide-by-0)
  - [DATE](#date)
    - [set a time zone](#set-a-time-zone)
- [EXTRACT](#extract)
  - [AGE](#age)
    - [usecase extract month](#usecase-extract-month)
- [PRIMARY KEYS / FOREIGN KEY & CONSTRAINT](#primary-keys--foreign-key--constraint)
  - [PK for PRIMARY KEY](#pk-for-primary-key)
  - [CONSTRAINT](#constraint)
    - [ADD](#add)
    - [DROP](#drop)
    - [CHECK CONSTRAINT](#check-constraint)
  - [ON Conflict Do Nothing](#on-conflict-do-nothing)
  - [FOREIGN KEY](#foreign-key)
    - [ADD a references with UPDATE](#add-a-references-with-update)
    - [JOIN](#join)
    - [DELETE ROW when we have a FOREIGN KEY](#delete-row-when-we-have-a-foreign-key)
- [EXPORT to CSV with postrgesql](#export-to-csv-with-postrgesql)
- [SEQUENCES](#sequences)
- [EXTENSION](#extension)
  - [UUID](#uuid)


<br>
<br>

___
## Requirement
* Postgresql : [link for installation](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads) I use version 12.7

### Environment variable (Windows)

<details>
<summary>Environment variable Zone</summary>

![image](./environment_variable_1.PNG)

</details>

### __```Zone 1 ```__ <!-- omit in toc -->
| Variable   |      Value  |   
|----------|:-------------:|
| Path | C:\Program Files\PostgreSQL\12\bin (Your own postgreSQL bin path) | 

<details>
<summary>Environment variable : path</summary>

![image](./environment_variable_2.PNG)

</details>

<br>
<br>

___
## Context explaination

<div align="center">

![image](postgresql_forma/Relation_1.PNG)
</div>

* Person has car
* Person can only have one car
* Car can belong to one person only

<br>

> **Note :** 
> * I have conscience that this rules maybe are disapointing because normaly 'Person can have car' mean 0, 1 or more. But here we see that the FK who reference a car was inside the table person, right? And a person should be unique inside our table so it's mean : 'Person can only have one car'.
> * One solution for this concern will be to simply put the FK inside the car table, so it's will be a car who will reference a person or not. But in our case one of the sources we use for learning with practice using this great ressources : [[FCC] Learn PostgreSQL Tutorial - Full Course for Beginners [EN][Amigoscode]](https://www.youtube.com/watch?v=qw--VYLpxG4) that's why we keep the same context given inside the video because if a concept seem complexe when we are going back to it we can simply go to the video at the right section and get some hint.

<div style="max-width:100%; margin:auto" align="center">
  <object data="./out/plantUML/Database_Schema/Database_Schema.svg" type="image/svg+xml" width="100%" style="max-width:830px"">
    <img src="./out/plantUML/Database_Schema/Database_Schema.svg" />
  </object>
</div>

* Left : current relationship (The one used)
* Right : normaly what should be more correct (Just provided for information)

Around the end we use UUID function for generate "id" but not at start, like it's a step by step learning path with incremental methodology. The cheatsheet follow the same behaviour.

### Ressources 

<details>
<summary>⭐️ Free Content : [FCC] Learn PostgreSQL Tutorial - Full Course for Beginners [EN][Amigoscode] ⭐️</summary>

<br>

* [⌨️ What is a Database](https://youtu.be/qw--VYLpxG4?t=196)
* [⌨️ What is SQL And Relational Database](https://youtu.be/qw--VYLpxG4?t=317)
* [⌨️ What is PostreSQL AKA Postrgres](https://youtu.be/qw--VYLpxG4?t=550)
* [⌨️ PostgreSQL Installation (Mac OS)](https://youtu.be/qw--VYLpxG4?t=653)
* [⌨️ PostgreSQL Installation (Windows)](https://youtu.be/qw--VYLpxG4?t=861)
* [⌨️ GUI Clients vs Terminal/CMD Clients](https://youtu.be/qw--VYLpxG4?t=1058)
* [⌨️ Setup PSQL (MAC OS)](https://youtu.be/qw--VYLpxG4?t=1299)
* [⌨️ Setup PSQL (Windows)](https://youtu.be/qw--VYLpxG4?t=1522)
* [⌨️ How to Create Database](https://youtu.be/qw--VYLpxG4?t=1815)
* [⌨️ How to Connect to Databases](https://youtu.be/qw--VYLpxG4?t=2015)
* [⌨️ A Very Dangerous Command](https://youtu.be/qw--VYLpxG4?t=2292)
* [⌨️ How To Create Tables](https://youtu.be/qw--VYLpxG4?t=2497)
* [⌨️ Creating Tables Without Constraints](https://youtu.be/qw--VYLpxG4?t=2746)
* [⌨️ Creating Tables with Constraints](https://youtu.be/qw--VYLpxG4?t=2952)
* [⌨️ Insert Into](https://youtu.be/qw--VYLpxG4?t=3355)
* [⌨️ Insert Into Example](https://youtu.be/qw--VYLpxG4?t=3555)
* [⌨️ Generate 1000 Rows with Mockaroo](https://youtu.be/qw--VYLpxG4?t=3756)
* [⌨️ Select From](https://youtu.be/qw--VYLpxG4?t=4348)
* [⌨️ Order By](https://youtu.be/qw--VYLpxG4?t=4518)
* [⌨️ Distinct](https://youtu.be/qw--VYLpxG4?t=4793)
* [⌨️ Where Clause and AND](https://youtu.be/qw--VYLpxG4?t=4919)
* [⌨️ Comparison Operators](https://youtu.be/qw--VYLpxG4?t=5129)
* [⌨️ Limit, Offset & Fetch](https://youtu.be/qw--VYLpxG4?t=5375)
* [⌨️ IN](https://youtu.be/qw--VYLpxG4?t=5563)
* [⌨️ Between](https://youtu.be/qw--VYLpxG4?t=5743)
* [⌨️ Like And iLike](https://youtu.be/qw--VYLpxG4?t=5865)
* [⌨️ Group By](https://youtu.be/qw--VYLpxG4?t=6190)
* [⌨️ Group By Having](https://youtu.be/qw--VYLpxG4?t=6401)
* [⌨️ Adding New Table And Data Using Mockaroo](https://youtu.be/qw--VYLpxG4?t=6728)
* [⌨️ Calculating Min, Max & Average](https://youtu.be/qw--VYLpxG4?t=6940)
* [⌨️ Sum](https://youtu.be/qw--VYLpxG4?t=7188)
* [⌨️ Basics of Arithmetic Operators](https://youtu.be/qw--VYLpxG4?t=7315)
* [⌨️ Arithmetic Operators (ROUND)](https://youtu.be/qw--VYLpxG4?t=7559)
* [⌨️ Alias](https://youtu.be/qw--VYLpxG4?t=7783)
* [⌨️ Coalesce](https://youtu.be/qw--VYLpxG4?t=7952)
* [⌨️ NULLIF](https://youtu.be/qw--VYLpxG4?t=8175)
* [⌨️ Timestamps And Dates Course](https://youtu.be/qw--VYLpxG4?t=8421)
* [⌨️ Adding And Subtracting With Dates](https://youtu.be/qw--VYLpxG4?t=8601)
* [⌨️ Extracting Fields From Timestamp](https://youtu.be/qw--VYLpxG4?t=8758)
* [⌨️ Age Function](https://youtu.be/qw--VYLpxG4?t=8848)
* [⌨️ What Are Primary Keys](https://youtu.be/qw--VYLpxG4?t=8964)
* [⌨️ Understanding Primary Keys](https://youtu.be/qw--VYLpxG4?t=9083)
* [⌨️ Adding Primary Key](https://youtu.be/qw--VYLpxG4?t=9386)
* [⌨️ Unique Constraints](https://youtu.be/qw--VYLpxG4?t=9655)
* [⌨️ Check Constraints](https://youtu.be/qw--VYLpxG4?t=10155)
* [⌨️ How to Delete Records](https://youtu.be/qw--VYLpxG4?t=10485)
* [⌨️ How to Update Records](https://youtu.be/qw--VYLpxG4?t=10896)
* [⌨️ On Conflict Do Nothing](https://youtu.be/qw--VYLpxG4?t=11155)
* [⌨️ Upsert](https://youtu.be/qw--VYLpxG4?t=11469)
* [⌨️ What Is A Relationship/Foreign Keys](https://youtu.be/qw--VYLpxG4?t=11801)
* [⌨️ Adding Relationship Between Tables](https://youtu.be/qw--VYLpxG4?t=11988)
* [⌨️ Updating Foreign Keys Columns](https://youtu.be/qw--VYLpxG4?t=12304)
* [⌨️ Inner Joins](https://youtu.be/qw--VYLpxG4?t=12570)
* [⌨️ Left Joins](https://youtu.be/qw--VYLpxG4?t=12917)
* [⌨️ Deleting Records With Foreign Keys](https://youtu.be/qw--VYLpxG4?t=13253)
* [⌨️ Exporting Query Results to CSV](https://youtu.be/qw--VYLpxG4?t=13647)
* [⌨️ Serial & Sequences](https://youtu.be/qw--VYLpxG4?t=13842)
* [⌨️ Extensions](https://youtu.be/qw--VYLpxG4?t=14238)
* [⌨️ Understanding UUID Data Type](https://youtu.be/qw--VYLpxG4?t=14379)
* [⌨️ UUID As Primary Keys](https://youtu.be/qw--VYLpxG4?t=14754)
* [⌨️ Conclusion](https://youtu.be/qw--VYLpxG4?t=15390)

 </details>

 <details>
 <summary>⭐️ Paid Content : Advanced Databases [EN][Amigoscode] ⭐️</summary>
<br>

Advanced Database
In this course you will learn advanced database concepts that you must know to write production ready applications. As a software engineer knowing these concepts will make sure you write solid backend application and fully understand what goes on behind the scenes. You will learn the following:
* Joins
* Indexes
* Transactions
* Database Administration
* Functions / Stored Procedures
* Schemas
* Database Backups

[Advanced Database : Amigoscode](https://www.amigoscode.com/p/advanced-databases) need to be paid courses for following the video.
</details>

 <details>
 <summary>⭐️ Paid Content : Database Design & Implementation [EN][Amigoscode] ⭐️</summary>
<br>

Database Design & Implementation
Backend starts from Your Database. When you have a solid database design coding is easy.
In this course you will learn how to design databases for your backend applications. You will learn how to design a subset of YouTube Platform allowing you design your own databases.
* Capture Entities
* Designing ERD
* ERD Cardinalities
* One to One Relationships
* One to Many Relationships
* Many to Many Relationships
* Defining Constraints
* Database Normalisation
* SQL Implementation

[Database Design & Implementation : Amigoscode](https://www.amigoscode.com/p/database-design) need to be paid courses for following the video.
</details>


 <details>
 <summary>⭐️ Free Content : Tutorials and PostgreSQL training [FR][xavki] ⭐️</summary>
<br>

Tutorial in French to start and progress on PostgreSQL. Many tips, definitions and concepts: psql, installation, tables, databases, pg_hba, postgresql, replication, archiving, backups, restore...

[Tutorials and PostgreSQL training [FR][xavki]](https://www.youtube.com/playlist?list=PLn6POgpklwWonHjoGXXSIXJWYzPSy2FeJ)

 </details>



 <details>
 <summary>⭐️ Free Content : Online exercice for SQL practice [EN][pgexercises] ⭐️</summary>
<br>

[Free Content : Online exercises for SQL practice [EN][pgexercices]](https://pgexercises.com/)

 </details>

> **Note :** I'm sorry but when I learn postgresql and sql using Amigoscode content on youtube i find him content awesome and straightforward. So i includ here him link to the paid content too if people want to thanks him awesome content. And for be able to see it, you will need to enroll on Amigoscode course.

<br>
<br>

### Mockaroo

We can use the website [Mockaroo](https://www.mockaroo.com/) for generating our table and some data :
* person :

|id|first_name|Last_name|email|gender|date_of_birth|country_of_birth|
|---|---|---|---|---|---|---|

![image](./postgresql_forma/Mockaroo.PNG)

after that we do some ajustement on ``person.sql`` file to fit the previous schemas :
```SQL
create table person (
	id BIGSSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	email VARCHAR(150),
	gender VARCHAR(50) NOT NULL,
	date_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50),
    car_id BIGINT REFERENCES car (id),
    UNIQUE(car_id),
    UNIQUE(email)
);
```

<br>

* car :
  
|id|make|model|price|
|---|---|---|---|

and for ``car.sql`` file :
```SQL
create table car (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	make VARCHAR(100) NOT NULL,
	model VARCHAR(100) NOT NULL,
	price NUMERIC(19, 2) NOT NULL
);
```

<br>

## Launch script

### windows 
```shell
\i 'C:\\Temp\\person.sql'
```

### mac & linux 
```shell
\i C:/Temp/person.sql
```

<br>
<br>

___

## Create database

```SQL
CREATE DATABASE name_of_db;
```

### check list of db

```
\l
```

<br>
<br>

___

## Connect to database

### url command line method

```shell
psql postgresql://USERNAME:PASSWORD@ADRESSE:PORT/DB_Name
```

* __``USERNAME``__ : your username the one you use when installing postgresql or if you modify that or create a new username later
* __``PASSWORD``__ : your password the one you use when installing postgresql or if you modify that or create a new username later
* __``ADRESSE``__ : adresse where is hosted your database __``localhost``__ can work
* __``PORT``__ : port for your database can be define inside your pgAdmin UI but default value is __``5432``__
* __``DB_Name``__ : the name of your database you want to connect

### command line method

```shell
psql -h ADRESSE -p PORT -U USERNAME DB_Name
```

this method will ask you a password if you haven't setup a __``.pgpass``__ file or __``pg_hba.conf``__ file but if you have setup one then you don't need a password.

#### Setup pgpass file

#### Setup pg_hba file

<br>
<br>


___
## Check 

### list of db

```
\l
```
<br>

### list dataTable and sequencies

```
\d
```

<br>

Alternative : show only table
```
\dt
```
<br>

### show a table

```
\d person
```

<br>

### Activate / Desactivate extended view 
```
\x
```

<br>

### Clear console
```
\! cls
```


<br>
<br>


___

## SQL Basics keyword
### CREATE TABLE

<details>
<summary>Create table without constraint</summary>

```SQL
CREATE TABLE person (
first_name VARCHAR(50),
last_name VARCHAR(50),
gender VARCHAR(7),
date_of_birth DATE);
```

</details>

<details>
<summary>Create table with constraint and primary key</summary>

```SQL
CREATE TABLE person (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	email VARCHAR(150),
	gender VARCHAR(50) NOT NULL,
	date_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50),
);
```

</details>

```SQL
CREATE TABLE person (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	email VARCHAR(150),
	gender VARCHAR(50) NOT NULL,
	date_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50),
    car_id BIGINT REFERENCES car(id),
    UNIQUE(car_id),
    UNIQUE(email)
);
```

> **BIGSERIAL :** can be use for ID because automatics incrementation inside the bdd
> 
> **UUID :** method for generating unique ID can be use for PK __``Primary Keys``__


<br>
<br>

___
### DELETE TABLE

```SQL
DROP TABLE person
```
* person : name of the table


<br>
<br>

___
### ADD one person as a ROW (Record)

```SQL
INSERT INTO person (
first_name, last_name, gender, date_of_birth, email)
VALUES ('Jake', 'Jones', 'MALE', DATE '1990-01-10', 'jake@gmail.com');
```

<br>
<br>

___
### SELECT

command :
```SQL
SELECT first_name, last_name FROM person;
```

<br>

<details>
<summary>output :</summary>

```SQL
  first_name  |   last_name
--------------+----------------
 Xylia        | Sprigging
 Arlette      | Elcoux
 Maxim        | Shillabear
 Mannie       | Avrahamoff
 Orrin        | Fayre
 Heindrick    | Pulfer
 Evelyn       | Erington
 Gerhard      | Daice
...
...
```
</details>

<br>
<br>

___
### ORDER BY

ordering by default : __``ascending``__  
* 1 --> 2 --> 3 --> 4 --> ...
* A --> B --> C --> D --> ...

command :

<details>
<summary>command for ordering by ascending ASC order</summary>

```SQL
SELECT * FROM person ORDER BY country_of_birth;
```

and

```SQL
SELECT * FROM person ORDER BY country_of_birth ASC;
```

<details>
<summary>output : </summary>

```
 id  |  first_name  |   last_name    |               email               |   gender    | date_of_birth |    country_of_birth
------+--------------+----------------+-----------------------------------+-------------+---------------+------------------------
  976 | Sharia       | Schimonek      | sschimonekr3@thetimes.co.uk       | Bigender    | 2021-08-16    | Afghanistan
  272 | Libbey       | Strowlger      | lstrowlger7j@youtube.com          | Genderqueer | 2021-06-25    | Afghanistan
  474 | Winnie       | Filby          | wfilbyd5@ibm.com                  | Non-binary  | 2021-09-06    | Afghanistan
   77 | Romy         | Faraday        |                                   | Non-binary  | 2020-12-18    | Afghanistan
  799 | Nikaniki     | Kerwood        | nkerwoodm6@tinypic.com            | Bigender    | 2021-09-22    | Aland Islands
  293 | Lock         | Brunnen        | lbrunnen84@soup.io                | Polygender  | 2021-07-27    | Albania
  163 | Ase          | Shinner        | ashinner4i@soundcloud.com         | Polygender  | 2020-11-01    | Albania
  981 | Edvard       | Quiddihy       | equiddihyr8@de.vu                 | Genderqueer | 2021-06-08    | Albania
   33 | Bertine      | Nancekivell    |                                   | Male        | 2021-03-07    | Albania
  972 | Bernarr      | Stoile         |                                   | Bigender    | 2021-09-19    | Albania
  399 | Erie         | Wogdon         |                                   | Agender     | 2020-12-01    | Albania
  916 | Corabelle    | O' Mara        |                                   | Female      | 2021-03-22    | Andorra
  402 | Kerstin      | Bernade        | kbernadeb5@feedburner.com         | Genderfluid | 2021-07-22    | Angola
  534 | Augustine    | Orkney         | aorkneyet@businessinsider.com     | Agender     | 2021-07-05    | Anguilla
  721 | Kevyn        | Carletti       | kcarlettik0@deviantart.com        | Bigender    | 2020-11-30    | Argentina
  871 | Domenico     | Maty           |                                   | Polygender  | 2020-11-13    | Argentina
...
...
```

</details>

<br>

> **Note :** order by ascending by default so you don't really need to use __``ASC``__ keyword

we can also 
```SQL
SELECT * FROM person ORDER BY country_of_birth, email;
```

</details>

<details>
<summary>command for ordering by ascending DSC order</summary>

```SQL
SELECT * FROM person ORDER BY country_of_birth DESC;
```

> **Note :** order by ascending by default so you don't really need to use __``DESC``__ keyword

</details>



<br>
<br>

___
### DISTINCT

description : remove duplicate inside preview

command :
```SQL
SELECT DISTINCT country_of_birth FROM person ORDER BY country_of_birth;
```
<br>

<details>
<summary>output :</summary>

```SQL
    country_of_birth
------------------------
 Afghanistan
 Aland Islands
 Albania
 Andorra
 Angola
 Anguilla
 Argentina
 Armenia
 Bahamas
 Bangladesh
 Belarus
 Bosnia and Herzegovina
 Botswana
 Brazil
...
...
```
</details>

<br>
<br>

___
### WHERE CLAUSE

description : filter by value 

**Note :** __``WHERE CLAUSE``__ is case sensitive

<details>
<summary>Filter with one condition on gender</summary>

command :
```SQL
SELECT * FROM person WHERE gender='Female';
```

<details>
<summary>output :</summary>

output : 
```SQL
 id  | first_name  |   last_name   |              email               | gender | date_of_birth | country_of_birth
-----+-------------+---------------+----------------------------------+--------+---------------+------------------
   7 | Evelyn      | Erington      | eerington6@answers.com           | Female | 2021-09-23    | China
  10 | Udall       | Vizor         | uvizor9@harvard.edu              | Female | 2021-07-28    | Iran
  20 | Chrissie    | Grouen        | cgrouenj@php.net                 | Female | 2021-02-28    | Russia
  21 | Pail        | Coade         | pcoadek@mtv.com                  | Female | 2021-06-01    | Russia
  35 | Alexandrina | Alltimes      | aalltimesy@senate.gov            | Female | 2020-11-03    | China
  43 | Shellie     | Georgeot      | sgeorgeot16@senate.gov           | Female | 2021-01-31    | Mexico
  49 | Merilyn     | Downing       | mdowning1c@dagondesign.com       | Female | 2020-12-21    | Russia
  73 | Shaylah     | Gilfoyle      | sgilfoyle20@cbc.ca               | Female | 2021-04-29    | Sweden
  79 | Salem       | McCart        | smccart26@devhub.com             | Female | 2020-11-26    | China
  81 | Tish        | McGlynn       |                                  | Female | 2021-09-26    | Laos
  82 | Zonnya      | Whistance     | zwhistance29@vk.com              | Female | 2021-05-17    | Thailand
...
```
</details>
<br>
</details>

<details>
<summary>Filter with multiple condition AND</summary>

command :
```SQL
SELECT * FROM person WHERE gender='Female' AND country_of_birth='Poland';
```
<br>
</details>

<details>
<summary>Filter with multiple condition AND & OR combination</summary>

command :
```SQL
SELECT * FROM person WHERE gender='Female' AND (country_of_birth='Poland' OR country_of_birth='China');
```

</details>

<br>
<br>

___
### Comparison 

command
```SQL
SELECT 1 = 2;
```

other operator mathematics possible : 
|operator|description|
|---|---|
|``>``|greater than|
|``>=``|greater than or equal|
|``<``|less than|
|``<=``|less than or equal|
|``<>``|not equal|


> **Note :** can be use on any datatype


<br>

<details>
<summary>output:</summary>

```SQL
 ?column?
----------
 f
(1 ligne)
```
<br>
</details>

> **Note :**
> * __``f``__ for false
> * __``t``__ for true



<br>
<br>

___
### LIMIT and OFFSET keywords

command :

<details>
<summary>SELECT 10 records (row) fron person</summary>

command : 
```SQL
SELECT * FROM person LIMIT 10;
```

<br>

<details>
<summary>output :</summary>

```SQL
 id | first_name | last_name  |            email            |   gender    | date_of_birth | country_of_birth
----+------------+------------+-----------------------------+-------------+---------------+------------------
  1 | Xylia      | Sprigging  | xsprigging0@paginegialle.it | Polygender  | 2020-10-25    | Zimbabwe
  2 | Arlette    | Elcoux     | aelcoux1@cbsnews.com        | Genderqueer | 2020-11-02    | Eritrea
  3 | Maxim      | Shillabear | mshillabear2@google.es      | Agender     | 2020-12-15    | United States
  4 | Mannie     | Avrahamoff |                             | Polygender  | 2021-07-12    | Russia
  5 | Orrin      | Fayre      | ofayre4@dyndns.org          | Genderfluid | 2021-08-29    | Philippines
  6 | Heindrick  | Pulfer     |                             | Male        | 2021-03-13    | China
  7 | Evelyn     | Erington   | eerington6@answers.com      | Female      | 2021-09-23    | China
  8 | Gerhard    | Daice      | gdaice7@japanpost.jp        | Agender     | 2021-10-11    | France
  9 | Pietro     | Borris     |                             | Genderqueer | 2020-11-02    | Japan
 10 | Udall      | Vizor      | uvizor9@harvard.edu         | Female      | 2021-07-28    | Iran
(10 lignes)
```

</details>
<br>
</details>

<details>
<summary>SELECT 10 records (row) fron person with an OFFSET of 5 so start to the 6th records and FETCH alternative command </summary>

```SQL
SELECT * FROM person OFFSET 5 LIMIT 10;
```

<br>

> **Note :** __``LIMIT``__ wasn't the normalise command for it on SQL, normaly it's __``FETCH``__ but like __``LIMIT``__ was really user friendly this one become popular and been implement almost on every database. See the fetch command below

```SQL
SELECT * FROM person OFFSET 5 FETCH FIRST 5 ROW ONLY;
```
<br>
</details>

<details>
<summary>SELECT the 10 lasts records from person</summary>

```SQL
SELECT * FROM person ORDER BY id DESC LIMIT 10;
```

<br>

</details>


```SQL
SELECT email FROM person WHERE email IS NOT NULL ORDER BY id DESC Limit 10;
```

<br>
<br>

___
### IN

command :
```SQL
SELECT * FROM person WHERE country_of_birth IN ('China', 'France');
```

<br>
<br>

___
### BETWEEN

<details>
<summary>retrieve person with id BETWEEN 6 and 40</summary>

```SQL
SELECT * FROM person WHERE id BETWEEN '6' AND '40';
```

<details>
<summary>output :</summary>

```
 id | first_name  |   last_name   |            email             |   gender    | date_of_birth |    country
----+-------------+---------------+------------------------------+-------------+---------------+----------------
  6 | Heindrick   | Pulfer        |                              | Male        | 2021-03-13    | China
  7 | Evelyn      | Erington      | eerington6@answers.com       | Female      | 2021-09-23    | China
  8 | Gerhard     | Daice         | gdaice7@japanpost.jp         | Agender     | 2021-10-11    | France
  9 | Pietro      | Borris        |                              | Genderqueer | 2020-11-02    | Japan
 10 | Udall       | Vizor         | uvizor9@harvard.edu          | Female      | 2021-07-28    | Iran
 11 | Virginie    | McFee         | vmcfeea@soup.io              | Polygender  | 2021-09-19    | Czech Republic
 12 | Quincey     | Tavinor       | qtavinorb@google.de          | Non-binary  | 2021-03-14    | China
 13 | Duffy       | Reven         |                              | Male        | 2021-01-31    | Indonesia
 14 | Kissee      | Anersen       | kanersend@sciencedaily.com   | Agender     | 2020-12-08    | South Sudan
 15 | Alaric      | Leil          | aleile@github.io             | Genderfluid | 2020-12-09    | Philippines
 16 | Doe         | Mawman        | dmawmanf@constantcontact.com | Genderfluid | 2020-11-07    | Japan
 17 | Atalanta    | Hallard       | ahallardg@elpais.com         | Genderfluid | 2021-04-07    | Cyprus
 18 | Wilfred     | Seear         | wseearh@ezinearticles.com    | Non-binary  | 2021-10-06    | Czech Republic
 19 | Kliment     | Behling       | kbehlingi@indiatimes.com     | Male        | 2021-05-14    | China
 20 | Chrissie    | Grouen        | cgrouenj@php.net             | Female      | 2021-02-28    | Russia
 21 | Pail        | Coade         | pcoadek@mtv.com              | Female      | 2021-06-01    | Russia
 22 | Vincent     | Payze         |                              | Bigender    | 2021-03-30    | Portugal
 23 | Anastassia  | Schieferstein | aschiefersteinm@alibaba.com  | Agender     | 2021-02-23    | Russia
 24 | Issie       | Worsnap       | iworsnapn@naver.com          | Male        | 2021-05-25    | Poland
 25 | Eugene      | Gookey        | egookeyo@comsenz.com         | Agender     | 2021-05-30    | Egypt
 26 | Junina      | Rollitt       | jrollittp@upenn.edu          | Bigender    | 2021-07-21    | Indonesia
 27 | Anett       | Arstingall    | aarstingallq@adobe.com       | Agender     | 2021-04-08    | France
 28 | Robyn       | Camies        |                              | Non-binary  | 2020-10-25    | Czech Republic
 29 | Petra       | Barrasse      | pbarrasses@gravatar.com      | Male        | 2021-07-22    | Indonesia
 30 | Ashlan      | Ahrenius      |                              | Non-binary  | 2021-05-19    | China
 31 | Linea       | Bierman       | lbiermanu@artisteer.com      | Genderqueer | 2021-01-03    | China
 32 | Ashton      | Harly         | aharlyv@harvard.edu          | Bigender    | 2021-06-29    | Armenia
 33 | Bertine     | Nancekivell   |                              | Male        | 2021-03-07    | Albania
 34 | Orrin       | Cleeton       |                              | Polygender  | 2021-10-07    | China
 35 | Alexandrina | Alltimes      | aalltimesy@senate.gov        | Female      | 2020-11-03    | China
 36 | Thorin      | Cattrell      |                              | Male        | 2021-06-06    | Indonesia
 37 | Larissa     | Peachment     |                              | Agender     | 2021-03-06    | Indonesia
 38 | Jdavie      | Ible          | jible11@fastcompany.com      | Bigender    | 2020-11-23    | Ukraine
 39 | Mariquilla  | Phare         |                              | Non-binary  | 2021-08-28    | El Salvador
 40 | Olympie     | Harwell       | oharwell13@usda.gov          | Agender     | 2020-11-10    | Cameroon
(35 lignes)
...
```
</details>

> **Note :** 6 AND 40 are included when you write the command :
> ```SELECT * FROM person WHERE id BETWEEN '6' AND '40';```

<br>
</details>

<details>
<summary>retrieve person with date_of_birth BETWEEN 2 DATES</summary>

command : 
```SQL
SELECT * FROM person WHERE date_of_birth BETWEEN DATE '2000-01-01' AND '2022-01-01';
```

OR

```SQL

SELECT * FROM person WHERE date_of_birth BETWEEN '2000-01-01' AND '2022-01-01';
```

<br>

<details>
<summary>output :</summary>

```
  id  |  first_name  |   last_name    |               email               |   gender    | date_of_birth |        country
------+--------------+----------------+-----------------------------------+-------------+---------------+------------------------
    2 | Arlette      | Elcoux         | aelcoux1@cbsnews.com              | Genderqueer | 2020-11-02    | Eritrea
    3 | Maxim        | Shillabear     | mshillabear2@google.es            | Agender     | 2020-12-15    | United States
    4 | Mannie       | Avrahamoff     |                                   | Polygender  | 2021-07-12    | Russia
    5 | Orrin        | Fayre          | ofayre4@dyndns.org                | Genderfluid | 2021-08-29    | Philippines
    6 | Heindrick    | Pulfer         |                                   | Male        | 2021-03-13    | China
    7 | Evelyn       | Erington       | eerington6@answers.com            | Female      | 2021-09-23    | China
    8 | Gerhard      | Daice          | gdaice7@japanpost.jp              | Agender     | 2021-10-11    | France
    9 | Pietro       | Borris         |                                   | Genderqueer | 2020-11-02    | Japan
   10 | Udall        | Vizor          | uvizor9@harvard.edu               | Female      | 2021-07-28    | Iran
   11 | Virginie     | McFee          | vmcfeea@soup.io                   | Polygender  | 2021-09-19    | Czech Republic   
   12 | Quincey      | Tavinor        | qtavinorb@google.de               | Non-binary  | 2021-03-14    | China
   13 | Duffy        | Reven          |                                   | Male        | 2021-01-31    | Indonesia
   14 | Kissee       | Anersen        | kanersend@sciencedaily.com        | Agender     | 2020-12-08    | South Sudan
   15 | Alaric       | Leil           | aleile@github.io                  | Genderfluid | 2020-12-09    | Philippines
   16 | Doe          | Mawman         | dmawmanf@constantcontact.com      | Genderfluid | 2020-11-07    | Japan
   17 | Atalanta     | Hallard        | ahallardg@elpais.com              | Genderfluid | 2021-04-07    | Cyprus
   18 | Wilfred      | Seear          | wseearh@ezinearticles.com         | Non-binary  | 2021-10-06    | Czech Republic   
   19 | Kliment      | Behling        | kbehlingi@indiatimes.com          | Male        | 2021-05-14    | China
   20 | Chrissie     | Grouen         | cgrouenj@php.net                  | Female      | 2021-02-28    | Russia
   21 | Pail         | Coade          | pcoadek@mtv.com                   | Female      | 2021-06-01    | Russia
   22 | Vincent      | Payze          |                                   | Bigender    | 2021-03-30    | Portugal
...
```
</details>

</details>

> **Note :** extremity are included with the Keywords __``BETWEEN``__ see example with __``id``__

<br>
<br>

___
### LIKE and ILIKE

retrieve all email ending by .com :
```SQL
SELECT * FROM person WHERE email LIKE '%.com'
```

* LIKE => case sensitive
* ILIKE => case sensitive

<br>

match ``_ _ _`` 3 characters following by ``@gmail.com``
```SQL
SELECT * FROM person WHERE email LIKE '___@gmail.com'
```

<br>
<br>

___
### Group by 

command : 
```SQL
SELECT country, COUNT(*) FROM person GROUP BY country;
```

<details>
<summary>output :</summary>

```SQL
        country         | count
------------------------+-------
 Bangladesh             |     7
 Indonesia              |   116
 Venezuela              |     8
 Cameroon               |     2
 Czech Republic         |    32
 Sweden                 |    26
 Montenegro             |     1
 Uganda                 |     4
 Jordan                 |     3
 Dominican Republic     |     3
 Cambodia               |     1
 Ireland                |     4
 Macedonia              |     2
 Sri Lanka              |     1
...
```
</details>

<br>

__``HAVING``__ perform a filter after __``GROUP BY``__ and need to be the next keyword after keyword __``GROUP BY``__
```SQL
SELECT country, COUNT(*) FROM person GROUP BY country HAVING COUNT(*) > 5 ORDER BY country DESC;
```


<details>
<summary>output :</summary>

```SQL
        country         | count
------------------------+-------
 Bangladesh             |     7
 Indonesia              |   116
 Venezuela              |     8
 Cameroon               |     2
 Czech Republic         |    32
 Sweden                 |    26
 Montenegro             |     1
 Uganda                 |     4
 Jordan                 |     3
 Dominican Republic     |     3
 Cambodia               |     1
 Ireland                |     4
 Macedonia              |     2
 Sri Lanka              |     1
...
```
</details>

> **Note :** Find other function than COUNT() and more information inside the documentation :
> * https://www.postgresql.org/docs/14/functions-aggregate.html


<br>
<br>


___
## UPDATE / DELETE / ADD
### ALTER a column
```SQL
ALTER TABLE person ALTER COLUMN gender set data type VARCHAR (50);
ALTER TABLE person ADD COLUMN country_of_birth VARCHAR (50);
ALTER TABLE person RENAME COLUMN country_of_birth TO country;
```

<br>
<br>

___

### DELETE

```SQL
DELETE FROM person WHERE id = 1;
```

<br>
<br>

___


### UPDATE 

command :
```SQL
UPDATE person SET email = 'test@gmail.com' WHERE id = 50;
UPDATE person SET email = 'test@gmail.com', first_name = 'John' WHERE id = 50;
```

<br>
<br>

___
## Arithmetics operation & function
### MIN / MAX / AVG / SUM

command
```SQL
SELECT MAX(price) FROM car;
```

<details>
<summary>output :</summary>

```SQL
   max
----------
 99915.50
(1 ligne)
```
</details>

* AVG
* MIN
* MAX
* SUM
> **Note :** Find other function than that and more information inside the documentation :
> * https://www.postgresql.org/docs/14/functions-aggregate.html


<br>

we can also use multiple function at same time example :
```SQL
SELECT ROUND(AVG(price)) FROM car;
```

<details>
<summary>output :</summary>

```SQL
 round
-------
 54850
(1 ligne)
```
</details>

<br>

we can also cast the result of a function with __``::``__ operator :
command :
```SQL
SELECT AVG(price)::numeric(10,2) FROM car;
```
<details>
<summary>output :</summary>

```SQL
   avg
----------
 54850.39
(1 ligne)
```
</details>

<br>


command :
```SQL
SELECT make, model, MIN(price) From car GROUP BY make, model;
```

<details>
<summary>output :</summary>

```SQL
     make      |        model         |   min
---------------+----------------------+----------
 Pontiac       | GTO                  | 32387.88
 Toyota        | Land Cruiser         | 51081.86
 Mercedes-Benz | S-Class              | 11810.67
 Infiniti      | FX                   | 64473.33
 BMW           | M6                   | 74583.10
 Saturn        | Aura                 | 94077.78
 Oldsmobile    | Silhouette           | 41962.28
 Daewoo        | Lanos                | 83250.58
 Subaru        | Forester             | 28593.87
 Ford          | Econoline E350       | 20043.51
 Ferrari       | 612 Scaglietti       | 14523.23
 Jeep          | Grand Cherokee       | 56399.60
 Honda         | Civic Si             | 70520.28
 Hyundai       | Accent               | 41019.17
 Isuzu         | Rodeo                | 11812.81
 Buick         | Electra              | 94782.37
 Buick         | Skyhawk              | 44523.51
...
```
</details>

<br>

command :
```SQL
SELECT make, SUM(price) FROM car GROUP BY make;
```
<details>
<summary>output :</summary>

```SQL
     make      |    sum
---------------+------------
 Ford          | 4603175.77
 Smart         |   58257.27
 Maserati      |  193260.79
 Dodge         | 2594682.79
 Infiniti      |  679515.74
 MINI          |  350316.16
 Jensen        |   24731.35
 Bentley       |  256902.09
 Citro├½n      |   17770.50
 Pontiac       | 2371857.32
 Plymouth      |  311295.40
 Holden        |   30883.67
 Audi          | 1426739.61
 Lexus         | 1186254.56
 Fairthorpe    |   41446.52
 Jeep          |  402241.00
...
...
```
</details>

<br>
<br>

___
### arithmetique operation 

command :
```SQL
SELECT 10 + 2;
```
output :
```SQL
 ?column?
----------
       12
(1 ligne)
```

possibility :
|operator|description|
|---|---|
|__``/``__|division|
|__``*``__|multiplication|
|__``+``__|addition|
|__``-``__|subtraction|
|__``^``__|power|
|__``!``__|factorial : 5! = 5 x 4 x 3 x 2 x 1|
|__``%``__|modulo : 5 % 2 = 1 because 5 / 2 + 1 = 0|

> **Note :** Factorial can be used for different purpose like calcul of 
>  * Binomial coefficient : $\binom{n}{k}$ = $\frac{n!}{k!(n-k)!}$ (calculation of number combinations) use for [statistics for example with Binomial distribution [FR]](https://fr.wikipedia.org/wiki/Loi_binomiale) [EN](https://en.wikipedia.org/wiki/Binomial_distribution) : 
> * You can find other usecase here : https://fr.wikipedia.org/wiki/Coefficient_binomial

<br>



<br>

command
```SQL
SELECT make, model, price, ROUND(price / 10, 2) FROM car;
```


<br>

output 
```SQL
     make      |        model         |  price   |  round
---------------+----------------------+----------+---------
 Mitsubishi    | Galant               | 45861.07 | 4586.11
 Pontiac       | Sunfire              | 20427.86 | 2042.79
 Nissan        | Maxima               | 74412.54 | 7441.25
 Saab          | 900                  | 53428.67 | 5342.87
 Honda         | S2000                | 94521.76 | 9452.18
 GMC           | Jimmy                | 69066.03 | 6906.60
 Ford          | F-Series             | 63790.81 | 6379.08
 GMC           | Sonoma               | 68905.72 | 6890.57
 Ford          | Taurus               | 56825.50 | 5682.55
 Subaru        | Outback              | 34886.05 | 3488.61
 Nissan        | Rogue                | 89159.57 | 8915.96
 Pontiac       | Gemini               | 26245.82 | 2624.58
 Hyundai       | Azera                | 86235.96 | 8623.60
...
...
```

<br>
<br>

___
### alias AS

command :
```SQL
SELECT make, model, price, round(price / 10, 2) AS ten_percent FROM car;
```
VS
```SQL
SELECT make, model, price, ROUND(price / 10, 2) FROM car;
```
<br>

<details>
<summary>output with AS keywords :</summary>

```SQL
     make      |        model         |  price   | ten_percent
---------------+----------------------+----------+-------------
 Mitsubishi    | Galant               | 45861.07 |     4586.11
 Pontiac       | Sunfire              | 20427.86 |     2042.79
 Nissan        | Maxima               | 74412.54 |     7441.25
 Saab          | 900                  | 53428.67 |     5342.87
 Honda         | S2000                | 94521.76 |     9452.18
 GMC           | Jimmy                | 69066.03 |     6906.60
 Ford          | F-Series             | 63790.81 |     6379.08
 GMC           | Sonoma               | 68905.72 |     6890.57
 Ford          | Taurus               | 56825.50 |     5682.55
 Subaru        | Outback              | 34886.05 |     3488.61
 Nissan        | Rogue                | 89159.57 |     8915.96
 Pontiac       | Gemini               | 26245.82 |     2624.58
 Hyundai       | Azera                | 86235.96 |     8623.60
...
```
</details>
VS
<details>
<summary>output without AS keywords :</summary>

```SQL
     make      |        model         |  price   |  round
---------------+----------------------+----------+---------
 Mitsubishi    | Galant               | 45861.07 | 4586.11
 Pontiac       | Sunfire              | 20427.86 | 2042.79
 Nissan        | Maxima               | 74412.54 | 7441.25
 Saab          | 900                  | 53428.67 | 5342.87
 Honda         | S2000                | 94521.76 | 9452.18
 GMC           | Jimmy                | 69066.03 | 6906.60
 Ford          | F-Series             | 63790.81 | 6379.08
 GMC           | Sonoma               | 68905.72 | 6890.57
 Ford          | Taurus               | 56825.50 | 5682.55
 Subaru        | Outback              | 34886.05 | 3488.61
 Nissan        | Rogue                | 89159.57 | 8915.96
 Pontiac       | Gemini               | 26245.82 | 2624.58
 Hyundai       | Azera                | 86235.96 | 8623.60
...
...
```
</details>




<br>
<br>

___
### COALESCE

return the first not null value and that's use for conditionnal sql queries

command :
```SQL
SELECT COALESCE(45, null, 11, 5, null, null, 12) AS number;
```
<details>
<summary>output :</summary>

```SQL
 number
--------
     45
(1 ligne)
```
</details>


<br>

Example : return email from person and replace __``NULL``__ value with __``Email not provided``__
```SQL
SELECT COALESCE(email, 'Email not provided') from person;

```
<details>
<summary>output :</summary>

```SQL
             coalesce
-----------------------------------
 xsprigging0@paginegialle.it
 aelcoux1@cbsnews.com
 mshillabear2@google.es
 Email not provided
 ofayre4@dyndns.org
 Email not provided
 eerington6@answers.com
 gdaice7@japanpost.jp
 Email not provided
 uvizor9@harvard.edu
 vmcfeea@soup.io
 qtavinorb@google.de
 Email not provided
 kanersend@sciencedaily.com
 aleile@github.io
 dmawmanf@constantcontact.com
...
...
```
</details>

<br>
<br>

___
### NULLIF

command
```SQL
SELECT NULLIF(0, 0);
```

return :
```
 nullif
--------

(1 ligne)
```

<br>

command
```SQL
SELECT NULLIF(5, 0);
```

return
```
 nullif
--------
5
(1 ligne)
```

> **Note :** so return the 1st argument if both arguments are different othertwise return NULL

<br>
<br>

#### Usecase handling error when divide by 0

When we want divide by 0 we will have trouble because we can't divide by 0 without throw an error 
but if we divide by null we havent an error and the result will be null so we can handle this usecase by coupling NULLIF with COALESCE 

command
```SQL
SELECT COALESCE(10/ NULLIF(0,0), 0);
```
<details>
<summary>output :</summary>

```
 coalesce
----------
        0
(1 ligne)
```
</details>

<br>

command
```SQL
SELECT COALESCE(10/ NULLIF(5,0), 0);
```
<details>
<summary>output :</summary>

```
 coalesce
----------
        2
(1 ligne)
```
</details>

___


<br>

___
### DATE 

The __``NOW()``__ function returns the current date and time. The return type of the __``NOW()``__ function is a timestamp with time zone. See the following example:

commande
```SQL
SELECT NOW();
```

substraction / add with date :
```
SELECT NOW() - INTERVAL'10 YEARS';
SELECT NOW() + INTERVAL'10 DAYS';
```

<br>

__``::``__ : cast operator so we cast the result of __``NOW``__ to an other format __``DATE``__
```SQL
SELECT NOW()::DATE;
```

<br>

command : with interval + cast
```SQL
SELECT (NOW() + INTERVAL'10 MONTHS')::DATE;
```

<details>
<summary>output :</summary>

```SQL
    date
------------
 2022-08-13
(1 ligne)
```
</details>

<br>

#### set a time zone 
```
SET TIMEZONE='America/Los_angeles';
```


<br>
<br>

___
## EXTRACT

WORK with date and time only

command :
```SQL
SELECT EXTRACT(YEAR FROM NOW());
```

return :
```SQL
 date_part
-----------
      2021
(1 ligne)
```

<br>

___


<br>

___
### AGE

perform calculation for age 
```SQL
AGE(NOW(), date_of_birth)
```

<br>

command :
```SQL
SELECT first_name, date_of_birth, AGE(NOW(), date_of_birth) AS age FROM person;
```
<details>
<summary>output :</summary>

```SQL
  first_name  | date_of_birth |               age
--------------+---------------+---------------------------------
 Xylia        | 2020-10-25    | 11 mons 19 days 13:57:40.945903
 Arlette      | 2020-11-02    | 11 mons 11 days 13:57:40.945903
 Maxim        | 2020-12-15    | 9 mons 29 days 13:57:40.945903
 Mannie       | 2021-07-12    | 3 mons 1 day 13:57:40.945903
 Orrin        | 2021-08-29    | 1 mon 15 days 13:57:40.945903
 Heindrick    | 2021-03-13    | 7 mons 13:57:40.945903
 Evelyn       | 2021-09-23    | 20 days 13:57:40.945903
 Gerhard      | 2021-10-11    | 2 days 13:57:40.945903
 Pietro       | 2020-11-02    | 11 mons 11 days 13:57:40.945903
 Udall        | 2021-07-28    | 2 mons 16 days 13:57:40.945903
 Virginie     | 2021-09-19    | 24 days 13:57:40.945903
 Quincey      | 2021-03-14    | 6 mons 30 days 13:57:40.945903
 Duffy        | 2021-01-31    | 8 mons 13 days 13:57:40.945903
 Kissee       | 2020-12-08    | 10 mons 5 days 13:57:40.945903
 Alaric       | 2020-12-09    | 10 mons 4 days 13:57:40.945903
 Doe          | 2020-11-07    | 11 mons 6 days 13:57:40.945903
 Atalanta     | 2021-04-07    | 6 mons 6 days 13:57:40.945903
 Wilfred      | 2021-10-06    | 7 days 13:57:40.945903
...
...
```
</details>


<br>

#### usecase extract month 
```SQL
SELECT first_name, date_of_birth, EXTRACT(MONTH FROM (AGE(NOW(), date_of_birth))) AS age_in_month FROM person;
```
<details>
<summary>output :</summary>

```SQL
  first_name  | date_of_birth | age_in_month
--------------+---------------+-----
 Xylia        | 2020-10-25    |  11
 Arlette      | 2020-11-02    |  11
 Maxim        | 2020-12-15    |   9
 Mannie       | 2021-07-12    |   3
 Orrin        | 2021-08-29    |   1
 Heindrick    | 2021-03-13    |   7
 Evelyn       | 2021-09-23    |   0
 Gerhard      | 2021-10-11    |   0
 Pietro       | 2020-11-02    |  11
 Udall        | 2021-07-28    |   2
 Virginie     | 2021-09-19    |   0
 Quincey      | 2021-03-14    |   6
 Duffy        | 2021-01-31    |   8
...
...
```
</details>


<br>
<br>

___
## PRIMARY KEYS / FOREIGN KEY & CONSTRAINT
### PK for PRIMARY KEY 

we can't have a duplicate value for primary key so if we try : 
```SQL
insert into car (id, make, model, price) values (1, 'Mitsubishi', 'Galant', '45861.07');
```

then we will have an error 
```
ERREUR:  la valeur d'une clé dupliquée rompt la contrainte unique « car_pkey »
DÉTAIL : La clé « (id)=(1) » existe déjà.
```

__CONCLUSION :__ primary key add a constraint to the data to be unique

<br>

this sequancies of queries :
```SQL
ALTER TABLE car DROP PRIMARY KEY(id);
insert into car (id, make, model, price) values (1, 'Mitsubishi', 'Galant', '45861.07');
ALTER TABLE car ADD PRIMARY KEY(id);
```

return :
```
throw an error because a value is duplicate
```

<br>

so we need to delete first the duplicate person 
```SQL
DELETE FROM person WHERE id = 1;
```

<br>

then after the command for add the primary key constraint will work 

<br>

___


<br>

___
### CONSTRAINT
#### ADD
add __``UNIQUE``__ constraint : 
```SQL
ALTER TABLE person ADD CONSTRAINT unique_email_address UNIQUE (email); 
```

<br>

#### DROP
```SQL
ALTER TABLE person DROP CONSTRAINT unique_email_address; 
```

<br>

After that if we check our table person with the command :
```shell
\d person
```

output :
```SQL
                                          Table ½ public.person ╗
    Colonne    |          Type          | Collationnement | NULL-able |             Par dÚfaut
---------------+------------------------+-----------------+-----------+------------------------------------
 id            | bigint                 |                 | not null  | nextval('person_id_seq'::regclass)
 first_name    | character varying(50)  |                 |           |
 last_name     | character varying(50)  |                 |           |
 email         | character varying(150) |                 |           |
 gender        | character varying(50)  |                 |           |
 date_of_birth | date                   |                 |           |
 country       | character varying(50)  |                 |           |
Index :
    "person_pkey" PRIMARY KEY, btree (id)
    "unique_email_address" UNIQUE CONSTRAINT, btree (email)
```

<br>

#### CHECK CONSTRAINT

add a constraint base on a boolean value

to add a condition on the possible values for the gender of a person here only Male or Female so in this case if we try to add a person with a gender that does not match one of the two possibilities then that will throw an error
```
ALTER TABLE person ADD CONSTRAINT gender_contraint CHECK (gender='Female' OR gender='Male');
```

<br>
<br>


___
### ON Conflict Do Nothing

handle on clonflict scenario

here handle duplicate key error :
command :
```SQL
insert into car (id, make, model, price) values (1, 'Mitsubishi', 'Galant', '45861.07');
```

return :
```SQL
ERREUR:  la valeur d'une clé dupliquée rompt la contrainte unique « car_pkey »
DÉTAIL : La clé « (id)=(1) » existe déjà.
```

<br>

so we will manage the case of conflict which is linked to the id which has a constraint to be unique:
and in this case the command becomes :

command :
```SQL
insert into car (id, make, model, price) values (1, 'Mitsubishi', 'Galant', '45861.07') 
ON CONFLICT (id) DO NOTHING;
```

return :
```
INSERT 0 0
```
Note : __``INSERT 0 0``__ mean __NO__ insert was performed because when an insert was properly perform we have __``INSERT 0 1``__

<br>

command :
```SQL
insert into car (id, make, model, price) values (1, 'Mitsubishi', 'Galant', '45861.07') 
ON CONFLICT (make) DO NOTHING;
```

return : will throw an error because no constraint on first_name column value
```SQL
ERREUR:  il n'existe aucune contrainte unique ou contrainte d'exclusion correspondant à la spécification ON CONFLICT
```

<br>

DO UPDATE instead of DO NOTHING :
* for example we realize a want to define again a line already existing and rather than DO NOTHING we want to UPDATE the mail of the person of which we make the INSERT with the new value of mail in this case

command :
```SQL
insert into person (id, first_name, last_name, email, gender, date_of_birth, country_of_birth) values ('1', 'Xylia', 'Sprigging', 'test@test.com', 'Polygender', '2020-10-25', 'Zimbabwe')
ON CONFLICT (id) DO UPDATE SET email = EXCLUDED.email;
```

<br>
<br>

scenario :

<details>
<summary>step 1</summary>

command :
```SQL
SELECT * FROM person WHERE id = 1;
```

return
```SQL
 id | first_name | last_name |            email            |   gender   | date_of_birth | country_of_birth
----+------------+-----------+-----------------------------+------------+---------------+----------
  1 | Xylia      | Sprigging | xsprigging0@paginegialle.it | Polygender | 2020-10-25    | Zimbabwe
(1 ligne)
```
<br>
</details>

<details>
<summary>step 2</summary>

command :
```SQL
insert into person (id, first_name, last_name, email, gender, date_of_birth, country_of_birth) values ('1', 'Xylia', 'Sprigging', 'test@test.com', 'Polygender', '2020-10-25', 'Zimbabwe')
ON CONFLICT (id) DO UPDATE SET email = EXCLUDED.email;
```

return :
```
INSERT 0 1
```
<br>
</details>

<details>
<summary>step 3</summary>

command :
```SQL
SELECT * FROM person WHERE id = 1;
```

return :
```SQL
 id | first_name | last_name |     email     |   gender   | date_of_birth | country_of_birth
----+------------+-----------+---------------+------------+---------------+----------
  1 | Xylia      | Sprigging | test@test.com | Polygender | 2020-10-25    | Zimbabwe
(1 ligne)
```
<br>
</details>


<br>
<br>

___


### FOREIGN KEY
FK is a column that references a PK

#### ADD a references with UPDATE

command :
```SQL
UPDATE person SET car_id = 1 WHERE id = 2;
```
return
```SQL
UPDATE 1
```

<br>

Now if we try to do it again then :
```SQL
UPDATE person SET car_id = 1 WHERE id = 2;
```
return an error because we have an __``UNIQUE CONSTRAINT``__

<br>

Then if we try to do :
```SQL
UPDATE person SET car_id = 1003 WHERE id = 2;
```
return an error because car_id 1003 doesnt exist

<br>
<br>

___

#### JOIN

|possibility|command|result C|
|---|---|---|
|JOIN|![image](postgresql_forma/INNERJOIN.png)|![image](postgresql_forma/INNERJOIN2.png)|
|LEFT JOIN|![image](postgresql_forma/LEFTJOIN.png)|![image](postgresql_forma/LEFTJOIN1.png)|

command JOIN:
```SQl
JOIN car ON person.car_id = car.id;
LEFT JOIN car ON person.car_id = car.id;
```

<br>

command LEFT JOIN:
```SQl
LEFT JOIN car ON person.car_id = car.id;
```


command :
```SQl
SELECT person.first_name, car.make
FROM person
JOIN car ON person.car_id = car.id;
```


<br>
<br>

___



#### DELETE ROW when we have a FOREIGN KEY

```SQL
DELETE FROM car WHERE id = 13;
```
> **Note :** if it's not working because we try to delete a car wich is already referenced by a person. So to be able to delete this car we need to update the person and put the car_id to NULL. So we are sur that the car is not referenced anymore. Then we can delete it


<br>
<br>

___

## EXPORT to CSV with postrgesql

we want to save the output of the following command inside a CSV file:
```SQL
SELECT * FROM person
LEFT JOIN car ON car.id = person.car_id;
```

<br>

for that postgresql have already a build in function so we search inside help with :
```
\? 
```

<br>

and finaly we use the following command to do it : 
```SQL
\copy (SELECT * person LEFT JOIN car ON car.id = person.car_id) TO '/path/file.csv' DELIMITER ',' CSV HEADER;
```

<br>
<br>

___

## SEQUENCES
```SQL
ALTER SEQUENCE person_id_seq RESTART WITH 9;
```



<br>
<br>

___


## EXTENSION

show available extension
```SQL
SELECT * FROM pg_available_extensions;
```

### UUID

```SQL
CREATE TABLE car (
	car_uid UUID NOT NULL PRIMARY KEY,
	make VARCHAR(100) NOT NULL,
	model VARCHAR(100) NOT NULL,
	price NUMERIC(19,2) NOT NULL CHECK (price > 0)
);
```

```SQL
CREATE TABLE person (
	person_uid UUID NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	email VARCHAR(150),
	gender VARCHAR(50) NOT NULL,
	date_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50),
    car_uid UUID REFERENCES car(car_uid),
    UNIQUE(car_uid),
    UNIQUE(email)
);
```

command :
```SQL
INSERT INTO person(person_uid, first_name, last_name, gender, email, date_of_birth, country of_birth)
VALUES (uuid_generate_v4(), 'John', 'Snow', 'Male', 'jonesnow@gmail.com', '281-10-23', 'Dorne')
```








<style>

blockquote {
  background-color: lightblue;
}

</style>

