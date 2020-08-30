- This demo is based on confluent documentation in https://ksqldb.io/quickstart-platform.html#quickstart-content

- Start ksqlDB's server
  ```
  docker-compose up
  ```

- Start ksqlDB's interactive CLI
  ```
  docker exec -it ksqldb-cli ksql http://ksqldb-server:8088
  ```

- Create a stream
  ```
  CREATE STREAM riderLocations (profileId VARCHAR, latitude DOUBLE, longitude DOUBLE)
  WITH (kafka_topic='locations', value_format='json', partitions=1);
  ```

- Run a continuous query over the stream
  ```
  -- Mountain View lat, long: 37.4133, -122.1162
  SELECT * FROM riderLocations
  WHERE GEO_DISTANCE(latitude, longitude, 37.4133, -122.1162) <= 5 EMIT CHANGES;
  ```

- Start another CLI session
  ```
  docker exec -it ksqldb-cli ksql http://ksqldb-server:8088
  ```

- Populate the stream with events
  ```
  INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('c2309eec', 37.7877, -122.4205);
  INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('18f4ea86', 37.3903, -122.0643);
  INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4ab5cbad', 37.3952, -122.0813);
  INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('8b6eae59', 37.3944, -122.0813);
  INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4a7c7b41', 37.4049, -122.0822);
  INSERT INTO riderLocations (profileId, latitude, longitude) VALUES ('4ddad000', 37.7857, -122.4011);
  ```