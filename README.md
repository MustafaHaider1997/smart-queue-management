# 🧠 Smart Queue Management – Microservices Orchestration

The **Smart Queue Management** repository is the root orchestration project for the **Smart Queue System** – a cloud-native, scalable microservices-based appointment and notification platform for clinics.

This repository uses **Docker Compose** to coordinate the deployment and interaction of all individual services and includes a **lightweight Flask-based frontend** for end users.

---

## 📦 Included Microservices

| Service                            | Tech Stack                                  | Purpose                                                   |
| ---------------------------------- | ------------------------------------------- | --------------------------------------------------------- |
| `smart-queue-user-service`         | Python FastAPI + MongoDB (CosmosDB)         | Handles user registration, login, JWT auth                |
| `smart-queue-appointment-service`  | ASP.NET Core + PostgreSQL (Azure DB)        | Manages appointment CRUD, emits queue updates via Redis   |
| `smart-queue-queue-service`        | Python Script + Azure Redis                 | Real-time Pub/Sub messaging, simulates queue events       |
| `smart-queue-notification-service` | Python Flask + Azure Communication Services | Listens to Redis queue and sends email alerts             |
| `smart-queue-frontend`             | Flask + Bootstrap                           | Web interface to register, login, and manage appointments |

---

## 🗂️ Directory Structure

```
smart-queue-management/
├── docker-compose.yml
├── smart-queue-user-service/
├── smart-queue-appointment-service/
├── smart-queue-queue-service/
├── smart-queue-notification-service/
└── smart-queue-frontend/
```

---

## 🔗 Azure Resources Used

| Resource                             | Usage                        |
| ------------------------------------ | ---------------------------- |
| Azure CosmosDB (Mongo API)           | User Service database        |
| Azure PostgreSQL                     | Appointment Service database |
| Azure Redis Cache                    | Queue Service Pub/Sub        |
| Azure Communication Services (Email) | Notification Service alerts  |

---

## ⚙️ Prerequisites

* Docker Desktop
* Azure for Students account with necessary services
* Git (to clone sub-repos)

---

## 🧪 Environment Setup

Each service has its own `.env` or `appsettings.json`. Example environment variables:

### `.env` – User Service

```env
MONGO_URI=mongodb+srv://<...>.cosmos.azure.com/
JWT_SECRET=supersecretkey1234567890987654321!
ALGORITHM=HS256
```

### `.env` – Queue Service

```env
REDIS_HOST=smartqueueredis.redis.cache.windows.net
REDIS_PORT=6380
REDIS_PASSWORD=<your_password>
REDIS_USE_SSL=True
```

### `.env` – Notification Service

```env
REDIS_HOST=smartqueueredis.redis.cache.windows.net
...
AZURE_COMMUNICATION_CONNECTION_STRING=...
AZURE_SENDER_EMAIL=...
AZURE_TO_EMAIL=...
```

### `appsettings.json` – Appointment Service

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=...azure.com;Database=..."
  },
  "Jwt": {
    "Key": "supersecretkey1234567890987654321!"
  },
  "Redis": {
    ...
  }
}
```

---

## 🚀 Run the Full Stack Locally

Make sure Docker is installed and running. Then:

```bash
cd smart-queue-management
docker compose build
docker compose up
```

---

## 🔍 Service URLs

| Service             | URL                                                                                  |
| ------------------- | ------------------------------------------------------------------------------------ |
| **Frontend UI**     | [http://localhost:3001](http://localhost:3001)                                       |
| **User API**        | [http://localhost:8000/docs](http://localhost:8000/docs)                             |
| **Appointment API** | [http://localhost:5243/swagger/index.html](http://localhost:5243/swagger/index.html) |

Redis and email listeners run as background processes.

---

## 🧩 Inter-Service Communication

* **User → Auth Token** → Used in Frontend
* **Frontend → Appointment** → Authenticated appointment CRUD
* **Appointment → Redis Pub/Sub** → Sends queue events
* **Queue → Notification Service** → Sends email on queue update

---

## 🔐 Authentication Flow

1. User registers or logs in via `/register` or `/login` (User API).
2. JWT token is returned and stored in session (Frontend).
3. Token is attached to all Appointment API requests via `Authorization: Bearer`.

---

## 🛠️ Deployment Notes

* You can deploy each service independently in Kubernetes later.
* MongoDB/PostgreSQL/Redis/Email are all cloud-hosted (Azure).
* Each service has its own repo for CI/CD compatibility.

---

## 📎 Repositories

| Service                            | GitHub Repository Link                                                     |
| ---------------------------------- | -------------------------------------------------------------------------- |
| `smart-queue-user-service`         | [GitHub](https://github.com/YourUsername/smart-queue-user-service)         |
| `smart-queue-appointment-service`  | [GitHub](https://github.com/YourUsername/smart-queue-appointment-service)  |
| `smart-queue-queue-service`        | [GitHub](https://github.com/YourUsername/smart-queue-queue-service)        |
| `smart-queue-notification-service` | [GitHub](https://github.com/YourUsername/smart-queue-notification-service) |
| `smart-queue-frontend`             | [GitHub](https://github.com/YourUsername/smart-queue-frontend)             |

---

## 📄 License

This project is licensed under the MIT License.
