# Library Management System (LMS)

A modern, scalable REST API for managing library operations including book inventory, borrowing records, and member management.

## Overview

This Library Management System provides a robust backend service built with Spring Boot 3.3 and PostgreSQL. It enables libraries to efficiently manage books, track borrowing activities, handle member information, and maintain library operations through intuitive REST endpoints.

### Key Features
- ✅ Complete book inventory management
- ✅ RESTful API with OpenAPI/Swagger documentation
- ✅ PostgreSQL database persistence
- ✅ Spring Data JPA for ORM
- ✅ Comprehensive error handling
- ✅ Spring Boot DevTools for development

---

## Quick Start

### Prerequisites
- **Java 17** or later
- **PostgreSQL 12+**
- **Maven 3.8+**

### Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/jakhar-ai-labs/lms.git
   cd lms
   ```

2. **Configure PostgreSQL:**
   ```bash
   # Create database
   createdb booksdb
   
   # Update credentials in application.properties if needed
   ```

3. **Update Database Configuration** (if using different credentials):
   ```properties
   # application.properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/booksdb
   spring.datasource.username=postgres
   spring.datasource.password=root
   ```

4. **Build the application:**
   ```bash
   ./mvnw clean install
   ```

5. **Run the application:**
   ```bash
   ./mvnw spring-boot:run
   ```

6. **Access the API:**
   - **Swagger UI:** http://localhost:8080/swagger-ui.html
   - **OpenAPI Docs:** http://localhost:8080/v3/api-docs
   - **Base URL:** http://localhost:8080/api

---

## Project Structure

```
lms/
├── src/
│   ├── main/
│   │   ├── java/com/lms/
│   │   │   ├── controller/          # REST API endpoints
│   │   │   │   └── LibraryController.java
│   │   │   ├── service/             # Business logic layer
│   │   │   │   ├── LibraryService.java
│   │   │   │   ├── LibraryServiceImpl.java
│   │   │   │   └── LibraryServiceDBImpl.java
│   │   │   ├── repository/          # Data access layer
│   │   │   │   └── BookRepository.java
│   │   │   ├── entity/              # Domain models
│   │   │   │   └── Book.java
│   │   │   ├── exception/           # Custom exceptions
│   │   │   │   └── ResourceNotFoundException.java
│   │   │   └── LmsApplication.java  # Main application class
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/com/lms/
│           └── LmsApplicationTests.java
├── pom.xml                          # Maven dependencies
├── mvnw                             # Maven wrapper (Linux/Mac)
└── README.md                        # This file
```

---

## API Endpoints

### Books Management

#### Get All Books
```
GET /api/books
```
**Response:** List of all books in the library

#### Get Book by ID
```
GET /api/books/{id}
```
**Response:** Single book details

#### Create New Book
```
POST /api/books
Content-Type: application/json

{
  "title": "String",
  "author": "String",
  "isbn": "String",
  "publicationYear": 2024,
  "availability": true,
  "quantity": 5
}
```

#### Update Book
```
PUT /api/books/{id}
Content-Type: application/json

