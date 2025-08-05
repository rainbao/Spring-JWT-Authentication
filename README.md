# Spring Boot JWT Authentication System

A complete JWT-based authentication system built with Spring Boot 3 and modern JavaScript, demonstrating stateless authentication best practices and secure API design.

## 🔑 Key Features

- **Stateless JWT Authentication** - Modern token-based authentication
- **Spring Security Integration** - Custom JWT filter with proper security configuration
- **Modular Frontend Architecture** - Clean separation of concerns with ES6 modules
- **Secure Token Management** - Configurable expiration and automatic cleanup
- **RESTful API Design** - Clean endpoints with proper HTTP status codes
- **PostgreSQL Integration** - Secure user storage with BCrypt password hashing

## 🏗️ Architecture Overview

### Backend (Spring Boot 3)
```
┌─────────────────┐    ┌──────────────┐    ┌─────────────────┐
│   JWT Filter    │────│ Auth Service │────│ User Repository │
│                 │    │              │    │                 │
│ Token Validation│    │ Login/Logout │    │   PostgreSQL    │
│ SecurityContext │    │ Registration │    │   User Storage  │
└─────────────────┘    └──────────────┘    └─────────────────┘
```

### Frontend (Vanilla JavaScript ES6)
```
┌─────────────────┐    ┌──────────────┐    ┌─────────────────┐
│  Auth Manager   │────│  API Client  │────│ Page Controllers│
│                 │    │              │    │                 │
│ Token Storage   │    │ HTTP Requests│    │ Login/Dashboard │
│ Auth State      │    │ Error Handle │    │ Event Handling  │
└─────────────────┘    └──────────────┘    └─────────────────┘
```

## 🚀 Quick Start

### Prerequisites
- Java 17+
- Maven 3.6+
- PostgreSQL 12+

### Environment Setup
Create environment variables or update `application.properties`:
```bash
DB_URL=jdbc:postgresql://localhost:5432/finapp
DB_USERNAME=your_db_user
DB_PASSWORD=your_db_password
JWT_SECRET=your-256-bit-secret-key
JWT_EXPIRATION=604800000
SECURITY_USER=admin
SECURITY_PASSWORD=admin_password
```

### Running the Application
```bash
# Clone the repository
git clone https://github.com/yourusername/spring-jwt-authentication.git
cd spring-jwt-authentication

# Build and run
mvn clean install
mvn spring-boot:run
```

Access the application at `http://localhost:8080`

## 🔐 Authentication Flow

### 1. User Registration/Login
```javascript
// Frontend login request
const response = await apiClient.login(username, password);
// Server returns JWT token
{ "token": "eyJhbGciOiJIUzI1NiIs..." }
```

### 2. Token Storage & Usage
```javascript
// Store token in sessionStorage
authManager.setAuth(token, username);

// Automatic header injection for API calls
headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
}
```

### 3. Server-Side Validation
```java
// JWT Filter extracts and validates tokens
@Component
public class JwtFilter extends OncePerRequestFilter {
    // Validates Bearer tokens from Authorization header
    // Sets Spring Security context for authenticated requests
}
```

## 📁 Project Structure

```
src/
├── main/
│   ├── java/com/rain/finapp/
│   │   ├── config/
│   │   │   └── SecurityConfig.java      # Spring Security configuration
│   │   ├── controller/
│   │   │   ├── AuthController.java      # Login/Register endpoints
│   │   │   └── UserController.java      # Protected user endpoints
│   │   ├── filter/
│   │   │   └── JwtFilter.java          # JWT validation filter
│   │   ├── service/
│   │   │   ├── AuthService.java        # Authentication business logic
│   │   │   └── UserService.java        # User management
│   │   ├── util/
│   │   │   └── JwtUtil.java           # JWT generation/validation
│   │   └── model/
│   │       └── User.java              # User entity
│   └── resources/
│       ├── static/
│       │   ├── js/
│       │   │   ├── auth.js            # Authentication manager
│       │   │   ├── api.js             # HTTP client
│       │   │   ├── login.js           # Login page controller
│       │   │   ├── dashboard.js       # Dashboard controller
│       │   │   └── utils.js           # UI utilities
│       │   ├── css/style.css          # Styling
│       │   ├── login.html            # Login page
│       │   ├── register.html         # Registration page
│       │   └── dashboard.html        # Protected dashboard
│       └── application.properties     # Configuration
```

