# kafka-connect-examples

### Kafka Source Connector example (postgres to kafka topic, timestamp new inserts)
Create source connector : ````curl -i -X POST -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d "@postgres_source_connector.json"````

Delete source connector : ````curl -X DELETE http://localhost:8083/connectors/source-connector/````
  
