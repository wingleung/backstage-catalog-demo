# API Reference

## REST Endpoints

### Create Payment

**POST** `/api/v1/payments`

Creates a new payment transaction.

**Request Body:**
```json
{
  "amount": 1000,
  "currency": "USD",
  "customer_id": "cust_123",
  "payment_method": "card",
  "metadata": {
    "order_id": "order_456"
  }
}
```

**Response:**
```json
{
  "id": "pay_789",
  "status": "succeeded",
  "amount": 1000,
  "currency": "USD",
  "created_at": "2025-10-10T15:30:00Z"
}
```

### Get Payment Status

**GET** `/api/v1/payments/{payment_id}`

Retrieves the current status of a payment.

**Response:**
```json
{
  "id": "pay_789",
  "status": "succeeded",
  "amount": 1000,
  "currency": "USD",
  "customer_id": "cust_123",
  "created_at": "2025-10-10T15:30:00Z",
  "updated_at": "2025-10-10T15:30:05Z"
}
```

### Refund Payment

**POST** `/api/v1/payments/{payment_id}/refund`

Initiates a refund for a completed payment.

**Request Body:**
```json
{
  "amount": 500,
  "reason": "customer_request"
}
```

## Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 404 | Not Found |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
