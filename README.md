# KafkaStream-UseCase
A Data Pipeline demo on the use of Kafka Streams to simulate a use case on Confluence using sample data. 

 # Usecases
This project creates an environment setup used to carry out clickstream analysis from a Docker container.

#  Download and Start Confluent Platform Using Docker

Download or copy the contents of the Confluent Platform all-in-one Docker Compose file, for example:

curl --silent --output docker-compose.yml \
  https://raw.githubusercontent.com/confluentinc/cp-all-in-one/6.1.1-post/cp-all-in-one/docker-compose.yml
Start Confluent Platform with the -d option to run in detached mode:

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/Confluent_PlatformDownload.PNG)


docker-compose up -d
The above command starts Confluent Platform with a separate container for each Confluent Platform component. Your output should resemble the following:

![alt text](https://github.com/hsaberedo/KafkaStream-UseCase/blob/main/End-Of_Start_Confluent_Platform-docker-compose-up-d.PNG)
