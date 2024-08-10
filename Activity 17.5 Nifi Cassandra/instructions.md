In this activity, you will create a NiFi pipeline for a Cassandra database. It will allow you to write data from a Cassandra database into a MySQL database. You will begin by populating a Cassandra database, and then you will build a NiFi pipeline that will allow you to perform some basic ETL operations between the Cassandra and MySQL databases. Finally, you will check that the data has been correctly written into your MySQL database.

Before starting this activity, review the submission instructions below to ensure that you collect the required screenshots as you progress through the activity.

Note that this activity has been tested using a Windows OS and the Catalina version of a Mac OS. If you use the Big Sur OS, you are recommended to use the myPhpAdmin container as demonstrated in this article: Run MySQL & phpMyAdmin Locally Using Docker.Links to an external site.

Reference

Yuste, Miguel. “Run MySQL & PhpMyAdmin Locally in 3 Steps Using Docker.” Medium. 2019. https://migueldoctor.medium.com/run-mysql-phpmyadmin-locally-in-3-steps-using-docker-74eb735fa1fc.Links to an external site.

To complete this activity, follow these steps:

Create the NifiNetwork Docker network and the NiFi and Cassandra containers by following the steps in Mini-Lesson 17.6.

`docker network create NifiNetwork`

`docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2`

`docker run -p 9042:9042 --name some-cassandra --network NifiNetwork -d cassandra`

Provide a screenshot of your Docker desktop to show that you successfully created the NiFi and Cassandra containers in the Docker network.

From the Docker desktop, start a bash window for the Cassandra server. Run the cqlsh command to run queries against the Cassandra database.

Provide a screenshot of the bash window in the Cassandra server to show that you successfully ran the cqlsh command.

Copy the code below into the Cassandra bash shell to create a data table for the Cassandra database and insert “Peter Parker” into the “person” table:

CREATE KEYSPACE IF NOT EXISTS k1 WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '1'}  AND durable_writes = true;
use k1;
    CREATE TABLE person (
              id text,
    name text,
    surname text,
    PRIMARY KEY (id));
INSERT INTO person (id, name, surname) VALUES ('001', 'Peter', 'Parker');
Select * from person;

Provide a screenshot of the Terminal window to show that “Peter Parker” was inserted correctly into the Cassandra data table.

Navigate to http://localhost:8080/nifi to start the NiFi UI. Provide a screenshot to show that you successfully started the NiFi UI.

Create a process group and name it Cassandra-test. Provide a screenshot to show that you successfully created the Cassandra-test process group.

Inside the Cassandra-test process group, select the gear icon to configure the Cassandra-test process group:

Select the gear icon to configure the Cassandra-test process group.

In the CONTROLLER SERVICES tab, select the plus sign, select CassandraSessionProvider as the type, and then select the gear icon. In the PROPERTIES tab, set the Cassandra Contact Points field equal to some-cassandra:9042. Set the Client Auth field equal to NONE and the Keyspace field equal to k1.

Provide a screenshot to show that you correctly set the fields of the PROPERTIES tab.

Enable the CassandraSessionProvider controller. Provide a screenshot to show that the CassandraSessionProvider controller service was successfully activated.

Drop a processor onto the NiFi canvas and filter for “Cassandra”. Select the QueryCassandra type.

Configure the PROPERTIES tab as follows: set the Cassandra Connection Provider field equal to CassandraSessionProvider, the Client Auth field equal to NONE, the Keyspace field equal to k1, and the Output Format field equal to JSON, CQL select query to SELECT * FROM person;. Check failure and retry under Automatically terminate relationships under settings. 

Provide a screenshot to show that you correctly set the fields of the PROPERTIES tab.

Create a MySQL connector by running the following command in the Terminal window:

`​​docker run -p 3307:3306 --name mysql -e MYSQL_ROOT_PASSWORD=MyNewPass --network NifiNetwork -d mysql:8.0`

Next, open up the connection using MySQL Workbench and run the following commands to generate a person table in the people database by running the following queries:

DROP DATABASE IF EXISTS people;

CREATE DATABASE IF NOT EXISTS people;

USE  people;

CREATE TABLE person
(
    id varchar(50),
    name varchar(50),
    surname varchar(50),
    PRIMARY KEY (id)
    );
select * from person;

Provide a screenshot to show that the person table is empty within MySQL Workbench.

Connect the MySQL container to NiFi as demonstrated in Video 17.7 by using a DBCPConnectionPool connector. The driver must also be configured as demonstrated in Video 17.6. Provide a screenshot of the DBCPConnectionPool connector service activated within the NiFi UI.

Add a SplitJSON processor to the pipeline connected to the QueryCassandra processor. In the SETTINGS tab, select failure and original to Automatically Terminate Relationships. Configure the PROPERTIES tab by setting the JsonPath Expression equal to $.*.

Provide two screenshots. The first screenshot should show that you set the values in the SETTINGS tab correctly. The second screenshot should show that you updated the scheduling time in the PROPERTIES tab correctly.

Add a ConvertJSONToSQL processor and connect it to the SplitJSON processor with splits as the relationship type. In the SETTINGS tab, select failure and original to Automatically Terminate Relationships. In the PROPERTIES tab set the following fields:

Set the JDBC Connection Pool field equal to MySQL.
Set the Statement Type field equal to INSERT.
Set the Table Name field equal to person.
Set the Catalog Name field equal to people.
Set the Translate Field Names field equal to false.
Set the Output Format field equal to JSON. (There is no Output Format field though!)
Set the SQL Parameter Attribute Prefix field equal to SQL.
Provide a screenshot to show that you configured the PROPERTIES tab for the ConvertJSONToSQL processor correctly.

Connect the QueryCassandra, SplitJSON, and ConvertJSONToSQL processors. The QueryCassandra and SplitJSON processors will be connected by a success relationship. The SplitJSON and ConvertJSONToSQL processors will be connected by a splits relationship. Provide a screenshot of the connected QueryCassandra, SplitJSON, and ConvertJSONToSQL processors.

Add a PutSQL processor. In the SETTINGS tab, select failure, retry, and success to Automatically Terminate Relationships. In the PROPERTIES tab, set the JDBC Connection Pool field equal to MySQL.

Configuring the SETTINGS tab for the PutSQL processor.

Configuring the PROPERTIES tab for the PutSQL processor.

Connect the ConvertJSONToSQL and PutSQL processors by an sql relationship. Provide a screenshot to show that all four processors (QueryCassandra, SplitJSON, ConvertJSONToSQL, and PutSQL) are connected.

Run each processor and watch the data propagate through the pipeline. Finally, navigate back to the MySQL Workbench and run the following query:

SELECT * FROM person;

Provide a screenshot to show the data was added to your MySQL database correctly.