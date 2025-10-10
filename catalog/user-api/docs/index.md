# User API

## Overview

The User API service provides comprehensive user management capabilities including authentication, authorization, profile management, and user lifecycle operations.

## Key Features

- **User Registration & Onboarding**: Streamlined user creation with email verification
- **Authentication**: JWT-based authentication with refresh token support
- **Authorization**: Role-based access control (RBAC) with fine-grained permissions
- **Profile Management**: Update user profiles, preferences, and settings
- **Account Security**: Password reset, 2FA, session management
- **OAuth Integration**: Social login support (Google, GitHub, Microsoft)

## Technology Stack

| Component | Technology |
|-----------|------------|
| Runtime | Node.js 20 |
| Framework | Express.js |
| Database | PostgreSQL 15 |
| Cache | Redis 7 |
| Auth Library | Passport.js + JWT |
| Validation | Joi |
| ORM | Prisma |

## API Endpoints Summary

### Authentication
- `POST /auth/register` - Register new user
- `POST /auth/login` - Authenticate user
- `POST /auth/logout` - Logout user
- `POST /auth/refresh` - Refresh access token
- `POST /auth/reset-password` - Request password reset

### User Management
- `GET /users` - List all users (admin only)
- `GET /users/:id` - Get user by ID
- `PUT /users/:id` - Update user profile
- `DELETE /users/:id` - Soft delete user
- `GET /users/me` - Get current user profile

### Roles & Permissions
- `GET /roles` - List available roles
- `POST /users/:id/roles` - Assign role to user
- `DELETE /users/:id/roles/:roleId` - Remove role from user

## Architecture Patterns

The User API follows these design principles:

- **Layered Architecture**: Controller → Service → Repository pattern
- **Dependency Injection**: For testability and modularity
- **Event-Driven**: Publishes domain events for user lifecycle changes
- **API Versioning**: Supports multiple API versions (`/v1`, `/v2`)

## Health & Monitoring

**Health Check Endpoint**: `/health`

```json
{
  "status": "healthy",
  "timestamp": "2025-10-10T15:30:00Z",
  "checks": {
    "database": "up",
    "redis": "up",
    "oauth_providers": "up"
  }
}
```
