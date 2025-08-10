Here’s a **polished, streamlined version** of your README — still technical and informative, but leaner and more recruiter/interviewer-friendly. It removes excess repetition, simplifies some areas, and adds clarity while maintaining your strong structure.

---

# Spring Boot JWT Authentication API

A secure, modular JWT-based authentication system built with **Spring Boot 3**, **PostgreSQL**, and **modern JavaScript**, designed to demonstrate stateless auth best practices for REST APIs and SPAs.

---

## 🔐 Key Features

* **Stateless JWT Authentication** using Spring Security
* **Secure Token Management** with configurable expiration
* **BCrypt Password Hashing** for user credential security
* **Custom JWT Filter** for token validation & context setup
* **RESTful API Design** with proper status codes
* **Modular Frontend** with vanilla ES6 for clean integration

---

## 🧱 Architecture Overview

### 🔧 Backend (Spring Boot)

```
[JWT Filter] → [AuthService] → [UserRepository] → [PostgreSQL]
```

### 🖥️ Frontend (Vanilla JS)

```
[AuthManager] → [API Client] → [Page Controllers]
```

---

## 🚀 Quick Start

### Prerequisites

* Java 17+
* Maven 3.6+
* PostgreSQL 12+

### Configuration

Create environment variables or update `application.properties`:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/finapp
spring.datasource.username=your_user
spring.datasource.password=your_password

jwt.secret=your-256-bit-secret
jwt.expiration=604800000  # 7 days in milliseconds
```

### Run the App

```bash
git clone https://github.com/rainbao/spring-jwt-authentication.git
cd spring-jwt-authentication
mvn clean install
mvn spring-boot:run
```

---

## 🔄 Authentication Flow

1. **User Logs In**
   Frontend calls `/api/login` and receives a JWT on success.

2. **Token Storage (Demo)**
   Token is stored in `sessionStorage` (note: *in production, use HTTP-only cookies*).

3. **Protected API Access**
   JWT is sent via `Authorization: Bearer <token>` for secured endpoints.

4. **Server Validation**
   JWT is validated by a custom filter and Spring Security context is set.

---

## 📁 Key Project Structure

```
src/
├── controller/         # Auth + protected endpoints
├── filter/             # JwtFilter for validation
├── service/            # Business logic
├── util/               # JwtUtil for token handling
├── model/              # User entity
└── resources/static/   # Vanilla JS frontend
```

---

## 📊 API Endpoints

| Method | Endpoint        | Description                    |
| ------ | --------------- | ------------------------------ |
| POST   | `/api/register` | Register a new user            |
| POST   | `/api/login`    | Authenticate and receive JWT   |
| GET    | `/api/me`       | Get current user info (secure) |

---

## 🛡️ Security Practices

* ✅ **Stateless Authentication** via short-lived JWTs
* ✅ **HttpOnly Cookies Recommended** for real deployments
* ✅ **BCrypt Hashing** for secure password storage
* ✅ **CORS Configured** for safe cross-origin access
* ✅ **Input & Error Handling** for robustness

---

## 🧪 Testing

### Postman

1. Register via `POST /api/register`
2. Login via `POST /api/login`
3. Copy JWT and use it as:

   ```
   Authorization: Bearer <your-token>
   ```

### Frontend (Browser)

1. Open `login.html`
2. Login or register
3. Navigate to `dashboard.html` (protected)
4. Check token in sessionStorage (DevTools > Application)

---

## 🗂️ Future Enhancements

* [ ] Token refresh flow with secure HttpOnly cookie
* [ ] Role-based access control (RBAC)
* [ ] OAuth2 provider login (e.g., Google)
* [ ] Rate limiting + IP lockout on failed login
* [ ] Audit logging

---

## 📝 License

This project is licensed under the [MIT License](LICENSE).

---

**Built with ❤️ using Spring Boot 3, Spring Security, PostgreSQL, and modern JavaScript.**
