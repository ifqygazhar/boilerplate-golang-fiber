# Boilerplate Golang Fiber MySQL

A boilerplate project for building REST APIs using Go Fiber framework with MySQL database and GORM ORM. This project follows clean architecture principles with clear separation of concerns.

## 🚀 Features

- ⚡ **Go Fiber v2** - Fast and expressive web framework
- 🗄️ **MySQL + GORM** - Database with powerful ORM
- 🏗️ **Clean Architecture** - Well-organized code structure
- 🔐 **JWT Support** - Ready for JWT authentication
- 🌍 **CORS Enabled** - CORS configuration for web applications
- 🚦 **Rate Limiting** - Request limiting for security
- 🕒 **Timezone Support** - Asia/Jakarta timezone configuration
- 📝 **Database Migration** - Tools for managing database schema
- 🔄 **Environment-based Config** - Multiple environment support
- 📦 **Response Standardization** - Consistent API response format

## 📁 Project Structure

```
.
├── cmd/
│   └── api/
│       └── main.go              # Application entry point
├── internal/
│   ├── handler/
│   │   └── handler.go           # HTTP handlers
│   ├── middleware/
│   │   └── middleware.go        # Custom middlewares
│   ├── model/
│   │   └── model.go             # Data models
│   ├── repository/
│   │   └── repository.go        # Database operations
│   └── service/
│       └── service.go           # Business logic
├── pkg/
│   ├── database/
│   │   ├── mysql.go             # Database connection
│   │   └── migrations/
│   │       └── migration.go     # Migration utilities
│   └── utils/
│       └── response.go          # Response helpers
├── .env.example                 # Environment variables template
├── .gitignore                   # Git ignore rules
├── go.mod                       # Go dependencies
├── go.sum                       # Go dependencies checksum
└── MakeFile                     # Migration commands
```

## 🛠️ Tech Stack

- **Go** 1.25.5
- **Fiber** v2.52.10 - Web framework
- **GORM** v1.31.1 - ORM library
- **MySQL Driver** v1.8.1
- **JWT** v5.3.0 - JSON Web Token
- **godotenv** v1.5.1 - Environment management
- **UUID** v1.6.0 - UUID generator

## 📋 Prerequisites

Make sure your system has the following installed:

- Go 1.25.5 or higher
- MySQL 8.0 or higher
- Make (optional, for migration commands)
- golang-migrate (for database migration)

### Install golang-migrate

**MacOS:**

```bash
brew install golang-migrate
```

**Linux:**

```bash
curl -L https://github.com/golang-migrate/migrate/releases/download/v4.15.2/migrate.linux-amd64.tar.gz | tar xvz
sudo mv migrate /usr/local/bin/migrate
```

**Windows:**

```bash
scoop install migrate
```

## 🚀 Getting Started

### 1. Clone Repository

```bash
git clone <repository-url>
cd boilerplate-golang-fiber
```

### 2. Setup Environment

Create environment file based on template:

```bash
cp .env.example .env.development
```

Edit `.env.development` and adjust with your configuration:

```env
DB_USER=root
DB_PASS=rahasia
DB_NAME=your_database_name
DB_HOST=127.0.0.1
DB_PORT=3306
APP_PORT=3000
```

### 3. Install Dependencies

```bash
go mod download
```

### 4. Setup Database

Create new database in MySQL:

```sql
CREATE DATABASE your_database_name CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 5. Run Database Migration

```bash
# Run all migrations
make up

# Or if you want to create new migration
make create name=create_users_table

# Rollback last migration
make down

# View current migration version
make version
```

### 6. Run Application

```bash
go run cmd/api/main.go
```

Server will run on `http://localhost:3000` (or according to configured `APP_PORT`)

## 🔧 Available Make Commands

```bash
# Create new migration file
make create name=migration_name

# Run all migrations
make up

# Rollback last migration
make down

# View current migration version
make version

# Fix "Dirty" state if migration fails
make force version=1
```

## 📡 API Endpoints

### Health Check

```http
GET /api/v1/health
```

**Response:**

```json
{
  "success": true,
  "message": "API is Healthy",
  "data": {
    "server_time": "2025-12-18 15:04:05",
    "timezone": "Asia/Jakarta"
  }
}
```

## 🏗️ Architecture Layers

### Handler Layer

Handles HTTP requests and responses. Responsible for:

- Parse request
- Input validation
- Call service layer
- Send response

### Service Layer

Contains application business logic. Responsible for:

- Orchestrating data flow
- Implement business rules
- Call repository layer

### Repository Layer

Handles database operations. Responsible for:

- Query database
- CRUD operations
- Data mapping

### Model Layer

Defines data structures used in the application

### Middleware Layer

Custom middlewares for:

- Authentication
- Authorization
- Logging
- Error handling

## 📦 Response Format

### Success Response

```go
utils.SuccessResponse(c, statusCode, message, data)
```

```json
{
  "success": true,
  "message": "Success message",
  "data": {
    /* your data */
  }
}
```

### Error Response

```go
utils.ErrorResponse(c, statusCode, message, errors)
```

```json
{
  "success": false,
  "message": "Error message",
  "errors": {
    /* error details */
  }
}
```

## 🔒 Security Features

- **CORS Configuration**: Configured for specific domains
- **Rate Limiting**: Max 50 requests per minute
- **JWT Ready**: Ready for authentication implementation
- **Environment Variables**: Sensitive data stored in environment

## 🌍 Environment Configuration

This project supports multiple environments:

- `.env.development` - Development environment
- `.env.production` - Production environment

By default, application will use development environment. Change by setting `APP_ENV`:

```bash
export APP_ENV=production
go run cmd/api/main.go
```

## 🕒 Timezone

Application is configured to use **Asia/Jakarta** timezone by default. Timezone is set during application startup and will affect all time-related operations.

## 📝 Database Migration

Migration files are stored in `pkg/database/migrations/`. Naming format:

```
000001_migration_name.up.sql
000001_migration_name.down.sql
```

Example migration file:

**000001_create_users_table.up.sql:**

```sql
CREATE TABLE users (
    id VARCHAR(36) PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

**000001_create_users_table.down.sql:**

```sql
DROP TABLE IF EXISTS users;
```

## 🔄 Development Workflow

1. Create model in `internal/model/`
2. Create repository in `internal/repository/`
3. Create service in `internal/service/`
4. Create handler in `internal/handler/`
5. Register route in `cmd/api/main.go`
6. Create middleware if needed in `internal/middleware/`

## 📚 Best Practices

- Use environment variables for configuration
- Separate business logic from HTTP handlers
- Use struct for request/response validation
- Implement proper error handling
- Use migration for database schema changes
- Follow clean architecture principles
- Write unit tests for business logic

## 🤝 Contributing

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is open-source and available under the MIT License.

## 👨‍💻 Author

Ifqy Gifha Azhar

## 🙏 Acknowledgments

- [Go Fiber](https://gofiber.io/) - Web framework
- [GORM](https://gorm.io/) - ORM library
- [golang-migrate](https://github.com/golang-migrate/migrate) - Database migration tool
