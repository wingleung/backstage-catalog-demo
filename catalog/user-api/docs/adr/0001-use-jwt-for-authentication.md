# 1. Use JWT for Authentication

Date: 2025-01-20

## Status

Accepted

## Context

We need an authentication mechanism for our User API that supports stateless authentication, scales horizontally, and works across microservices.

## Decision

We will use **JWT (JSON Web Tokens)** for authentication with:
- **Access Tokens**: Short-lived (15 minutes)
- **Refresh Tokens**: Long-lived (7 days)
- **Algorithm**: HS256

## Consequences

### Positive
- Stateless - no server-side session storage
- Scalable - easy to scale horizontally
- Self-contained - all user info in token

### Negative
- Cannot revoke tokens before expiry
- Larger than session IDs

### Mitigation
- Short expiry times (15 min)
- Token blacklist in Redis for logout
