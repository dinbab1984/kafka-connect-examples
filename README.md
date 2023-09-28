# kafka-connect-examples

## Pre-requisites
### Set up Postgres, Kafka (includes kafka-connect)
#### Clone the git repository https://github.com/dinbab1984/docker-compose
#### Start Postgres and Kafka (includes kafka-connect) and  as described in https://github.com/dinbab1984/docker-compose#readme
#### Clone this git repository

#### Connect to postgres databae running in docker using the credentials as https://github.com/dinbab1984/docker-compose/blob/main/postgres.yml (hint : download and install pgadmin tool, connection properties hostname = localhost)

create a log table using the query as follows: 
````
CREATE TABLE log
(
    id uuid NOT NULL DEFAULT gen_random_uuid(),
    message character varying(100) NOT NULL,
    created_at timestamp without time zone NOT NULL DEFAULT now(),
    CONSTRAINT log_pkey PRIMARY KEY (id)
);
````

### Kafka Source Connector example (postgres to kafka topic auto create, timestamp new inserts)
Create source connector : ````curl -i -X POST -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d "@postgres_source_connector.json"````

Delete source connector : ````curl -X DELETE http://localhost:8083/connectors/source-connector/````

### Kafka Sink Connector example (kafka topic to postgres, table auto create & insert mode)
Create Sink connector : ````curl -i -X POST -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d "@postgres_sink_connector.json````

Delete sink connector : ````curl -X DELETE http://localhost:8083/connectors/sink-connector/```
