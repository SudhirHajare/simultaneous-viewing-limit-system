# 👀 simultaneous-viewing-limit-system

Limit simultaneous viewing with Apache Kafka & Spring Boot Microservices, Redis

## Overview

### 📲 About OTT Service..

Most people use media services. Recently, users (especially because of the covid-19 😷 ) have been staying indoors, and usage has been increasing. In particular, it uses OTT services, which have subscription-based pricing services.

Therefore, there is a trend that users share their accounts and use them together. Are there any policy or technical issues that could arise from this?

If it is an account sharing service, there is an issue about simultaneous viewing restrictions. Then, let's implement this simultaneous viewing limit asynchronously and expandable from the basics through various micro-services! 👩🏻‍💻👨‍💻

---

## 📮 Why Redis?

In release server, there will be a lot of connections, and watch logs.

This system checks log with:

```text
key = set-top-box_id
hashkey = unique vod episode_id
```

for searching faster.

---

## 🧤 Why Kafka?

Micro services need to response each other async.

So, use Kafka as a Message Queue.

Produce topic, and Consume each topics that micro service need to subscribe.

---

## 📝 Before we start..

### Push service
(Spring Boot Micro service)

### Check service
(Spring Boot Micro service)

### Redis Cluster

- 3 master nodes
- 3 slave nodes

### Kafka Cluster

- 3 kafka cluster
- 1 zookeeper server

(Actually, 3 zookeeper server is proper)

All of these servers are executed by:

```text
localhost:portnumber
```

---

![System Architecture](image/system-architecture.png)

# System Requirements

- Java 8
- Apache Kafka 2.3.0
- Redis 6.0.x

---

# You can start Redis & Kafka cluster easily with these documents!

## Redis Cluster tutorial
😎 Start Redis cluster easily

## Kafka tutorial
🙄 Start Kafka easily

---

# Server ports

| Server | Port |
|--------|------|
| Push Service | 9002 |
| Check Service | 9000 |
| Redis Cluster | 7001 ~ 7005 |
| Kafka Cluster | 9093 ~ 9095 |
| Zookeeper | 2182 |

---

# Required Parameter

## Request Params for Start VOD

```json
{
  "pcid": "username",
  "episode_id": "episode_id",
  "stb_id": "stb_id",
  "play_start": "YYYY-mm-dd'T'HH:mm:ss",
  "mac_address": "mac_address",
  "running": true
}
```

---

## Request Params for Stop VOD

```json
{
  "pcid": "username",
  "episode_id": "episode_id",
  "stb_id": "stb_id",
  "play_start": "YYYY-mm-dd'T'HH:mm:ss",
  "play_end": "YYYY-mm-dd'T'HH:mm:ss",
  "mac_address": "mac_address",
  "running": false
}
```

---

# API Server

| METHOD | PATH | DESCRIPTION |
|--------|------|-------------|
| POST | :9002/send | Send request data for start watching VOD when new user connects |
| POST | :9002/force | Send request data for start watching VOD when another user exists |
| PATCH | :9002/stop | Send request data for stop watching VOD |
| POST | :9000/connect | Send request data for check connected user |

---

# WebSocket Queue

| PATH | DESCRIPTION |
|------|-------------|
| /queue/notify | send notify to user who subscribed each channel |
| /queue/disconnect | send disconnect notify to user who subscribed each channel |
| /queue/connect | send success response to user who subscribed each channel |

---

# Execute Spring boot application

## Execute with mvnw

```bash
./mvnw spring-boot:run
```

---

## Maven Packaging

```bash
mvn package
```

---

## Execute with jar file

```bash
java -jar target/push-service-0.0.1-SNAPSHOT.jar

java -jar target/check-service-0.0.1-SNAPSHOT.jar
```

---

# Conclusion

Now you can use this Viewing-Limit-System.
