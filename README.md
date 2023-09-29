# kafka-connect-examples

## Pre-requisites
### Set up Postgres, Kafka (includes kafka-connect)
Clone the git repository https://github.com/dinbab1984/docker-compose
Start Postgres and Kafka (includes kafka-connect) and  as described in https://github.com/dinbab1984/docker-compose#readme

Connect to postgres databae running in docker using the credentials as https://github.com/dinbab1984/docker-compose/blob/main/postgres.yml (hint : download and install suitable postgres sql client tool, connection properties hostname = localhost)

create a log table using the query as follows: 
````
CREATE TABLE log
(
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    message character varying(100) NOT NULL,
    created_at timestamp with time zone NOT NULL DEFAULT now(),
    CONSTRAINT log_pkey PRIMARY KEY (id)
);
````
Clone this git repository

### Kafka Source Connector example (postgres to kafka topic auto create, timestamp new inserts)
Create source connector : ````curl -i -X POST -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d "@postgres_source_connector.json"````

Delete source connector(only applicable if intended to recreate): ````curl -X DELETE http://localhost:8083/connectors/source-connector/````

### Kafka Sink Connector example (kafka topic to postgres, table auto create & insert mode)
Create Sink connector : ````curl -i -X POST -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d "@postgres_sink_connector.json````

Delete sink connector(only applicable if intended to recreate): ````curl -X DELETE http://localhost:8083/connectors/sink-connector/````

List all connectors: ````curl -X GET http://localhost:8083/connectors````

### Test the Kafka Connector by manually adding records to postgres log table as follows:
````
INSERT INTO log(message) VALUES('first log message');
INSERT INTO log(message) VALUES('second log message');
INSERT INTO log(message) VALUES('third log message');
INSERT INTO log(message) VALUES('fourth log message');
INSERT INTO log(message) VALUES('fifth log message');
````
There should be a table with name "kafka_log" with the same records from log table.  
If yes, then it proves the data movement as follows:  
postgres table(log) --via source connector--> auto created kafka topic(postgres_log) --via sink connector--> auto created postgres table(kafka_log)