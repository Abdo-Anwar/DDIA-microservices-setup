# DDIA Microservices Setup – Polyglot Persistence & Performance Testing

This repository provides a **ready‑to‑use setup environment** for experimenting
with **data‑intensive microservices** as part of the
**Designing Data‑Intensive Applications (DDIA)** course.

The goal of this repository is to **remove setup friction** and allow students
and practitioners to focus on **storage design, caching strategies, and
performance testing**—rather than environment configuration.

> ⚠️ **Important**  
> This repository is **not a full lab solution**.  
> It provides infrastructure and wiring only.  
> The actual implementations (data models, caching logic, gRPC services, and
> performance evaluation) are intentionally left to the lab exercises.

---

## ✅ Quick Start (Setup & Run)

### 1️⃣ Prerequisites

Make sure the following are installed:

- Java **11+**
- Maven
- Docker & Docker Compose
- Git

---

### 2️⃣ Clone the Repository

```bash
git clone https://github.com/Abdo-Anwar/DDIA-microservices-setup.git
cd DDIA-microservices-setup
```

---

### 3️⃣ Start Databases (MySQL & MongoDB)

This lab uses **polyglot persistence**. Databases are started via Docker Compose:

```bash
docker-compose up -d
```

Containers started:

- **MySQL** → intended for the Ratings service
- **MongoDB** → intended for caching movie data

Verify:

```bash
docker ps
```

---

### 4️⃣ Run the Microservices

Start services **in the following order**.

#### Discovery Server

```bash
cd discovery-server
mvn spring-boot:run
```

#### Remaining Services (each in a separate terminal)

```bash
cd movie-info-service
mvn spring-boot:run
```

```bash
cd ratings-data-service
mvn spring-boot:run
```

```bash
cd movie-catalog-service
mvn spring-boot:run
```

---

### 5️⃣ Service Endpoints

| Service                 | URL                                      |
| ----------------------- | ---------------------------------------- |
| Eureka Discovery Server | http://localhost:8761                    |
| Movie Catalog Service   | http://localhost:8081/catalog/{userId}   |
| Movie Info Service      | http://localhost:8082/movies/{movieId}   |
| Ratings Data Service    | http://localhost:8083/ratings/{userId}   |

---

## 🧠 Original Project Architecture

The original project follows a **microservices‑based architecture**
for a simple movie catalog application.

### Core Microservices

- **MovieInfoService**
  - Fetches movie metadata from an external movie database API.

- **RatingsDataService**
  - Provides user ratings for movies.

- **MovieCatalogService**
  - Aggregates data from MovieInfoService and RatingsDataService.

- **DiscoveryServer**
  - Eureka service registry enabling service discovery and loose coupling.

> The architectural design emphasizes **independent deployability**, a key
> principle in building scalable data‑intensive systems.

![Screen Shot 2021-09-23 at 16 48 57](https://user-images.githubusercontent.com/22833948/134519062-0013cbf9-8a5f-4a43-ba14-635ccdbab04b.png)

---

## 🧪 Lab Focus & Extensions

This repository does **not** change the business logic of the services.
Instead, it supports DDIA lab experiments such as:

- Polyglot persistence (different data models per service)
- Caching and read optimization
- Microservices performance analysis
- Latency & throughput evaluation under load

Design decisions and implementations are intentionally left for students
to explore as part of the lab.

---

## 💾 Storage Design Rationale (DDIA Perspective)

Suggested (not enforced) directions:

- **Ratings Service → MySQL**
  - Structured, transactional data
  - Consistency‑oriented access patterns

- **Movie Info Cache → MongoDB**
  - Read‑heavy workload
  - Reduced external API calls
  - Flexible schema for cached documents

These choices align with DDIA principles of
**choosing data models based on access patterns**.

---

## 🚀 Performance Testing with Apache JMeter

### 1️⃣ Download JMeter

Download Apache JMeter from the official website:

👉 https://jmeter.apache.org/download_jmeter.cgi

Choose the **Binary (.zip)** package.

---

### 2️⃣ Extract & Run JMeter

#### Linux / macOS

```bash
unzip apache-jmeter-*.zip
cd apache-jmeter-*/bin
./jmeter
```

#### Windows

1. Extract the ZIP
2. Navigate to: `apache-jmeter\bin`
3. Run: `jmeter.bat`

---

### 3️⃣ Suggested Tests

Create two test plans:

#### ✅ Performance Test

- Moderate load
- Measure:
  - Average latency
  - P90 latency
  - Throughput

#### ✅ Stress Test

- Gradually increase users
- Observe:
  - Failure point
  - Maximum sustainable throughput

Focus endpoint:

```
GET http://localhost:8082/movies/{movieId}
```

Compare results **before and after adding caching**.

---

## 📦 Repository Structure

```
DDIA-microservices-setup/
│
├── docker-compose.yml
├── discovery-server/
├── movie-info-service/
├── ratings-data-service/
├── movie-catalog-service/
├── performance-tests/
│   └── notes.md
├── images/
│   └── original-architecture.png
├── README.md
└── LICENSE
```

---

## 🎯 Learning Objectives

This setup enables hands‑on exploration of:

- Microservices and service discovery
- Polyglot persistence
- Caching strategies
- Performance vs. stress testing
- Trade‑offs in data‑intensive system design

---

## 📌 Acknowledgements

This setup is based on the original open‑source project:

👉 https://github.com/yigiterinc/spring-boot-microservices

Adapted and extended for **educational and experimentation purposes**
within the DDIA course context.

---

## 👤 Author

**Abdelrhman Anwar**  
Data Engineer

GitHub: https://github.com/Abdo-Anwar  
LinkedIn: https://www.linkedin.com/in/abdelrhman-anwar

---
