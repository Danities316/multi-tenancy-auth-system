# Multi-Tenancy Authentication System

An open-source, multi-tenancy authentication system built with Node.js, designed to support MongoDB, PostgreSQL, and MySQL. This system provides tenant-specific user authentication, role-based access control (RBAC), and a modular architecture for extensibility.

---

## Features

- **Multi-Tenancy Support**:
  - Database-per-tenant or shared-database strategies.
  - Tenant isolation and configuration management.

- **Authentication**:
  - JWT-based stateless authentication.
  - Secure user registration and password management with bcrypt.

- **Role-Based Access Control (RBAC)**:
  - Tenant-specific roles and permissions.
  - Middleware for permission validation.

- **Database Compatibility**:
  - Supports MongoDB, PostgreSQL, and MySQL.
  - Dynamic database connection management.

- **Extensibility**:
  - Modular architecture for easy customization and scalability.
  - Easily add support for additional databases or authentication strategies.

---

## Getting Started

Follow these steps to set up the project locally.

### Prerequisites

Ensure you have the following installed:
- [Node.js](https://nodejs.org/) (v16 or higher)
- [Docker](https://www.docker.com/) (for optional containerized deployment)
- A running instance of MongoDB, PostgreSQL, or MySQL (can be local or hosted)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/Danities316/multi-tenancy-auth-system.git
   cd multi-tenancy-auth-system
2.  Install dependencies:
    ```bash
    npm install
3.  Set up your .env file:
    -  Copy the .env.example file and rename it to .env.
    -  Configure the environment variables for your setup.
4.  Run database migrations:
    ```bash
      npm run migrate
5.  Start the application:
    `npm start`

 ## 📌 Setting Up Your Database
 This open-source project supports **MongoDB, PostgreSQL, and MySQL**. You can configure your preferred database in the `.env` file.
 ### 1️⃣ **Choose Your Database**
 Edit `.env` and set your database type:
 ```bash
 DB_TYPE=postgres DB_HOST=localhost DB_USER=myuser DB_PASSWORD=mypassword DB_NAME=mydatabase
 ```bash
 ### 2️⃣ **Start the Server**
 Run: npm run dev

 ```bash
 ### 3️⃣ **Multi-Tenant Support**
 If you're running **multi-tenant mode**, send requests with an **`X-Tenant-ID`** header.
 **Example:**
 X-Tenant-ID: tenant123

npm run dev
    
##  API Documentation

Visit /api-docs after running the application to access the API documentation (powered by Swagger).

## Usage

### Example: Setting Up a Tenant
1.  Create a tenant using the /tenants endpoint.
2.  Use the X-Tenant-ID header or subdomain to interact with tenant-specific resources.

###  Example: User Authentication
-  Register a new user using the /auth/register endpoint.
-  Log in using /auth/login to receive a JWT.
-  Use the JWT for subsequent requests to authenticate and validate roles.

##  Project Structure
    ```bash
    
    ├── config/              # Configuration files (e.g., database, environment)
    ├── models/              # Database models for tenants, users, roles, etc.
    ├── adapters/            # Database adapters (MongoDB, PostgreSQL, MySQL)
    ├── services/            # Business logic for auth, tenants, RBAC
    ├── routes/              # API route definitions
    ├── middleware/          # Middleware (auth, tenant resolution, etc.)
    ├── utils/               # Utility functions and helpers
    ├── tests/               # Unit and integration tests
    ├── docs/                # API documentation and guides
    ├── .github/             # GitHub templates and workflows
    └── README.md            # Project README
---
##  Contributing
Contributions are welcome! Please follow these steps:
1.  Fork the repository.
2.  Create a new branch for your feature or fix:
     ```bash
    git checkout -b feature-name
4.  Commit your changes:
    ```bash
    git commit -m "Add your message here"
6. Push to your branch:
   ```bash
     git push origin feature-name
7.  Open a pull request.

For more details, see [CONTRIBUTING.md](https://github.com/Danities316/multi-tenancy-auth-system/edit/main/README).

---
## Roadmap

Here’s what’s planned for future releases:

Add support for additional databases (e.g., SQLite).
-  Implement multi-factor authentication (MFA).
-  Add email notifications for tenant management.
Check the [issues](https://github.com/Danities316/multi-tenancy-auth-system/edit/main/issue) page to see what’s in progress.
---
## License

This project is licensed under the [MIT License](https://github.com/Danities316/multi-tenancy-auth-system/edit/main/issue).
---
##  Acknowledgements
-  [Node.js](https://nodejs.org/en)
-  [Express.js](https://expressjs.com/)
-  [Knex.js](https://knexjs.org/)
-  [JWT](https://jwt.io/)

---
##  Feedback and Support
For feedback or support, please open an issue or start a discussion in the repository.

