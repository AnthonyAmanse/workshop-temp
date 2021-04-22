# Introduction

In this workshop, you will deploy an example food delivery application that is using Kafka and OpenShift. The example application is utilizing a Kafka topic to produce and consume records of orders. The application has multiple microservices that consume and process these records/messages. The architecture below will explain the roles of these microservices. To scale them based on the incoming messages instead of the default Horizontal Pod Autoscaler \(HPA\) that uses CPU and memory thresholds, you will use KEDA or Kubernetes Event-Driven Autoscaler.

In this workshop, you will learn how to

* Deploy microservices with Kafka integration in OpenShift
* Use KEDA or Kubernetes Event-driven Autoscaler
* Scale these microservices based on the consumer lag

### Architecture

![](.gitbook/assets/image%20%281%29.png)



### Architecture Flow

1. The user starts the simulator from the frontend. The simulator sends requests to the API service to create orders. The API service is using an asynchronous REST method. Requests get a correlation ID.
2. The API service produces the message to the Kafka topic.
3. The order consumer picks up the message and processes it. This service is responsible for validating the transaction. This microservice can also produce messages to tha Kafka topic
4. The status consumer consumes the message when it sees that the transaction is created and validated by the order consumer.
5. The status consumer updates the Redis databas with the result of the transaction. This result is keyed with the correlation ID. The status microservice is responsible for updating the REST requests' correlation ID with the response. It's also responsible for serving the API service for it to fetch the responses.
6. The frontend then polls for a response from the API service using the correlation ID. The API service fetches it from the status microservice.
7. The restaurant microservice is subscribed to the Kafka topic so restaurants know when to start preparing the order. It also produces a message so that the courier consumer knows when to pick it up.
8. The courier microservice is subscribed so it gets notified when the order is ready for pick up. It also produces a message when the order is complete so that the order consumer can update the transaction in its database.
9. The realtime data is subscribed to the Kafka topic so it can serve the events in the simulator's graph. The pod data is providing the data for the number of pods of the consumer microservices to the frontend's architecture image.
10. KEDA will scale the number of pods of the consumer microservices when it reaches a certain consumer lag that is set in the deployment.

