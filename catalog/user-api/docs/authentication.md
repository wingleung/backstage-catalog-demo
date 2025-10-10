# Authentication

## Overview

The User API implements JWT (JSON Web Token) based authentication with access and refresh token patterns for secure, stateless authentication.

## Authentication Flow

```
┌──────┐                                   ┌─────────┐
│Client│                                   │User API │
└──┬───┘                                   └────┬────┘
   │                                            │
   │  POST /auth/login                          │
   │  { email, password }                       │
   ├───────────────────────────────────────────>│
   │                                            │
   │                                            │ Validate Credentials
   │                                            │ Generate Tokens
   │                                            │
   │  { accessToken, refreshToken }             │
   │<───────────────────────────────────────────┤
   │                                            │
   │  GET /users/me                             │
   │  Authorization: Bearer {accessToken}       │
   ├───────────────────────────────────────────>│
   │                                            │
   │                                            │ Verify Token
   │                                            │
   │  { user data }                             │
   │<───────────────────────────────────────────┤
```

## Token Types

### Access Token
- **Purpose**: Short-lived token for API access
- **Expiry**: 15 minutes
- **Storage**: Memory or secure cookie
- **Claims**: `userId`, `email`, `roles`, `permissions`

### Refresh Token
- **Purpose**: Long-lived token to obtain new access tokens
- **Expiry**: 7 days
- **Storage**: Secure, httpOnly cookie
- **Rotation**: New refresh token issued on each use

## Authentication Methods

### 1. Email/Password Authentication

**Endpoint**: `POST /auth/login`

```bash
curl -X POST http://localhost:3000/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123!"
  }'
```

**Response**:
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 900,
  "tokenType": "Bearer"
}
```

### 2. OAuth 2.0 Social Login

**Supported Providers**:
- Google
- GitHub
- Microsoft

**Flow**:
1. Redirect to provider: `GET /auth/oauth/{provider}`
2. User authorizes
3. Callback handled: `GET /auth/oauth/{provider}/callback`
4. JWT tokens issued

### 3. API Key Authentication

For service-to-service communication:

```bash
curl -X GET http://localhost:3000/v1/users/me \
  -H "X-API-Key: your-api-key"
```

## Using Access Tokens

Include the access token in the Authorization header:

```bash
curl -X GET http://localhost:3000/v1/users/me \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Token Refresh

When the access token expires, use the refresh token:

**Endpoint**: `POST /auth/refresh`

```bash
curl -X POST http://localhost:3000/v1/auth/refresh \
  -H "Content-Type: application/json" \
  -d '{
    "refreshToken": "YOUR_REFRESH_TOKEN"
  }'
```

## Security Best Practices

### Password Requirements
- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character

### Token Security
- Tokens are signed with HS256 algorithm
- Refresh tokens are stored hashed in database
- Token blacklist for logout
- Rate limiting on authentication endpoints

### Account Protection
- Maximum 5 failed login attempts before temporary lockout
- Email verification required for new accounts
- Password reset with time-limited tokens
- 2FA support for enhanced security

## Error Responses

### Invalid Credentials
```json
{
  "error": "INVALID_CREDENTIALS",
  "message": "Email or password is incorrect",
  "statusCode": 401
}
```

### Expired Token
```json
{
  "error": "TOKEN_EXPIRED",
  "message": "Access token has expired",
  "statusCode": 401
}
```

### Account Locked
```json
{
  "error": "ACCOUNT_LOCKED",
  "message": "Account locked due to multiple failed login attempts",
  "statusCode": 403,
  "retryAfter": 900
}
```