{
  "title": "Updated Title",
  "availability": true,
  "quantity": 3
}
```

#### Delete Book
```
DELETE /api/books/{id}
```

#### Search Books
```
GET /api/books/search?keyword=java
```

---

## Technology Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| **Framework** | Spring Boot | 3.3.3 |
| **Language** | Java | 17 |
| **Database** | PostgreSQL | 12+ |
| **ORM** | Spring Data JPA | (via Spring Boot) |
| **API Docs** | SpringDoc OpenAPI | 2.6.0 |
| **Build Tool** | Maven | 3.8+ |
| **Development** | Spring DevTools | (via Spring Boot) |

---

## Database Schema

### Books Table
```sql
CREATE TABLE books (
  id SERIAL PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  author VARCHAR(255) NOT NULL,
  isbn VARCHAR(20) UNIQUE,
  publication_year INTEGER,
  availability BOOLEAN DEFAULT true,
  quantity INTEGER DEFAULT 1,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## Configuration

### Application Properties

**Development (default):**
```properties
# application.properties
spring.application.name=lms
spring.datasource.url=jdbc:postgresql://localhost:5432/booksdb
spring.datasource.username=postgres
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=create
logging.level.org.hibernate.SQL=DEBUG
```

**Production recommendations:**
```properties
# Use update instead of create
spring.jpa.hibernate.ddl-auto=update
# Reduce logging
logging.level.org.hibernate.SQL=INFO
# Enable connection pooling
spring.datasource.hikari.maximum-pool-size=20
```

---

## Service Layer

The application follows a **three-tier architecture:**

1. **Controller Layer** (`LibraryController`)
   - HTTP request handling
   - Request validation
   - Response formatting

2. **Service Layer** (`LibraryService`, `LibraryServiceImpl`)
   - Business logic implementation
   - Transactional boundaries
   - Data transformation

3. **Repository Layer** (`BookRepository`)
   - Database operations
   - Query methods
   - Data persistence

---

## Error Handling

### Custom Exceptions
- `ResourceNotFoundException` - Thrown when book/resource not found (HTTP 404)
- `InvalidRequestException` - Thrown for invalid input (HTTP 400)
- `DatabaseException` - Thrown for database operation failures

### Response Format
```json
{
  "status": "ERROR",
  "code": 404,
  "message": "Book not found",
  "timestamp": "2024-10-06T10:30:00Z"
}
```

---

## Development

### Logging Configuration

SQL query logging is enabled for development:
```properties
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
```

### IDE Setup
- **IntelliJ IDEA:** Spring Boot plugin recommended
- **VSCode:** Extension Pack for Java + Spring Boot Extension Pack
- **Eclipse:** STS (Spring Tools Suite)

---

## Testing

Run the test suite:
```bash
./mvnw test
```

Test files are located in `src/test/java/com/lms/`

---

## Maven Commands

```bash
# Clean and build
./mvnw clean install

# Run application
./mvnw spring-boot:run

# Run tests
./mvnw test

# Build JAR
./mvnw package

# View dependencies
./mvnw dependency:tree

# Format code
./mvnw spotless:apply
```

---

## API Documentation

Once the application is running, interactive API documentation is available:

- **Swagger UI:** http://localhost:8080/swagger-ui.html
- **OpenAPI JSON:** http://localhost:8080/v3/api-docs
- **OpenAPI YAML:** http://localhost:8080/v3/api-docs.yaml

---

## Troubleshooting

### Database Connection Issues
```
ERROR: FATAL: password authentication failed for user "postgres"
```
✅ Solution: Update credentials in `application.properties`

### Port Already in Use
```
Application failed to start with port 8080
```
✅ Solution: Change port in `application.properties`
```properties
server.port=8081
```

### JPA DDL Issues
```
ERROR: Table 'books' does not exist
```
✅ Solution: Ensure `spring.jpa.hibernate.ddl-auto=create` is set on first run

---

## Contributing

1. Create a feature branch: `git checkout -b feature/your-feature`
2. Commit changes: `git commit -am 'Add feature'`
3. Push to branch: `git push origin feature/your-feature`
4. Submit a pull request

---

## License

This project is open source and available under the MIT License.

---

## Contact & Support

- **Repository:** https://github.com/jakhar-ai-labs/lms
- **Issues:** Use GitHub Issues for bug reports and feature requests

---

## Future Enhancements

- [ ] User authentication and authorization
- [ ] Book borrowing/return tracking
- [ ] Member management module
- [ ] Fine calculation system
- [ ] Notification system for due dates
- [ ] Advanced search and filtering
- [ ] API rate limiting
- [ ] Caching layer (Redis)
- [ ] Docker containerization
- [ ] CI/CD pipeline

---

**Last Updated:** October 2024  
**Version:** 0.0.1-SNAPSHOT
