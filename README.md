# KafkaStream-UseCase
A Data Pipeline demo on the use of Kafka Streams to simulate a use case on Confluence using sample data. 

 # Business case
This project creates an environment setup used to carry out clickstream analysis from a Docker container.

Below are some of the information that can be extracted from the tables:

* Among others, One of the tables created, CLICK_USER_SESSIONS, shows a count of user activity for a given user session. The clickstream is used to simulates user sessions in a web connectivity.

* The demo also helps us to have information about web users, their cities, levels and location of registration.

* Can be used to determine the type and count of web events carried out by a user.

* Can be used to determine the responses received by a web user. 

All these are very useful information to understand user experiences about a web access.


#  Download and Start Confluent Platform Using Docker

Download or copy the contents of the Confluent Platform all-in-one Docker Compose file, for example:

curl --silent --output docker-compose.yml \
  https://raw.githubusercontent.com/confluentinc/cp-all-in-one/6.1.1-post/cp-all-in-one/docker-compose.yml
  
My Docker yaml file also included in the repository:
_**docker-compose.yaml**_


Start Confluent Platform with the -d option to run in detached mode:

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/Confluent_PlatformDownload.PNG)

Use the following commands and sequence to start the docker:
```
* Start the docker: sudo /etc/init.d/docker start

* Check status:     sudo /etc/init.d/docker status

* start docker-compose Confluent Platform: docker-compose up -d
```


The above command is used to start Confluent Platform with a separate container for each Confluent Platform component. The output is shown as follows:

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/End-Of_Start_Confluent_Platform-docker-compose-up-d.PNG)


To verify that the services are up and running, the following command was used:

docker-compose ps
The output is shown as follows:

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/Docker-Compose-ServicesCheck.PNG)



# Creating Kafka Topics

In this step, I created Kafka topics using Confluent Control Center. Confluent Control Center provides the functionality for building and monitoring production data pipelines and event streaming applications.

1. I Navigated to the Control Center web interface at http://localhost:9021.

NB: If Confluent Platform was installed on a different host, it is to replace localhost with the host name in the address.

It took take a minute or two for Control Center to come online.

2. Click **the controlcenter.cluster** tile.

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/KafkaTopicsControl_Center_Web_Interface.PNG)

3. In the navigation bar, click Topics to open the topics list, and then click Add a topic.

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/Control_Center_NewTopic.PNG)

4. In the Topic name field, I specified **pageviews** and clicked Create with defaults.

Note that topic names are case-sensitive.

5. In the navigation bar, click **Topics** to open the topics list, and then click **Add a topic**.

6. In the Topic name field, specify **users** and click **Create with defaults.**

# Installation of a Kafka Connector and Generating Sample Data

In this step, Kafka Connect was used to run a demo source connector called **kafka-connect-datagen** that creates sample data for the Kafka topics **clickstreams** and ** users**.

1. Run the first instance of the Kafka Connect Datagen connector to produce Kafka data to the clickstream topic in AVRO format.

a. In the navigation bar, click Connect.

b. Click the connect-default cluster in the Connect Clusters list.

c. Click Add connector.

d. Select the DatagenConnector tile.


e. In the Name field, enter datagen-clickstream as the name of the connector.

f. Enter the following configuration values:

* **Key converter class**: org.apache.kafka.connect.storage.StringConverter.
* **kafka.topic**: clickstream.
* **max.interval**: 100.
* **quickstart**: clickstream.

g. Click Continue.

h. Review the connector configuration and click Launch.

An example was the one done for users shown below. Similar was done for cliskstream connector.

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/users_genfile.PNG)



2. Run the second instance of the **Kafka Connect Datagen** connector to produce Kafka data to the _**users**_ topic in AVRO format.

a. In the navigation bar, click **Connect**.

b. Click the _**connect-default**_ cluster in the **Connect Clusters** list.

c. Click **Add connector**.

d. Select the _**DatagenConnector**_ tile.

