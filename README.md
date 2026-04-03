# JWT Authentication System

A complete JWT-based authentication system built with **Spring Boot 3**, **Spring Security**, and **MySQL**.

## Features

- User Registration
- User Login with JWT Token
- Protected API Endpoints
- Get Current User Profile
- Stateless Authentication
- BCrypt Password Encryption

## Technology Stack

- **Java 17**
- **Spring Boot 3.2.0**
- **Spring Security 6**
- **Spring Data JPA**
- **MySQL Database**
- **JWT (jjwt 0.11.5)**
- **Lombok**
- **Maven**

## Project Structure

```
JWT Authentication System/
├── src/main/java/com/jwt/auth/
│   ├── JwtAuthApplication.java
│   ├── controller/
│   │   └── AuthController.java
│   ├── dto/
│   │   ├── AuthResponse.java
│   │   ├── LoginRequest.java
│   │   ├── RegisterRequest.java
│   │   └── UserResponse.java
│   ├── entity/
│   │   └── User.java
│   ├── repository/
│   │   └── UserRepository.java
│   ├── security/
│   │   ├── JwtFilter.java
│   │   ├── JwtUtil.java
│   │   ├── PasswordEncoderConfig.java
│   │   └── SecurityConfig.java
│   └── service/
│       └── UserService.java
├── src/main/resources/
│   └── application.properties
└── pom.xml
```

## Prerequisites

- Java 17 or higher
- Maven 3.6+
- MySQL 8.0+ (running locally)
- MySQL root password: `admin`

## Database Configuration

The application uses MySQL with the following configuration:

```properties
Database: jwt_auth_db
Username: root
Password: admin
URL: jdbc:mysql://localhost:3306/jwt_auth_db
```

The database will be created automatically if it doesn't exist.

## API Endpoints

| Endpoint | Method | Public | Description |
|----------|--------|--------|-------------|
| `/api/auth/register` | POST | Yes | Register a new user |
| `/api/auth/login` | POST | Yes | Login and get JWT token |
| `/api/auth/me` | GET | No | Get current user profile |

## Running the Application

### 1. Start MySQL Server
Ensure MySQL is running on your local machine.

### 2. Run the Application
```bash
mvn spring-boot:run
```

The server will start on `http://localhost:8080`

## Testing with Postman

### 1. Register a New User

**URL:** `POST http://localhost:8080/api/auth/register`

**Headers:**
```
Content-Type: application/json
```

**Body:**
```json
{
    "username": "john",
    "email": "john@example.com",
    "password": "password123"
}
```

**Response:**
```json
{
    "token": "eyJhbGciOiJIUzI1NiJ9...",
    "type": "Bearer",
    "user": {
        "id": 1,
        "username": "john",
        "email": "john@example.com",
        "role": "ROLE_USER"
    }
}
```

### 2. Login

**URL:** `POST http://localhost:8080/api/auth/login`

**Headers:**
```
Content-Type: application/json
```

**Body:**
```json
{
    "username": "john",
    "password": "password123"
}
```

**Response:**
```json
{
    "token": "eyJhbGciOiJIUzI1NiJ9...",
    "type": "Bearer",
    "user": {
        "id": 1,
        "username": "john",
        "email": "john@example.com",
        "role": "ROLE_USER"
    }
}
```

### 3. Access Protected Endpoint

**URL:** `GET http://localhost:8080/api/auth/me`

**Headers:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
```

**Response:**
```json
{
    "id": 1,
    "username": "john",
    "email": "john@example.com",
    "role": "ROLE_USER"
}
```

## Testing with cURL

### Register
```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"john","email":"john@example.com","password":"password123"}'
```

### Login
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"john","password":"password123"}'
```

### Get Profile
```bash
curl -X GET http://localhost:8080/api/auth/me \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

## Testing with PowerShell

### Register
```powershell
Invoke-RestMethod -Uri "http://localhost:8080/api/auth/register" -Method POST -ContentType "application/json" -Body '{"username":"john","email":"john@example.com","password":"password123"}'
```

### Login
```powershell
Invoke-RestMethod -Uri "http://localhost:8080/api/auth/login" -Method POST -ContentType "application/json" -Body '{"username":"john","password":"password123"}'
```

### Get Profile
```powershell
Invoke-RestMethod -Uri "http://localhost:8080/api/auth/me" -Headers @{"Authorization"="Bearer YOUR_JWT_TOKEN"}
```

## JWT Configuration

| Property | Value | Description |
|----------|-------|-------------|
| `jwt.secret` | `mySecretKey12345678901234567890123456789012` | Signing key |
| `jwt.expiration` | `86400000` | Token validity (24 hours in milliseconds) |

## Security Features

- **BCrypt Password Encoding** - Passwords are encrypted with BCrypt
- **JWT Stateless Authentication** - No server-side sessions
- **CSRF Disabled** - For stateless API architecture
- **Role-Based Access** - Default role: ROLE_USER
- **Token Expiration** - Tokens expire after 24 hours

## User Entity

| Field | Type | Constraints |
|-------|------|-------------|
| id | Long | Primary Key, Auto-generated |
| username | String | Unique, Not null |
| email | String | Unique, Not null |
| password | String | Not null, BCrypt encrypted |
| role | String | Default: ROLE_USER |

## Common Errors

### 403 Forbidden
- User doesn't exist (login before registration)
- Invalid JWT token
- Token expired

### 401 Unauthorized
- Missing Authorization header
- Invalid token format

## License

This project is open source and available under the MIT License.

## Author

Built as a learning project for JWT authentication with Spring Boot.
