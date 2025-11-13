# 🐳 Local Database Environment

A cross-platform Docker-based setup for local database development — includes **MySQL**, **PostgreSQL**, **MongoDB**, and **DynamoDB**, along with their WebUI

This setup provides an easy, consistent way to run multiple database engines locally without installing them directly on your system.

I'm personally using this setup since long and decided to share it online, so that anyone can use it, if it seems helpful.

## 🚀 Features

- Unified **Docker Compose** configuration
- Modular: start only the databases you need
- Works on **macOS**, **Linux**, and **Windows (via WSL or Git Bash)**
- Lightweight shell utility (`bin/db`) for common operations
- Persistent data volumes under `/data` (excluded from Git)
- Web UIs:
  - **DynamoDB Admin** → http://localhost:8001  
  - **Mongo Express** → http://localhost:8081 `Not usng 8080, commonly used by Tomcat (Java developers)`
  - **PHP MyAdmin** → http://localhost:8082
  - **PHP PG Admin** → http://localhost:8083 

## 🧩 Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (or Docker Engine)
- Bash shell (macOS, Linux, or WSL)
- Git (for cloning)

No local database installation required.

## 🧰 Setup

Clone the repo and make the script executable:

```bash
git clone https://github.com/kapilsharma/KAPsDB.git
cd DB
chmod +x bin/db
```

Your folder structure should look like:

```kotlin
KAPsDB/
├── bin/
│   └── db
├── data/
│   ├── mysql/
│   ├── postgres/
│   ├── mongo/
│   └── dynamodb/
├── docker-compose.yml
└── README.md
```

# ⚙️ Usage

## 🏁 Start Databases

### Start all:

```bash
./bin/db start
```

### Start specific database

```bash
./bin/db start mysql_dev
./bin/db start phpmyadmin        # Web UI for MySQL (http://localhost:8082)
./bin/db start postgres_dev
./bin/db start phppgadmin        # Web UI for Postgres (http://localhost:8083)
./bin/db start mongo_dev
./bin/db start mongo_express     # Web UI for MongoDB (http://localhost:8081)
./bin/db start dynamodb-local
./bin/db start dynamodb-admin.   # Web UI for DynamoDB (http://localhost:8001)
```

> **Note**
>
> Starting Web UI will start related DB container as well
> 
> **Example**: Starting phpmyadmin will also start mysql_dev.
> 
> Similarly, starting phppgadmin, mongo-express, and dynamodb-admin will start postgres_dev, mongo_dev, dynamodb-local respectively.

## 🛑 Stop Databases

```bash
./bin/db stop mysql_dev
./bin/db stopall      # stop all running containers
```

## Restart

```bash
./bin/db restart mongo_dev
```

## 📜 Logs

```bash
./bin/db logs mysql_dev
```

## 🧹 Clean Up (⚠️ removes volumes & data)

```bash
./bin/db clean
```

## 📊 Check Status

```bash
./bin/db status
```

# 🧱 Database Connection Details

| Service          | Host      | Port  | Username | Password         | Notes              |
|------------------|-----------|-------|----------|------------------|--------------------|
| MySQL.           | localhost | 3306  | root.    | mysecretpassword | DB: my_phpstorm_db |
| PhpMyAdmin.      | localhost | 8082. | root.    | mysecretpassword | Web GUI            |
| PostgreSQL       | localhost | 5432  | myuser   | mysecretpassword | DB: my_datagrip_db |
| PhpPGAdmin       | localhost | 8083  | myuser   | mysecretpassword | Web GUI            |
| MongoDB.         | localhost | 27017 | myuser.  | mysecretpassword |.                   |
| Mongo Express UI | localhost | 8081  | admin.   | password.        | Web GUI.           |
| DynamoDB Local.  | localhost | 8000  | (none).  | (none).          | Local-only         |
| DynamoDB Admin UI| localhost | 8001  | (none).  | (none)           | Web GUI.           |

# 🧩 Using in DataGrip / Other IDEs

I personally use DataGrip or JetBrains IDE like PHP Storm, Intelli JIdea, Web Storm, Py Charm, etc. If you are using some similar UI to check your database, bwelow are some useful setting to connect database, once started.

## MongoDB

- Type: MongoDB
- Host: localhost
- Port: 27017
- Auth DB: admin
- User: myuser
- Pass: mysecretpassword

## DynamoDB (via AWS Toolkit)

- Open AWS Explorer
- Add new connection
    - Service: DynamoDB
    - Region: us-east-1
    - Endpoint URL: http://localhost:8000
    - Access Key / Secret Key: any fake values
- Test connection ✅

# Data Persistence

All database files are stored under /data.
These folders are ignored in Git (see .gitignore) but persist locally across container restarts.

To reset everything:

```bash
./bin/db clean
```

# 👥 Team Usage Notes

- Works out-of-the-box after cloning — no setup steps required beyond Docker.
- First run will pull the needed images (only for services started).
- Supports partial service start (e.g., just MySQL or Mongo).
- To keep repo clean, all local data folders contain a .gitkeep file only.

# 📜 License

This setup is open for internal or educational use.
Feel free to modify and reuse it across your projects.

Officially KAPsDB is available using [MIT License](./LICENSE)

# 💬 Credits

Maintained by Kapil Sharma
Technical Architect — Smart Working Solutions

---

# 🧭 Future Database Roadmap

This environment currently includes the most common relational and NoSQL databases used in web development — **MySQL**, **PostgreSQL**, **MongoDB**, and **DynamoDB** — along with their respective admin UIs.

As part of ongoing learning and exploration, the following databases are planned for future inclusion:

| Category                  | Database          | Purpose  | Notes  |
|-------------------------- |-------------------|----------|--------|
| 🧠 **Cache / In-Memory**  | **Redis**         | High-performance caching, pub/sub messaging, and session storage. | Widely used in Laravel, Node.js, and microservices. |
| 🔍 **Search / Analytics** | **Elasticsearch** | Full-text search and analytics engine.                            | Ideal for logs, e-commerce search, and AI data indexing. |
| 🧱 **Wide-Column**        | **Cassandra**     | Distributed columnar database.                                    | Great for large-scale, fault-tolerant data storage. |
| 🕸️ **Graph**              | **Neo4j**         | Graph database for relationship-heavy data.                       | Perfect for social networks, recommendation systems, or knowledge graphs. |
| 📈 **Time-Series**        | **InfluxDB**      | Time-series database optimized for metrics and events.            | Useful for monitoring, IoT, and AI/ML performance tracking. |

> ⚙️ **Note:**  
> These databases are not yet part of the active `docker-compose.yml` or `bin/db` script.  
> They are included here as part of the long-term **Local Data Playground** vision — a single, extensible environment for exploring multiple database technologies under Docker.

### 🧱 Expansion Philosophy

This setup is designed to grow modularly:
- Each future database will be added as a **self-contained service block** in `docker-compose.yml`.  
- The `bin/db` script will automatically recognize and manage new containers without structural change.  
- Future additions will maintain **cross-platform support** (macOS, Linux, and Windows/WSL).

> The goal is to evolve this project into a **complete local database ecosystem** for experimentation, learning, and architecture prototyping.