e. In the **Name** field, enter _**datagen-users**_ as the name of the connector.

f. Enter the following configuration values:

* **Key converter class:** org.apache.kafka.connect.storage.StringConverter
* **kafka.topic:** users
* **max.interval:** 1000
* **quickstart:** users

g. Click **Continue**.

h. Review the connector configuration and click **Launch**.


The following other connectors where created with their equivalent datagens respectively:
* clickstream_codes:  datagen_clickstream_codes
* clickstream_users:  datagen_clickstream_users


#  Creating and Writing to a Stream and Table using ksqlDB

These were carried out using the ksqlDB web console and the CLi.
CLi used:
ksqlDB CLI from your Docker container with this command: _**docker-compose exec ksqldb-cli ksql http://ksqldb-server:8088**_.

**Create Streams and Tables**

The following streams were created:
* clickstream
* USER_CLICKSTREAM
* ENRICHED_ERROR_CODES
* USERS

And the following tables were created:
* USERS
* clickstream_codes
* WEB_USERS
* events_per_min
* pages_per_min

**NB: **
_**The streams and tables creation scripts are in file CREATE_CONNECTORS_STREAMS_AND_TABLES.txt in this repository.**_

And a sample step is selected to show how a stream and a table is created.


In this step, I used ksqlDB CLi to create a stream for the _clickstream_  topic.

1.Connect to **ksqlDB CLi** with this command: _**docker-compose exec ksqldb-cli ksql http://ksqldb-server:8088**_.

2. Paste the create topic for clickstream unto the CLi prompt and execute as below:

3. Copy the following code into the editor window and click **Run query** to create the CLICKSTREAM stream. Stream names are not case-sensitive.

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/ksql_create_stream_clicksteam.PNG)

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/ksql_create_stream_user_and_errorcodes.PNG)

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/ksql_create_stream_codes_webusers.PNG)


4. Copy the following code into the editor window and click **Run query** to create the USERS table using ksqlDB web console and CLi consoles. Table names are not case-sensitive.

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/create_table_users.PNG)

Other create table queries:

```
STREAM CODE LOOKUP TABLE:
=========================
CREATE TABLE clickstream_codes (
        code int primary key,
        definition varchar
    ) with (
        kafka_topic = 'clickstream_codes',
        value_format = 'AVRO'
    );

USERS LOOKUP TABLE:
=========================
CREATE TABLE WEB_USERS (
        user_id int primary key,
        registered_At BIGINT,
        username varchar,
        first_name varchar,
        last_name varchar,
        city varchar,
        level varchar
    ) with (
        kafka_topic = 'clickstream_users',
        value_format = 'AVRO'
    );
```


# Queries Execution

In this step, I executed several ksqlDB queries against my streams and tables using ksqlDB CLi console.

1. Connect to the ksqlDB CLi.

2. Results of the queries executed are shown as follows:

```
select * from users emit changes limit 3;

select * from clickstream emit changes limit 3;

select * from clickstream_codes emit changes limit 10;

select * from web_users emit changes limit 5;

select * from click_user_sessions emit changes limit 5;
```

![alt text](query_output_users_limit10.PNG)

![alt text](select_clickstreamcodes.PNG)

![alt text](select_clickusersessions.PNG)

![alt text](select_webusers.PNG)


The queries can also be executed using the ksqlDB web confluent web console as described below:

The setting instructs ksqlDB queries to read all available topic data from the beginning. This configuration was used for each subsequent query. 


* In the **Editor** tab, click **Add query properties** to add a custom query property.

* Set the _auto.offset.reset_ parameter to _Earliest_.
 
a. Click Stop to stop the current running query.

b. Create a non-persistent query that returns data from a stream with the results limited to a maximum of three rows:

c. Enter the same select queries listed above as executed in the CLi prompt.


# My Flow

![alt text](My_Flow.PNG)


# Conclusion

Apache Kafka and ksqlDB are rich and can be used to achieve different Data Pipeline usecases. 



  



