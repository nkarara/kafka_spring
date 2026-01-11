# Kafka Spring Cloud Streams Demo

This project is a comprehensive demonstration of building **Event-Driven Microservices** using **Spring Cloud Stream** and **Kafka Streams**.

## Architecture Overview

The system allows for decoupled event production, consumption, and stream processing. The implementation followed these steps:

1.  **Consumer**: Starting with a simple sink to log messages.
2.  **Producer**: Creating a source to generate traffic.
3.  **REST Integration**: Exposing web endpoints for interaction.
4.  **Stream Processing**: Implementing the business logic and analytics.

---

## Implementation Steps (Chronological)

### 1. The Consumer (Sink)
The first component implemented was the `pageEventConsumer`. It simply subscribes to topic **T2** and logs incoming messages. This allows us to verify that the basic Kafka connection is working.

<img width="569" height="117" alt="consumer" src="https://github.com/user-attachments/assets/8a9e3ad1-7714-4136-b08c-c2d48f15c608" />

### 2. The Producer (Source)
Next, we added the `pageEventSupplier` to act as a data source. It generates random `events.org.nabil.kafkaspringcloudstreams.PageEvent` objects.

<img width="569" height="117" alt="producer" src="https://github.com/user-attachments/assets/f660b06e-7126-487c-a639-eea136dd4690" />

### 3. REST Controller Integration
To interact with the system manually, the `PageEventController` was created. It uses `StreamBridge` to publish events on demand.

<img width="1512" height="253" alt="rest_ctrl_w_consumer" src="https://github.com/user-attachments/assets/6bd0e851-0270-447f-8f81-dda2bdabeedb" />

### 4. Supplier Configuration
The supplier was configured with a poller to trigger every second, ensuring a steady stream of data for testing.

<img width="751" height="316" alt="test_supplier_timer" src="https://github.com/user-attachments/assets/bf20d539-a93e-4b54-8605-2214e3511d41" />

### 5. Stream Processing Logic
The core logic resides in the `KStreamFunction`. It processes the input stream (T3), filters, groups, and windows the data to calculate real-time analytics.

<img width="1512" height="131" alt="kafka-stream" src="https://github.com/user-attachments/assets/56fbe0aa-b9bb-4910-abd6-58ad822365b8" />

### 6. Domain Object: events.org.nabil.kafkaspringcloudstreams.PageEvent
The data structure flowing through the system is the `events.org.nabil.kafkaspringcloudstreams.PageEvent` record.

<img width="752" height="499" alt="events" src="https://github.com/user-attachments/assets/3f741b0a-ec26-468c-b91a-9d011e976dd0" />

### 7. Real-Time Results
Finally, the results of the stream processing (T4) show the aggregation in real-time.

(<img width="752" height="499" alt="real-time-stream" src="https://github.com/user-attachments/assets/851444f0-9e13-4f0d-b610-95a3ea934ec8" />

---

## How to Run

### 1. Start Kafka Infrastructure
```bash
docker compose up -d
```
> **Ports**: External `localhost:9090`, Internal `broker:29092`.

### 2. Run the Application
```bash
./mvnw spring-boot:run
```

### 3. Verify & Interact

**Monitor Real-Time Output (T4)**:
```bash
docker exec -it kafka-broker kafka-console-consumer \
  --bootstrap-server broker:29092 \
  --topic T4 \
  --property print.key=true \
  --property print.value=true \
  --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
  --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```

**Live Analytics Dashboard**:
```bash
curl localhost:8080/analytics
```
