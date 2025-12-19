# Go TODO API - Clean Architecture & Robust Error Handling
Welcome to my first Go project! This is a high-performance TODO Management API built with the Gin Gonic framework. Despite being my first experience with Golang, I have focused on implementing industry-standard practices, including Layered Architecture (Repository-Service-Handler), centralized error management, and secure authentication.

## 🌟 Key Features
- **Layered Architecture**: Strict separation of concerns between Transports (Handlers), Business Logic (Services), and Data Access (Repositories).

- **RESTful API Design**: Standardized endpoints for Todo and Authentication management.

- **Centralized Error Handling**: Custom error types and a global middleware to ensure consistent API responses.

- **Authentication & Authorization:**

  - **JWT (JSON Web Token)**: Secure access using Access and Refresh tokens.

  - **Secure Password Hashing**: Utilizing bcrypt for user security.

- **Redis Integration:**

  - **Logout Mechanism**: Redis is used to manage and invalidate refresh tokens upon logout.

  - **OTP Management**: Designed for expiring OTP tokens (ready for expansion).

- **Database Management:** GORM for MySQL with Auto-migration support.

- **Standardized API Responses:** All responses follow a unified structure (Success/Error, Message, Data/Details).

## 🛠 Tech Stack
- **Language:** Go (Golang)

- **Framework:** Gin Gonic

- **ORM:** GORM (MySQL)

- **Cache:** Redis

- **Security:** JWT, Bcrypt

- **Configuration:** Dotenv

## 📂 Project Structure
```
.
├── cmd/
│   └── main.go           # Application entry point & Dependency Injection
├── internal/
│   ├── apperror/         # Custom error definitions (Business & HTTP)
│   ├── config/           # DB & Redis connection logic
│   ├── dtos/             # Data Transfer Objects (Requests/Responses)
│   ├── handlers/         # Controller layer (Request parsing)
│   ├── middlewares/      # JWT Auth & Error Handling middlewares
│   ├── models/           # Database Entities
│   ├── repositories/     # Data Access layer (GORM)
│   ├── routes/           # API Route definitions
│   ├── services/         # Business logic layer
│   └── utils/            # JWT helpers
└── .env                  # Environment variables
```
## ⚙️ Installation & Setup
### Prerequisites
- Go 1.21+

- MySQL

- Redis

### 1. Environment Configuration
Create a .env file in the root directory:

Đoạn mã
```
DB_USER=root
DB_PASSWORD=yourpassword
DB_HOST=127.0.0.1
DB_PORT=3306
DB_NAME=todo_db

REDIS_ADDR=localhost:6379
REDIS_PASSWORD=

JWT_SECRET=your_access_secret
JWT_REFRESH_SECRET=your_refresh_secret
SERVER_PORT=8080
```
### 2. Run the Project
```
Bash

# Install dependencies
go mod tidy

# Start the server
go run cmd/main.go
```
## 🔐 API Implementation Highlights
### Unified Response Format
Every request returns a consistent JSON object:

- **Success**: { "success": true, "message": "...", "data": { ... } }

- **Error**: { "success": false, "message": "...", "error": { "code": 401, "message": "..." } }

### Centralized Error Handling
Instead of handling errors in every handler, the project uses a Global Error Middleware. Handlers simply pass errors to the Gin context, and the middleware translates them into proper HTTP responses using the apperror package.

### Secure Authentication Flow
- **Login**: Returns an Access Token (short-lived) and a Refresh Token (long-lived).

- **Refresh**: Uses the Refresh Token to obtain a new Access Token without re-logging.

- **Logout**: Deletes the Refresh Token from Redis to prevent further use.

## Conclusion
This project represents my journey in learning Go's concurrency and strict typing while maintaining the architectural discipline required for production-grade APIs.
