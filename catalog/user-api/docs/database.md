# Database Schema

## Overview

The User API uses PostgreSQL with Prisma ORM for type-safe database access. The schema supports user management, authentication, and authorization.

## Entity Relationship Diagram

```
┌─────────────┐         ┌─────────────┐         ┌─────────────┐
│    User     │         │  UserRole   │         │    Role     │
├─────────────┤         ├─────────────┤         ├─────────────┤
│ id (PK)     │────────<│ userId (FK) │>────────│ id (PK)     │
│ email       │         │ roleId (FK) │         │ name        │
│ password    │         │ assignedAt  │         │ description │
│ firstName   │         └─────────────┘         │ permissions │
│ lastName    │                                 └─────────────┘
│ createdAt   │         
│ updatedAt   │         ┌─────────────┐
│ deletedAt   │         │  Session    │
│ verified    │         ├─────────────┤
└─────────────┘────────<│ userId (FK) │
                        │ token       │
                        │ expiresAt   │
                        │ createdAt   │
                        └─────────────┘
```

## Tables

### users

Stores core user information.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique user identifier |
| email | VARCHAR(255) | UNIQUE, NOT NULL | User email address |
| password | VARCHAR(255) | NOT NULL | Hashed password (bcrypt) |
| first_name | VARCHAR(100) | NOT NULL | User's first name |
| last_name | VARCHAR(100) | NOT NULL | User's last name |
| verified | BOOLEAN | DEFAULT false | Email verification status |
| two_factor_enabled | BOOLEAN | DEFAULT false | 2FA status |
| two_factor_secret | VARCHAR(255) | NULLABLE | TOTP secret for 2FA |
| last_login | TIMESTAMP | NULLABLE | Last successful login |
| failed_login_attempts | INTEGER | DEFAULT 0 | Failed login counter |
| locked_until | TIMESTAMP | NULLABLE | Account lock expiry |
| created_at | TIMESTAMP | DEFAULT NOW() | Record creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |
| deleted_at | TIMESTAMP | NULLABLE | Soft delete timestamp |

**Indexes**:
- `idx_users_email` on `email`
- `idx_users_deleted_at` on `deleted_at`

### roles

Defines available roles in the system.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique role identifier |
| name | VARCHAR(50) | UNIQUE, NOT NULL | Role name (e.g., 'admin') |
| description | TEXT | NULLABLE | Role description |
| permissions | JSONB | NOT NULL | Array of permission strings |
| created_at | TIMESTAMP | DEFAULT NOW() | Record creation time |

**Default Roles**:
- `admin` - Full system access
- `user` - Standard user access
- `moderator` - Content moderation access
- `readonly` - Read-only access

### user_roles

Many-to-many relationship between users and roles.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier |
| user_id | UUID | FOREIGN KEY | Reference to users.id |
| role_id | UUID | FOREIGN KEY | Reference to roles.id |
| assigned_at | TIMESTAMP | DEFAULT NOW() | Role assignment time |
| assigned_by | UUID | FOREIGN KEY | User who assigned the role |

**Indexes**:
- `idx_user_roles_user_id` on `user_id`
- `idx_user_roles_role_id` on `role_id`
- `unique_user_role` on `(user_id, role_id)`

### sessions

Tracks active user sessions and refresh tokens.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique session identifier |
| user_id | UUID | FOREIGN KEY | Reference to users.id |
| token_hash | VARCHAR(255) | NOT NULL | Hashed refresh token |
| device_info | JSONB | NULLABLE | Device/browser information |
| ip_address | INET | NULLABLE | Client IP address |
| expires_at | TIMESTAMP | NOT NULL | Session expiry time |
| revoked | BOOLEAN | DEFAULT false | Revocation status |
| created_at | TIMESTAMP | DEFAULT NOW() | Session start time |

**Indexes**:
- `idx_sessions_user_id` on `user_id`
- `idx_sessions_expires_at` on `expires_at`
- `idx_sessions_token_hash` on `token_hash`

### oauth_accounts

Links external OAuth provider accounts to users.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique identifier |
| user_id | UUID | FOREIGN KEY | Reference to users.id |
| provider | VARCHAR(50) | NOT NULL | OAuth provider (google, github) |
| provider_user_id | VARCHAR(255) | NOT NULL | Provider's user ID |
| access_token | TEXT | NULLABLE | Encrypted access token |
| refresh_token | TEXT | NULLABLE | Encrypted refresh token |
| created_at | TIMESTAMP | DEFAULT NOW() | Link creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last token refresh |

**Indexes**:
- `unique_provider_account` on `(provider, provider_user_id)`

## Migrations

### Running Migrations

```bash
# Create a new migration
npm run migrate:create -- --name add_user_preferences

# Apply pending migrations
npm run migrate:dev

# Apply migrations in production
npm run migrate:deploy

# Rollback last migration
npm run migrate:rollback
```

### Example Migration

```sql
-- Add user preferences table
CREATE TABLE user_preferences (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  theme VARCHAR(20) DEFAULT 'light',
  language VARCHAR(5) DEFAULT 'en',
  notifications JSONB DEFAULT '{"email": true, "push": false}',
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_user_preferences_user_id ON user_preferences(user_id);
```

## Backup & Restore

```bash
# Backup database
pg_dump -h localhost -U user userdb > backup.sql

# Restore database
psql -h localhost -U user userdb < backup.sql
```
