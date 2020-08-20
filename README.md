# Deploy a high-availability web app using CloudFormation, Project Starter Code

The objective of this project is to deploy web servers for a highly available web app using CloudFormation.

<!--more-->

[//]: # (Image References)

[image1]: ./images/Diagrama_AWS.jpeg "AWS Diagram"
[image2]: ./images/image_event_datafile_new.jpg "Denormalized dataset"


---


#### How to run the program with your own code

For the execution of your own code, in the Visual Studio Code application, we open a terminal with powershell and run our CloudFormation script.

##### For Windows systems, we have the following scripts:

To create the stack from the project.
```bash
  .\Create_Stack.bat UdagramApp structure_network_server.yml structure_network_server.json
```

To update the stack from the project.
```bash
  .\Update_Stack.bat UdagramApp structure_network_server.yml structure_network_server.json
```

To delete the stack from the project.
```bash
  .\Delete_Stack.bat UdagramApp
```

##### For Linux systems, we have the following scripts:

To create the stack from the project.
```bash
  ./Create_Stack.sh UdagramApp structure_network_server.yml structure_network_server.json
```

To update the stack from the project.
```bash
  ./Update_Stack.sh UdagramApp structure_network_server.yml structure_network_server.json
```

To delete the stack from the project.
```bash
  ./Delete_Stack.sh UdagramApp
```


---

The summary of the files and folders within repo is provided in the table below:

| File/Folder              | Definition                                                                                                   |
| :----------------------- | :----------------------------------------------------------------------------------------------------------- |
| event_data/*             | Folder that contains all the csv files with the data used in this project.                                   |
| images/*                 | Folder containing the images of the project.                                                                 |
|                          |                                                                                                              |
| event_datafile_new.csv   | Contains the denormalized dataset that was generated in the ETL pipeline procedures.                         |
| Project_1B_ Project_Template.ipynb | Reads and processes the denormalized dataset and loads the data into the tables. This notebook contains detailed instructions on the ETL process, the data to be loaded in each of the three examples as well as their corresponding queries. |
|                          |                                                                                                              |
| README.md                | Contains the project documentation.                                                                          |
| README.pdf               | Contains the project documentation in PDF format.                                                            |


---

**Steps to complete the project:**

#### Problem.

1. Your company is creating an Instagram clone called Udagram. Developers pushed the latest version of their code in a zip file located in a public S3 Bucket.
2. You have been tasked with deploying the application, along with the necessary supporting software into its matching infrastructure.
3. This needs to be done in an automated fashion so that the infrastructure can be discarded as soon as the testing team finishes their tests and gathers their results.


## [Rubric Points](https://review.udacity.com/#!/rubrics/2556/view)
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
## Scenario.

In this project, we'll deploy web servers for a highly available web app using CloudFormation. We'll write the code that creates and deploys the infrastructure and application for an Instagram-like app from the ground up. We'll begin with deploying the networking components, followed by servers, security roles and software.


## AWS Diagram.

The diagram we have designed and will use for the Udagram application is as follows:

![alt text][image1]

## Apache Cassandra.

Aside from being a backbone for Facebook, Uber, and Netflix, Cassandra is a very scalable and resilient database that is easy to master and simple to configure. Apache Cassandra uses its own query language – CQL – which is similar to SQL. Note that JOINS, GROUP BY, or subqueries are not supported by CQL.

Some terms used in Cassandra differ from those we already know:

A keyspace, for example, is analogous to the term database in a relational database.

Another example is a partition, which is a collection of rows. Cassandra organizes data into partitions; there, each partition consists of multiple columns.

Partitions are stored on a node. Nodes (or servers) are generally part of a cluster where each node is responsible for a fraction of the partitions.

The Primary Key defines how each row can be uniquely identified and how the data is distributed across the nodes in our system. A partition key is responsible for identifying the partition or node in the cluster that stores a row – whereas the purpose of a clustering key (or clustering column) is to store row data within a partition in a sorted order.

When we have only one partition key and no clustering column, it is called a Single Primary Key. Should we use one (or more) partition key(s) and one (or more) clustering column(s) instead, we call it a Compound Primary Key or Composite Primary Key.


## Data.

The data used in this project, it's better to understand what they represents.

#### Song Dataset.

We'll be working with dataset: event_data. The directory of CSV files partitioned by date. For example, here are the file paths for this dataset.

```bash
  event_data/2018-11-01-events.csv
  event_data/2018-11-02-events.csv
  event_data/2018-11-03-events.csv
  .
  .
  event_data/2018-11-30-events.csv
```

These files are in CSV format and contains several records with the song data separated by a comma, below is an example of what a single song file, 2018-11-01-events.csv, looks like.

```bash
  Black Eyed Peas,Logged In,Sylvie,F,0,Cruz,214.93506,free,"Washington-Arlington-Alexandria, DC-VA-MD-WV",PUT,NextSong,1.54027E+12,9,Pump It,200,1.54111E+12,10
```

The code to pre-process the CSV files was provided already. So no need to go in-depth for it.


## ETL Pipeline.

Extract, transform, load (ETL) is the general procedure of copying data from one or more sources into a destination system which represents the data differently from, or in a different context than, the sources.

#### ETL Pipeline for Creating and Querying NoSQL Database.

We need to create a streamlined CSV file from all these. The final file will be used to extract and insert data into Apache Cassandra tables.

The event_datafile_new.csv has 6821 rows and contains the following columns:

* artist
* firstName of user
* gender of user
* item number in session
* last name of user
* length of the song
* level (paid or free song)
* location of the user
* sessionId
* song title
* userId

The image below is a screenshot of what the denormalized data should appear like in the event_datafile_new.csv after the code above is run:

![alt text][image2]

## Apache Cassandra Coding Portion.

We will model our data based on the queries provided to us by the analytics team at Sparkify. But first, let's setup Apache Cassandra for this. This is a three step process:

#### Create a Cluster.

We create a cluster and connect it to our local host. This makes a connection to a Cassandra instance on our local machine.

```bash
# This should make a connection to a Cassandra instance your local machine (127.0.0.1).
from cassandra.cluster import Cluster

try:
    # Connect to local Apache Cassandra instance.
    cluster = Cluster(['127.0.0.1'])
    # To establish connection and begin executing queries, need a session.
    session = cluster.connect()

except Exception as e:
    print(e)
```

#### Create a Keyspace.

```bash
# Create a keyspace called sparkify.
try:
    session.execute("""
        CREATE KEYSPACE IF NOT EXISTS sparkify
        WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1}"""
    )

except Exception as e:
    print(e)
```

#### Set Keyspace.

```bash
# Set KEYSPACE to the keyspace specified above.
try:
    session.set_keyspace("sparkify")

except Exception as e:
    print(e)
```


## Data Modeling.

In Apache Cassandra, we model our data based on the queries we will perform. Aggregation like GROUP BY, JOIN are highly discouraged in Cassandra. This is because we shouldn't scan the entire data because it is distributed on multiple nodes. It will slow down our system because sending all of that data from multiple nodes to a single machine will crash it.

Now we will create the tables to run the following queries:

1. Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4.

2. Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182.

3. Give me every user name (first and last) in my music app history who listened to the song "All Hands Against His Own".

To gain more technical detail about the coding portion please view the ETL notebook.


## Conclusion.

This project provides Sparkify startup customers with tools to analyze their data and help answer their key business questions, such as "Which artist and song was heard in a specified session," "Which artist, song and user was heard in a specified session," or "Which users heard a certain song".