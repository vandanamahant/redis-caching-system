# API Contracts - Redis Caching Interface

## 🔌 Overview
This document outlines the RESTful API endpoints for interacting with the Redis caching layer.

---

### 1. Retrieve Cache Entry by Key
- **Endpoint:** `GET /api/v1/cache/{key}`
- **Description:** Fetches cached property details using the specified key.

#### Responses

**Success (200 OK):**
```json
{
  "status": "success",
  "data": {
    "cache_key": "prop:listing:12345",
    "cache_value": { "title": "Downtown Apartment", "price": 150000 },
    "ttl_seconds": 3600
  }
}
```

**Unhappy Path - Empty State (404 Not Found):**
```json
{
  "status": "error",
  "message": "No data found"
}
```

---

### 2. Create / Update Cache Entry
- **Endpoint:** `POST /api/v1/cache`
- **Description:** Adds or updates a cache entry with XSS sanitation.

#### Request Body
```json
{
  "cache_key": "prop:listing:12345",
  "cache_value": "{\"title\": \"Downtown Apartment\"}",
  "category": "residential",
  "ttl_seconds": 3600
}
```

---

### 3. Delete / Invalidate Cache Entry
- **Endpoint:** `DELETE /api/v1/cache/{key}`
- **Description:** Removes or invalidates a specific cache entry.

#### Responses
- **Success (204 No Content):** Successfully deleted the cache key.
- **Unhappy Path (404 Not Found):**
  ```json
  {
    "status": "error",
    "message": "No data found"
  }
  ```

---

## 🔒 Security & Performance Guidelines
- **Input Sanitization:** All incoming cache keys and values must be sanitized to prevent XSS injections before processing.
- **Telemetry Logging:** Successful API interactions will trigger a simulated telemetry ping to the console:
  > `[Analytics] User interacted with Redis Caching`
- **Loading States:** For slow network conditions (e.g., 3G simulation), asynchronous API calls must display a visual loading indicator on the client side.
