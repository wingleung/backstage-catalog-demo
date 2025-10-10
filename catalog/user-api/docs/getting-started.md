# Getting Started

## Prerequisites

Before running the User API locally, ensure you have:

- Node.js 20+ installed
- PostgreSQL 15+ running
- Redis 7+ running
- npm or yarn package manager

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-org/user-api.git
cd user-api
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment

Create a `.env` file in the project root:

```bash
# Application
NODE_ENV=development
PORT=3000
API_VERSION=v1

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/userdb

# Redis
REDIS_URL=redis://localhost:6379

# JWT Configuration
JWT_SECRET=your-secret-key-change-in-production
JWT_EXPIRY=15m
REFRESH_TOKEN_EXPIRY=7d

# OAuth Providers
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GITHUB_CLIENT_ID=your-github-client-id
GITHUB_CLIENT_SECRET=your-github-client-secret

# Email Service
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password
```

### 4. Run Database Migrations

```bash
npm run migrate:dev
```

### 5. Seed Sample Data (Optional)

```bash
npm run seed
```

This creates:
- Admin user: `admin@example.com` / `admin123`
- Regular user: `user@example.com` / `user123`
- Sample roles and permissions

### 6. Start the Development Server

```bash
npm run dev
```

The API will be available at `http://localhost:3000`

## Testing the API

### Using cURL

```bash
# Register a new user
curl -X POST http://localhost:3000/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "Test123!",
    "firstName": "John",
    "lastName": "Doe"
  }'

# Login
curl -X POST http://localhost:3000/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "Test123!"
  }'
```

### Using Postman

Import the Postman collection from `/docs/postman/user-api.postman_collection.json`

## Running Tests

```bash
# Unit tests
npm test

# Integration tests
npm run test:integration

# E2E tests
npm run test:e2e

# Coverage report
npm run test:coverage
```

## Development Workflow

1. Create a feature branch: `git checkout -b feature/my-feature`
2. Make changes and add tests
3. Run linter: `npm run lint`
4. Run tests: `npm test`
5. Commit changes: `git commit -m "feat: add new feature"`
6. Push and create PR: `git push origin feature/my-feature`
