
---

# 📘 Blog Platform API

## Overview

A full-stack **blog platform backend** built with **Spring Boot**, **Spring Security**, **JWT authentication**, and **REST APIs**. This service supports user registration, login, role-based access control, and CRUD operations for blog posts and comments.

## Key Features

* User registration & authentication (JWT)
* Role-based authorization (USER, ADMIN)
* CRUD for Posts
* CRUD for Comments
* Secure endpoints with Spring Security
* Pagination and filtering support

---

## 🚀 Getting Started

### Prerequisites

* Java 17+
* Maven 3.6+
* PostgreSQL / MySQL
* (Optional) Docker & Docker-Compose

### Setup

1. Clone the repo:

   ```bash
   git clone https://github.com/<your_username>/blog-platform.git
   cd blog-platform
   ```
2. Create `.env` or configure `application.properties`:

   ```
   spring.datasource.url=jdbc:postgresql://localhost:5432/blogdb
   spring.datasource.username=postgres
   spring.datasource.password=yourpassword

   jwt.secret=YourJWTSecret
   jwt.expirationMs=86400000
   ```
3. Run:

   ```bash
   mvn spring-boot:run
   ```

---

## 🔐 Authentication

All protected endpoints require a **Bearer JWT token**.

```http
Authorization: Bearer <JWT_TOKEN>
```

---

## 📡 API Reference

---

### 🧑‍💻 Auth

#### **Register User**

```http
POST /api/auth/signup
Content-Type: application/json
```

**Request Body**

```json
{
  "username": "john",
  "email": "john@example.com",
  "password": "password123"
}
```

**Response**

```json
{
  "message": "User registered successfully"
}
```

---

#### **Login**

```http
POST /api/auth/login
Content-Type: application/json
```

**Request Body**

```json
{
  "username": "john",
  "password": "password123"
}
```

**Response**

```json
{
  "token": "JWT_TOKEN",
  "type": "Bearer",
  "user": "john",
  "roles": ["ROLE_USER"]
}
```

---

### 📝 Blog Posts

---

#### **Get All Posts**

```http
GET /api/posts
```

**Query Parameters (optional)**

| Param | Description |
| ----- | ----------- |
| page  | page number |
| size  | page size   |
| sort  | sort order  |

**Response**

```json
[
  {
    "id": 1,
    "title": "First Post",
    "content": "...",
    "author": "john",
    "createdAt": "2026-02-01T12:34:56Z"
  }
]
```

---

#### **Get Post by ID**

```http
GET /api/posts/{postId}
```

**Response**

```json
{
  "id": 1,
  "title": "First Post",
  "content": "...",
  "author": "john"
}
```

---

#### **Create a Post**

```http
POST /api/posts
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

**Request Body**

```json
{
  "title": "New Post",
  "content": "This is the content"
}
```

**Response**

```json
{
  "id": 5,
  "title": "New Post",
  "content": "This is the content",
  "author": "john"
}
```

---

#### **Update a Post**

```http
PUT /api/posts/{postId}
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

**Request Body**

```json
{
  "title": "Updated Title",
  "content": "Updated content"
}
```

**Response**

```json
{
  "id": 5,
  "title": "Updated Title"
}
```

---

#### **Delete a Post**

```http
DELETE /api/posts/{postId}
Authorization: Bearer <JWT_TOKEN>
```

**Response**

```json
{
  "message": "Post deleted successfully"
}
```

---

### 💬 Comments

---

#### **Get Comments for Post**

```http
GET /api/posts/{postId}/comments
```

**Response**

```json
[
  {
    "id": 10,
    "postId": 5,
    "content": "Great post!",
    "author": "alice"
  }
]
```

---

#### **Add Comment to Post**

```http
POST /api/posts/{postId}/comments
Authorization: Bearer <JWT_TOKEN>
```

**Request Body**

```json
{
  "content": "Nice article!"
}
```

**Response**

```json
{
  "id": 11,
  "postId": 5,
  "content": "Nice article!",
  "author": "john"
}
```

---

#### **Delete Comment**

```http
DELETE /api/posts/{postId}/comments/{commentId}
Authorization: Bearer <JWT_TOKEN>
```

**Response**

```json
{
  "message": "Comment removed"
}
```

---

## 🧠 Error Responses

| Status | Meaning      |
| ------ | ------------ |
| 400    | Bad Request  |
| 401    | Unauthorized |
| 403    | Forbidden    |
| 404    | Not Found    |
| 500    | Server Error |

---

## 🧪 Testing

Use **Postman** or **Swagger UI** (if integrated) to test endpoints.

---

## 📦 Deployment

Build with Maven:

```bash
mvn clean package
```

Deploy to any cloud (Heroku, AWS, GCP, Azure).

---

## 📁 Folder Structure (typical)

```
src/main/java/com/yourorg/blog
├─ controller
├─ service
├─ repository
├─ model
├─ security
└─ dto
```

---


