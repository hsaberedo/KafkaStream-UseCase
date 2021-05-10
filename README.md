# KafkaStream-UseCase
A Data Pipeline demo on the use of Kafka Streams to simulate a use case on Confluence using sample data. 

 # Usecase
This project creates an environment setup used to carry out clickstream analysis from a Docker container.

#  Download and Start Confluent Platform Using Docker

Download or copy the contents of the Confluent Platform all-in-one Docker Compose file, for example:

curl --silent --output docker-compose.yml \
  https://raw.githubusercontent.com/confluentinc/cp-all-in-one/6.1.1-post/cp-all-in-one/docker-compose.yml
Start Confluent Platform with the -d option to run in detached mode:

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/Confluent_PlatformDownload.PNG)


docker-compose up -d
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

# Install a Kafka Connector and Generate Sample Data

In this step, Kafka Connect was used to run a demo source connector called **kafka-connect-datagen** that creates sample data for the Kafka topics **clickstreams** and ** users**.

1. Run the first instance of the Kafka Connect Datagen connector to produce Kafka data to the clickstreams topic in AVRO format.

a. In the navigation bar, click Connect.

b. Click the connect-default cluster in the Connect Clusters list.

c. Click Add connector.

d. Select the DatagenConnector tile.


e. In the Name field, enter datagen-pageviews as the name of the connector.

f. Enter the following configuration values:

* **Key converter class**: org.apache.kafka.connect.storage.StringConverter.
* **kafka.topic**: clickstream.
* **max.interval**: 100.
* **quickstart**: clickstream.

g. Click Continue.

h. Review the connector configuration and click Launch.