## 🔧 Technical Implementation

### JWT Token Management
```java
// JwtUtil.java - Token generation with configurable expiration
public String generateToken(String username) {
    return Jwts.builder()
        .setSubject(username)
        .claim("username", username)
        .setIssuedAt(new Date(System.currentTimeMillis()))
        .setExpiration(new Date(System.currentTimeMillis() + jwtExpiration))
        .signWith(getSignInKey(), SignatureAlgorithm.HS256)
        .compact();
}
```

### Security Configuration
```java
// SecurityConfig.java - Stateless JWT configuration
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.sessionManagement()
        .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        .and()
        .addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class);
}
```

### Frontend Authentication Manager
```javascript
// auth.js - Clean token management
class AuthManager {
    setAuth(token, username) {
        sessionStorage.setItem('jwtToken', token);
        sessionStorage.setItem('username', username);
    }
    
    getAuthHeaders() {
        return {
            'Authorization': `Bearer ${this.getToken()}`,
            'Content-Type': 'application/json'
        };
    }
}
```

## 🛡️ Security Features

- **Stateless Authentication** - No server-side session storage
- **Configurable Token Expiration** - Customizable JWT lifetime
- **Secure Password Storage** - BCrypt hashing with salt rounds
- **CORS Configuration** - Proper cross-origin request handling
- **Input Validation** - Bean validation on all endpoints
- **Error Handling** - Proper HTTP status codes and error messages

## 📊 API Endpoints

### Authentication
- `POST /api/register` - User registration
- `POST /api/login` - User authentication (returns JWT)

### Protected Resources
- `GET /api/me` - Get current user information

### Example Usage
```bash
# Register new user
curl -X POST http://localhost:8080/api/register \
  -H "Content-Type: application/json" \
  -d '{"username":"john","email":"john@example.com","password":"password123"}'

# Login and get JWT token
curl -X POST http://localhost:8080/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"john","password":"password123"}'

# Access protected endpoint
curl -X GET http://localhost:8080/api/me \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIs..."
```

## 🧪 Testing

### Using Postman
1. **Register a user** via `POST /api/register`
2. **Login** via `POST /api/login` to get JWT token
3. **Copy the token** from the response
4. **Add Authorization header** for protected endpoints:
   ```
   Authorization: Bearer your-jwt-token-here
   ```

### Frontend Testing
1. Open `http://localhost:8080/login.html`
2. Register a new account or login with existing credentials
3. Navigate to dashboard to see protected content
4. Check browser DevTools → Application → Session Storage for token

## � Token Lifecycle

1. **Generation** - Server creates JWT on successful login
2. **Storage** - Client stores in sessionStorage (XSS-resistant)
3. **Transmission** - Sent as Bearer token in Authorization header
4. **Validation** - Server validates signature and expiration
5. **Expiration** - Automatic cleanup and redirect to login

## 🛠️ Development Features

- **Hot Reload** - Spring Boot DevTools for rapid development
- **SQL Logging** - Hibernate query logging for debugging
- **Error Handling** - Comprehensive exception handling
- **Modular Design** - Clean separation of concerns

## 📈 Performance Considerations

- **Stateless Design** - Horizontal scaling capability
- **Efficient Queries** - Optimized database interactions
- **Minimal Dependencies** - Lean Spring Boot configuration
- **Client-Side Routing** - Reduced server requests

## 🚧 Future Enhancements

- [ ] JWT Token Refresh mechanism
- [ ] Role-based authorization (RBAC)
- [ ] OAuth2 integration
- [ ] Rate limiting for auth endpoints
- [ ] Audit logging for security events

## 📝 Configuration

### JWT Settings
```properties
# JWT Configuration
jwt.secret=${JWT_SECRET}
jwt.expiration=604800000  # 7 days in milliseconds
```

### Database Configuration
```properties
# PostgreSQL Configuration
spring.datasource.url=${DB_URL}
spring.datasource.username=${DB_USERNAME}
spring.datasource.password=${DB_PASSWORD}
spring.jpa.hibernate.ddl-auto=update
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Built with ❤️ using Spring Boot 3, Spring Security, and modern JavaScript**
