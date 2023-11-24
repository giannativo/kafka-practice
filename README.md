# kafka-practice

This a basic test to run kafka broker locally, producing to and consuming from a topic through Python.

## Setup

1. Generate Zookeeper and Kafka containers

`docker run -d -p 2181:2181 ubuntu/zookeeper:edge`

Get docker inet address with ifconfig command

`docker run -d --name kafka-container -e TZ=UTC --network host -p 9092:9092 -e ZOOKEEPER_HOST=docker_inet_address ubuntu/kafka:3.1-22.04_beta`

2. Create purchases topic in kafka broker

`docker exec -it kafka-container /bin/bash`

`kafka-topics.sh --bootstrap-server localhost:9092 --create --topic purchases --partitions 20 --replication-factor 1`

3. Create virtualenv and install dependencies

`virtualenv env`
`source env/bin/activate`
`pip install -r requirements.txt`

4. Run scripts

`python producer.py getting_started.ini`

Expected output:

```
Produced event to topic purchases: key = htanaka      value = t-shirts    
Produced event to topic purchases: key = htanaka      value = gift card   
Produced event to topic purchases: key = htanaka      value = book        
Produced event to topic purchases: key = jsmith       value = alarm clock 
Produced event to topic purchases: key = jsmith       value = gift card   
Produced event to topic purchases: key = awalther     value = book        
Produced event to topic purchases: key = sgarcia      value = book        
Produced event to topic purchases: key = jbernard     value = book        
Produced event to topic purchases: key = jbernard     value = batteries   
Produced event to topic purchases: key = jbernard     value = gift card  
```

`python consumer.py getting_started.ini`

Expected output:

```
Waiting...
Waiting...
Consumed event from topic purchases: key = jsmith       value = alarm clock 
Consumed event from topic purchases: key = jsmith       value = gift card   
Consumed event from topic purchases: key = awalther     value = book        
Consumed event from topic purchases: key = sgarcia      value = book        
Consumed event from topic purchases: key = jbernard     value = book        
Consumed event from topic purchases: key = jbernard     value = batteries   
Consumed event from topic purchases: key = jbernard     value = gift card   
Consumed event from topic purchases: key = htanaka      value = t-shirts    
Consumed event from topic purchases: key = htanaka      value = gift card   
Consumed event from topic purchases: key = htanaka      value = book        
Waiting...
Waiting...
 
```