# Spring-Security

# Spring Boot Security with JWT & Refresh Token


This project implements a complete authentication layer using Spring Boot, Spring Security, and JWT. It provides a practical blueprint for token-based authentication in modern backend services. The repository demonstrates how to integrate secure login flows, stateless authorization, and refresh-token rotation into a Java application.

The code includes configuration, domain models, custom JWT filters, and authorization logic that can be reused or extended in microservices or monolithic systems.

---

## Highlights & Techniques Used

### JWT Access + Refresh Token Workflow  
The application uses stateless authentication via JSON Web Tokens. Access tokens include user identity and roles in their claims. Refresh tokens allow clients to obtain new access tokens without re-authenticating.  
JWT basics: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization#bearer_token

### Custom Security Filter Chain  
A custom filter is added to the Spring Security filter chain. It:
- Extracts tokens from the `Authorization` header  
- Validates tokens and expiration  
- Loads user details dynamically  
- Populates authentication into `SecurityContextHolder`

Security authentication basics:  
https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication

### Role-Based Access Control  
Uses Spring Security’s built-in authorization at controller and method levels, including annotations like `@PreAuthorize`.

### Password Encoding with BCrypt  
Passwords are hashed using `BCryptPasswordEncoder`, which automatically salts and applies a cost factor for secure storage.

### Token Claims & Parsing  
Access and refresh tokens include subject, issuer, expiration timestamps, and custom metadata using standard cryptographic utilities.

### DTO-Based API Design  
The login and refresh endpoints use clearly defined DTOs for request and response structures, keeping the API clean and maintainable.

---

## Libraries and Tools of Interest

- **Spring Boot Security** — Authentication, authorization, filter chaining  
- **Spring Data JPA** — ORM and repository pattern  
- **JJWT (Java JWT Library)** — Token creation, validation, claims  
  https://github.com/jwtk/jjwt  
- **Lombok** — Reduces boilerplate in entities and DTOs  
  https://projectlombok.org/  
- **MySQL / H2** — Backend database for user persistence  
- **BCryptPasswordEncoder** — Secure password hashing algorithm
  
These tools form a production-grade security foundation for backend services.

---

## Project Structure

```bash
/
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── example/
│                   └── security/
│                       ├── config/
│                       ├── controller/
│                       ├── dto/
│                       ├── entity/
│                       ├── repository/
│                       ├── service/
│                       └── util/
└── pom.xml

```
## Architecture Diagram (JWT Flow)

```bash
+-----------+        Login Request        +------------------+
|   Client  | ---------------------------> | Auth Controller  |
+-----------+                              +------------------+
                                                |
                                                v
                                    +---------------------------+
                                    | Generate Access + Refresh |
                                    |       Tokens             |
                                    +---------------------------+
                                                |
                                                v
+-----------+   Protected Request w/ JWT    +------------------+
|   Client  | ----------------------------> | JWT Filter       |
+-----------+                                (Validates Token) |
                                                |
                                                v
                                        +---------------+
                                        | Controller    |
                                        |   Handler     |
                                        +---------------+
