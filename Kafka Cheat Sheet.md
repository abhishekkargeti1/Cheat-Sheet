#  												     **Kafka Cheat Sheet**





##### **Command to run  Zookeeper**



###### bin\\windows\\zookeeper-server-start.bat config\\zookeeper.properties



##### **Command to run  kafka server**



###### bin\\windows\\zookeeper-server-start.bat config\\zookeeper.properties



##### **Command to create kafka topic**



###### bin\\windows\\kafka-topics.bat --create --topic <topic name> --bootstrap-server localhost:9092



##### **Command to list the topic**



###### bin\\windows\\kafka-topics.bat --list --bootstrap-server localhost:9092



##### **Command to produce message in the topic**



###### bin\\windows\\kafka-console-producer.bat --topic myfirst-topic --bootstrap-server localhost:9092



##### **Command to read the event data**



###### bin\\windows\\kafka-console-consumer.bat --topic myfirst-topic --from-beginning --bootstrap-server localhost:9092

