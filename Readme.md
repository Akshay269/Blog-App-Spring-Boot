This project, by **Devtiro**, is a comprehensive **Blog Platform** built using **Spring Boot 3.4**, **Spring Security**, and **PostgreSQL**. It focuses on implementing **JWT (JSON Web Token) Authentication**, complex JPA relationships, and clean code principles using tools like **Lombok** and **MapStruct**.

### Project Analysis

* **Backend:** Java 21, Spring Boot 3.4.
* **Security:** Stateless JWT Authentication. Users must login to create, edit, or delete content, while reading posts, categories, and tags remains public.
* **Database:** PostgreSQL (production-style) via Docker Compose, and H2 for integration testing.
* **Key Design Patterns:**
* **Layered Architecture:** Controllers → Services → Repositories.
* **DTO Pattern:** Decoupling API contracts from database entities.
* **Mappers:** Using MapStruct to automate Entity-DTO conversion.
* **Global Exception Handling:** Centralized `@ControllerAdvice` to handle validation, authentication, and "Not Found" errors.


* **Advanced Features:**
* Calculating "Estimated Reading Time" on the fly during post creation.
* N+1 Query optimization using HQL `JOIN FETCH` for post counts in categories/tags.
* Bi-directional JPA relationships (One-to-Many, Many-to-Many).



---

### GitHub README.md

```markdown
# Spring Boot JWT Blog Platform

A full-stack blog platform featuring secure JWT authentication, content categorization, and a rich text editor integration. Built with a focus on modern Spring Boot best practices.

## 🚀 Features
- **JWT Authentication:** Secure login with stateless token-based sessions.
- **Post Management:** CRUD operations for blog posts (Draft vs. Published states).
- **Organization:** Categorize posts and add multiple tags.
- **Automatic Metadata:** Calculated "Reading Time" and audit timestamps.
- **Validation:** Strict input validation using Jakarta Validation.
- **Frontend Included:** Compatible with a React-based UI.

## 🛠️ Tech Stack
- **Backend:** Java 21, Spring Boot 3.4, Spring Security
- **Data:** Spring Data JPA, PostgreSQL, H2 (Testing)
- **Tools:** Lombok, MapStruct, Maven, Docker
- **Security:** JJWT (Java JWT Library)

## 🏗️ Getting Started

### 1. Database Setup
Ensure Docker is running and start the PostgreSQL database:
```bash
docker-compose up -d

```

### 2. Configuration

Update `src/main/resources/application.properties` with your JWT secret:

```properties
jwt.secret=your_very_secure_random_string_at_least_32_bytes

```

### 3. Build & Run

```bash
./mvnw spring-boot:run

```

---

## 📖 API Reference

### 🔐 Authentication

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/api/v1/auth/login` | Returns JWT and expiry for valid credentials. |

### 📝 Posts

| Method | Endpoint | Auth | Description |
| --- | --- | --- | --- |
| `GET` | `/api/v1/posts` | Public | List published posts (filter by `categoryId` or `tagId`). |
| `GET` | `/api/v1/posts/drafts` | Private | List draft posts for the authenticated user. |
| `POST` | `/api/v1/posts` | Private | Create a new post. |
| `GET` | `/api/v1/posts/{id}` | Public | Get post details by ID. |
| `PUT` | `/api/v1/posts/{id}` | Private | Update an existing post. |
| `DELETE` | `/api/v1/posts/{id}` | Private | Delete a post. |

### 📁 Categories & Tags

| Method | Endpoint | Auth | Description |
| --- | --- | --- | --- |
| `GET` | `/api/v1/categories` | Public | List all categories with post counts. |
| `POST` | `/api/v1/categories` | Private | Create a new category. |
| `DELETE` | `/api/v1/categories/{id}` | Private | Delete category (only if empty). |
| `GET` | `/api/v1/tags` | Public | List all tags with post counts. |
| `POST` | `/api/v1/tags` | Private | Bulk create tags. |

---

## 🧱 Data Model

* **User:** `id`, `email`, `password`, `name`, `createdAt`.
* **Post:** `id`, `title`, `content`, `status` (DRAFT/PUBLISHED), `readingTime`, `author_id`, `category_id`.
* **Category:** `id`, `name`.
* **Tag:** `id`, `name`.

```

---

### Full API Reference (Markdown)

#### 1. Authentication
**POST** `/api/v1/auth/login`
- **Body:** `{ "email": "user@test.com", "password": "password" }`
- **Response:** `200 OK`
```json
{
  "token": "eyJhbG...",
  "expiresIn": 86400
}

```

#### 2. Create Post

**POST** `/api/v1/posts`

* **Headers:** `Authorization: Bearer <token>`
* **Body:**

```json
{
  "title": "My New Blog",
  "content": "Full content here...",
  "categoryId": "uuid",
  "tagIds": ["uuid1", "uuid2"],
  "status": "PUBLISHED"
}

```

#### 3. Error Handling

The system returns a standard error object for all failures:

```json
{
  "status": 401,
  "message": "Incorrect username or password",
  "errors": []
}

```

### Video Source

Step-by-step build: [Build a Blog Platform with Spring Security](http://www.youtube.com/watch?v=Gd6AQsthXNY).
