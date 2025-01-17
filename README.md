## Built on the best distribution of Apache Cassandra™. DataStax Enterprise is the always-on data platform designed to allow you to effortlessly build and scale your apps, integrating graph, search, analytics, administration, developer tooling, and monitoring into a single unified platform.We power your apps' real-time moments so you can create instant insights and powerful customer experiences.  

## Tableau is an industy leading BI too that focuses on data visualization, dashboarding and data discovery.  It can be characterized as a "Visual Analytics" application - you don't just use it to visualize data, but instead you conduct analysis through seeing the data in visuals.  

## So why not utilize the power of Both DataStax Enterprise and Tableau together.

### In this simple tutorial we will walk through connecting Tableau to DataStax Enterprise (DSE) via Spark ODBC driver and creating a simple Tableau workbook. We will Utilize DSE analytics to perform the computations, joins and aggregates on the server side (DSE) which is faster and more effecient, while harnesing Tableau to visualize the data.

This Tutorial was created using Tableau Desktop 10.5 running on Windows 10, DSE 5.1.5 and ODBC Spark driver 1.2.2.1002

## Prerequisites

* A DataStax Academy Account to Download/Install DSE and Simba ODBC Driver for Apache Spark:  If you do not already have one, visit https://academy.datastax.com/
* A working DSE install running an Analytics workload.  
  * You can download DataStax Enterprise for free from https://academy.datastax.com/downloads and find installation instructions here http://docs.datastax.com/en/getting_started/doc/getting_started/installDSE.html 
  * Our Docker Images are hosted on [Docker Hub](https://hub.docker.com/r/datastax/dse-server/). For documentation including configuration options, environment variables, and compose examples head over to [DataStax Academy](https://academy.datastax.com/quick-downloads?utm_campaign=Docker_2019&utm_medium=web&utm_source=docker&utm_term=-&utm_content=Web_Academy_Downloads)
    * You will need to expose the public port 10000 to allow Tableau to connect to the Spark Trift Server along with Spark UI ports 7080, 7077, 4040, 9077
    

* A working Tableau Desktop install and basic Tableau understanding.  
  * You can download a free Tableau trial from https://www.tableau.com/products/trial 


## The Simba ODBC Driver for Apache Spark

* Download the Simba ODBC Driver for Apache Spark for your version of Microsoft Windows (32bit or 64bit) running Tableau Desktop by visiting https://academy.datastax.com/download-drivers
* Install the Simba ODBC Driver for Apache Spark.

## Demo Data

* Download the demo data to your DSE node from https://github.com/roberd13/DSE-Spark-with-Tableau/tree/master/DemoData
  * Run the cqlscript.sh script to create schemas and load the data. A Keyspace named **killr_video** with 2 tables **videos** and **videos_by_actor** will be created.

## Now lets Create a Connection to DSE in Tableau
<!--
* Start the Spark SQL Thriftserver on one of your DSE analytics nodes by running `dse spark-sql-thriftserver start`
* Open Tableau and create a connection to DSE by selecting Other Databases (ODBC) under To a Server.  You may need to click more to show this option
-->

* Open Tableau and create a connection to DSE by selecting Spark SQL under To a Server.  You may need to click more to show this option

* Setup your connection by 
  * Add the ipaddress of your DSE node running the Spark SQL Thrift Server 
  * Set Port to 10000
  * Set Type to SparkThriftServer
  * Authentication to username 
  * Click **Sign in** 

![](https://github.com/roberd13/tableau-dse6-demo/blob/master/images/sparksql_tableau.png)

<!-- ![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/connection.png) 
* When your workbook opens after Signing in. On the Data Source tab under connections.
  * Select **Spark** as the Database
  * Search Schema using the drop down and Select **Killr_video** 
  * Search Table using the drop down and your tables will be listed
  * Drag **videos and videos_by_actor** to the tables area 
  -->
* When your workbook opens after Signing in. On the Data Source tab under connections.
  * Search Schema using the drop down and Select **Killr_video** 
  * Search Table using the drop down and your tables will be listed
  * Drag **videos and videos_by_actor** to the tables area

<!-- 
![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/Data%20Source.png) 
-->
![](https://github.com/roberd13/tableau-dse6-demo/blob/master/images/Data%20Source.png)

  * Be sure to keep a Live connection, if you use Extract, all of the data will be loaded into Tableau.  
  * Create an inner join clause for Release Year = Release Year (Videos By Actor. (click on the blue part of the link between the 2 tables to be able to edit this inner join)

![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/Join%20clause.png)  

  * Click **Update Now** to see a sample of your data
  
![](https://github.com/roberd13/tableau-dse6-demo/blob/master/images/Updated%20Data.png)
<!-- 
![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/Updated%20Data.png) 
-->

## Now Lets See the Count of Movies grouped by Movie Ratings Per Year

* Click on Sheet 1 Tab at the bottom 
  * Convert Release Year (Videos By Actor) to Discrete
  
  ![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/discrete.png)
  
  * Drag Mpaa Rating to Color in the Marks section
   
  ![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/Marks.png)
  
  * Drag Release Year (Videos By Actor) to Columns
  * Drag Mpaa Rating to Rows
  * Change the Mpaa Rating under rows to a Count Measure type by using the drop down 
  
  ![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/measure.png)
  
  * Select Stacked Bar on the Show Me snap on 
  
  ![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/showme.png)
  
  * Click Show Me to hide the snap on 
  
* You now have a color coded graph with the Count of Movies grouped by Movie Ratings Per Year

![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/graph.png)

### If you are curious what Tableau did under the hood, you can visit http://anyanalyticsnode:7080 which will take you to the Spark Master UI, you can then click *org.apache.spark.sql.hive.thriftserver.HiveThriftServer2* which will direct you to the Thriftserver Application UI and you can see all of the queries performed during this tutorial.

### Master UI
![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/MasterUI.png)


### Thriftserver Application UI
![](https://github.com/roberd13/DSE-Spark-with-Tableau/blob/master/images/Thrift%20UI.png)
