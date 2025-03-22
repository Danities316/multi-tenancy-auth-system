# TenantAuth - Multi-Tenancy Authentication System

A scalable, multi-tenant authentication system built with Node.js, supporting MongoDB, PostgreSQL, and MySQL databases with a database-per-tenant or shared-database strategy. Designed for THINKING 2025, it integrates Redis caching, Sequelize/Knex for database management, and lays the groundwork for passwordless auth, AI-driven security, and decentralized identity (DID).

## Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
  - [Creating a Tenant](#creating-a-tenant)
  - [Resolving a Tenant](#resolving-a-tenant)
  - [Testing with Postman](#testing-with-postman)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [License](#license)

## Features

- **Multi-Database Support**: MongoDB, PostgreSQL, and MySQL with dynamic adapter selection.
- **Tenant Isolation**: Configurable database-per-tenant or shared-database strategies.
- **Centralized Tenant Registry**: Tenant metadata stored in MongoDB, cached in Redis.
- **Security**: Password encryption with bcrypt, encrypted configs with AES-256.
- **Scalability**: Redis caching for fast tenant resolution.
- **Extensibility**: Hooks for passwordless auth (FIDO2), AI-driven tenant prediction (TensorFlow.js), and DID.
- **API-Driven**: RESTful endpoints for tenant creation and resolution.

## Architecture

- **Core**: Node.js with Express.js.
- **Databases**:
  - MongoDB: Tenant configurations (Tenant collection).
  - PostgreSQL/MySQL: Tenant-specific data (User, Roles tables).
- **Adapters**: Custom `DatabaseAdapter` classes (`MongoDBAdapter`, `PostgreSQLAdapter`, `MySQLAdapter`) using Knex.js (raw SQL) and Sequelize (ORM).
- **Middleware**: `resolveTenant` for tenant context resolution.
- **Caching**: Redis for tenant configs.
- **Security**: Helmet, rate-limiting, JWT authentication (planned).

  ```bash
  [Client] --> [API (/api/v1/auth/tenant)] --> [createTenant]
                                  |
  [Client] --> [API (/api/v1/test/tenant)] --> [resolveTenant]
                                  |
                           [MongoDB (Tenant Registry)]
                                  |
                         [Redis (Cached Configs)]
                                  |
                  [PostgreSQL/MySQL (Tenant DBs)]


## Prerequisites

- Node.js: v18.x or higher
- MongoDB: v5.x or higher
- PostgreSQL: v15.x or higher
- MySQL: v8.x or higher
- Redis: v7.x or higher
- Postman: For API testing

## Installation

1. **Clone the Repository:**

   ```bash
   git clone [https://github.com/yourusername/multi-tenant-auth-system.git](https://github.com/yourusername/multi-tenant-auth-system.git)
   cd multi-tenant-auth-system

2. **Install Dependencies:**
   ```bash
   npm install

3. **Set Up Databases:**
   - *MongoDB:* Start locally (mongod) or use a cloud instance.
   - *PostgreSQL:* Install and create a superuser database (superdb).
   ```bash
   psql -U postgres -c "CREATE DATABASE superdb;"
  - **MySQL:** Install and create a superuser database (supermysql).  
   ```bash
    mysql -u root -p -e "CREATE DATABASE supermysql;"

  - Redis: Start locally (redis-server).

## Configuration
 ```bash
1. Create a .env File:
   # General
     PORT=3000
     DB_STRATEGY=database-per-tenant
     ENCRYPTION_KEY=your-32-character-secret-key-here
     JWT_SECRET=your-jwt-secret
     DATABASE_URL=mongodb://localhost:27017/thinkingDB
     REDIS_URL=redis://localhost:6379

   # PostgreSQL
     POSTGRES_DB_HOST=localhost
     POSTGRES_DB_PORT=5432
     POSTGRES_DB_SUPERUSER=postgres
     POSTGRES_SUPERUSER_DB_PASSWORD=yourpostgrespassword
     POSTGRES_SUPERUSER_DB_NAME=superdb

   # MySQL
     DB_HOST=localhost
     DB_PORT=3306
     DB_SUPERUSER=mysqluser
     SUPERUSER_DB_PASSWORD=yourmysqlpassword
     SUPERUSER_DB_NAME=supermysql

2. Verify Environment:
   Ensure all services are running:
   ```bash
   mongo --version
   psql --version
   mysql --version
   redis-cli PING  # Should return "PONG"

## Usage
1. **Start the Server:**
   ```bash
   npm start
   Server runs on http://localhost:3000.

2. **Creating a Tenant**
- **Endpoint:** POST /api/v1/auth/tenant
- Payload (example for PostgreSQL):
  ```bash
  {
    "name": "PgTenant",
    "dbType": "postgresql",
    "host": "localhost",
    "username": "pgUser",
    "email": "pg@tenant.com",
    "password": "pgPass123",
    "database": "pgTenantDB"
  }

- **Response:**
  ```bash
  {
    "tenantId": "pgUser_<nanoid>",
    "name": "PgTenant",
    "dbType": "postgresql",
    "email": "pg@tenant.com",
    "host": "localhost",
    "dbStrategy": "database-per-tenant",
    "username": "pgUser",
    "password": "<hashed>",
    "database": "pgUser_<nanoid>"
  }
## Resolving a Tenant
- **Endpoint:** GET /api/v1/test/tenant
-  Headers:
   - x-tenant-id: pgUser_<nanoid> (from creation response)
-  **Response:**
   ```bash
    {
  "tenant": {
    "dbType": "postgresql",
    "host": "localhost",
    "username": "pgUser",
    "password": "<encrypted>",
    "database": "pgUser_<nanoid>",
    "port": "5432",
    "authMethods": ["password"],
    "theme": {},
    "didEnabled": false
  },
  "dbConnected": true
  }

## Testing with Postman
1. Import Collection:
  Copy this JSON into a .json file and import into Postman:
  ```bash
  {
  "info": {
    "name": "THINKING 2025 Tests",
    "schema": "[https://schema.getpostman.com/json/collection/v2.1.0/collection.json]    (https://schema.getpostman.com/json/collection/v2.1.0/collection.json)"
  },
  "item": [
    {
      "name": "Create PostgreSQL Tenant",
      "request": {
        "method": "POST",
        "header": [],
        "body": {
          "mode": "raw",
          "raw": "{\"name\": \"PgTenant\", \"dbType\": \"postgresql\", \"host\": \"localhost\", \"username\": \"pgUser\", \"email\": \"pg@tenant.com\", \"password\": \"pgPass123\", \"database\": \"pgTenantDB\"}",
          "options": { "raw": { "language": "json" } }
        },
        "url": "http://localhost:3000/api/v1/auth/tenant"
      }
    },
    {
      "name": "Resolve Tenant",
      "request": {
        "method": "GET",
        "header": [
          { "key": "x-tenant-id", "value": "pgUser_<nanoid>" }
        ],
        "url": "http://localhost:3000/api/v1/test/tenant"
      }
    }
  ]
}

2. Run Tests:
Update the x-tenant-id with the tenantId from the creation response.
Check console logs and database state for confirmation.

## Future Enhancements
- **Passwordless Authentication:** Integrate FIDO2/WebAuthn for loginTenant.
- AI-Driven Security: Use TensorFlow.js in resolveTenant to predict tenants without x-tenant-id.
- Decentralized Identity (DID): Add blockchain-based identity verification.
- Rate Limiting: Enhance express-rate-limit for tenant-specific quotas.
- Monitoring: Add logging with Winston or Prometheus.

## Contributing
1. Fork the repository.
2. Create a feature branch (git checkout -b feature/new-feature).
3. Commit changes (git commit -m "Add new feature").
4. Push to the branch (git push origin feature/new-feature).
5. Open a Pull Request.

License
This project is licensed under the MIT License. See LICENSE for details.


