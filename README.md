# Python Portal Backend API

## Production-Grade Express.js Microservice

This is the backend API microservice for the Python Learning Portal, providing exercise management, user authentication, progress tracking, and integration with the Python executor service.

## 🏗️ Architecture

```
@python-portal/backend/
├── src/
│   ├── controllers/          # Request handlers
│   ├── middleware/           # Custom middleware
│   ├── routes/              # API route definitions
│   ├── services/            # Business logic
│   ├── models/              # Data models
│   ├── utils/               # Utility functions
│   ├── config/              # Configuration
│   └── index.ts             # Application entry point
├── database/
│   ├── migrations/          # Database migrations
│   └── seeds/               # Database seed data
├── scripts/
│   ├── migrate.ts           # Migration runner
│   └── seed.ts              # Seed data runner
└── tests/
    ├── unit/                # Unit tests
    ├── integration/         # Integration tests
    └── fixtures/            # Test fixtures
```

## 🚀 Quick Start

```bash
# Install dependencies
npm install

# Development mode
npm run dev

# Build for production
npm run build

# Run production server
npm run serve
```

## 📡 API Endpoints

### Authentication
```
POST   /api/auth/login       # User login
POST   /api/auth/register    # User registration
POST   /api/auth/refresh     # Token refresh
POST   /api/auth/logout      # User logout
```

### Exercises
```
GET    /api/exercises        # List all exercises
GET    /api/exercises/:id    # Get specific exercise
POST   /api/exercises/run    # Execute Python code
POST   /api/exercises/test   # Run exercise tests
```

### User Progress
```
GET    /api/progress         # Get user progress
POST   /api/progress         # Update progress
GET    /api/progress/stats   # Progress statistics
```

### Health & Monitoring
```
GET    /health               # Health check
GET    /metrics              # Application metrics
```

## 🔧 Configuration

### Environment Variables

```bash
# Server Configuration
PORT=3000
NODE_ENV=production
CORS_ORIGIN=https://your-frontend-url.com

# Database
DATABASE_URL=./database/portal.db

# Authentication
JWT_SECRET=your-super-secret-jwt-key
JWT_EXPIRES_IN=24h
BCRYPT_ROUNDS=12

# External Services
EXECUTOR_SERVICE_URL=http://executor:8000
EXECUTOR_TIMEOUT=30000

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000  # 15 minutes
RATE_LIMIT_MAX_REQUESTS=100

# Logging
LOG_LEVEL=info
LOG_FORMAT=combined
```

## 🐳 Docker Deployment

```bash
# Build image
docker build -t python-portal-backend .

# Run container
docker run -p 3000:3000 \
  -e NODE_ENV=production \
  -e JWT_SECRET=your-secret \
  python-portal-backend
```

## 🗄️ Database

Using SQLite for simplicity and portability:

```bash
# Run migrations
npm run migrate

# Seed database
npm run seed
```

### Schema Overview

```sql
-- Users table
CREATE TABLE users (
  id TEXT PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  username TEXT UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- User progress table
CREATE TABLE user_progress (
  id TEXT PRIMARY KEY,
  user_id TEXT NOT NULL,
  exercise_id TEXT NOT NULL,
  status TEXT NOT NULL, -- 'not_started', 'in_progress', 'completed'
  completion_time INTEGER,
  attempts INTEGER DEFAULT 0,
  last_code TEXT,
  FOREIGN KEY (user_id) REFERENCES users (id)
);

-- Exercise submissions table
CREATE TABLE exercise_submissions (
  id TEXT PRIMARY KEY,
  user_id TEXT NOT NULL,
  exercise_id TEXT NOT NULL,
  code TEXT NOT NULL,
  test_results TEXT, -- JSON
  success BOOLEAN NOT NULL,
  submitted_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users (id)
);
```

## 🧪 Testing

```bash
# Run all tests
npm test

# Unit tests only
npm run test:unit

# Integration tests
npm run test:integration

# Coverage report
npm run test:coverage
```

## 📊 Monitoring & Observability

### Health Checks
- **Liveness**: `/health` - Basic server health
- **Readiness**: `/health/ready` - Database connectivity
- **Dependencies**: `/health/deps` - External service status

### Metrics
- Request/response metrics
- Database query performance
- Python execution statistics
- User activity tracking

## 🔐 Security

### Authentication
- JWT-based authentication
- Secure password hashing (bcrypt)
- Token refresh mechanism
- Session management

### API Security
- Helmet.js for security headers
- CORS configuration
- Rate limiting
- Input validation (Joi)
- SQL injection prevention

### Code Execution Security
- Delegated to isolated executor service
- Request sanitization
- Timeout enforcement
- Resource limitations

## 🚀 Performance

### Optimizations
- Response compression
- Connection pooling
- Query optimization
- Caching strategies
- Async processing

### Scaling
- Stateless design
- Horizontal scaling ready
- Load balancer compatible
- Container orchestration support

## 🛠️ Development

### Code Quality
- TypeScript strict mode
- ESLint + Prettier
- Comprehensive testing
- Code coverage tracking
- Pre-commit hooks

### API Documentation
- OpenAPI/Swagger specification
- Interactive documentation
- Request/response examples
- Error code documentation

## 📋 API Response Format

```typescript
// Success Response
{
  "success": true,
  "data": { /* response data */ },
  "message": "Operation completed successfully"
}

// Error Response
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": { /* additional context */ }
  }
}
```

## 🤝 Contributing

1. Fork the repository
2. Create feature branch
3. Write comprehensive tests
4. Follow coding standards
5. Submit pull request

## 📝 License

MIT - see [LICENSE](LICENSE) file

---

**Production Ready**: This microservice is designed for production deployment with comprehensive monitoring, security, and scalability features.