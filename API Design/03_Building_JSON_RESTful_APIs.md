# Chapter 03: Building JSON/RESTful APIs

## Introduction

**📌 RESTful API**: An API that follows REST (Representational State Transfer) architectural principles using HTTP methods and JSON data format.

This chapter covers practical implementation patterns for building robust, scalable RESTful APIs.

---

## CRUD Operations

### What is CRUD?

**CRUD (Create, Read, Update, Delete)**: The four basic operations for managing data in persistent storage.

### CRUD to HTTP Mapping

| Operation | HTTP Method | Endpoint | Success Status | Idempotent |
|-----------|-------------|----------|----------------|------------|
| **Create** | POST | `/users` | 201 Created | No |
| **Read (All)** | GET | `/users` | 200 OK | Yes |
| **Read (One)** | GET | `/users/{id}` | 200 OK | Yes |
| **Update (Full)** | PUT | `/users/{id}` | 200 OK | Yes |
| **Update (Partial)** | PATCH | `/users/{id}` | 200 OK | No |
| **Delete** | DELETE | `/users/{id}` | 204 No Content | Yes |

**📌 Idempotent**: Same result no matter how many times you call it.

### CRUD Examples

#### Create User (POST)

**Request:**
```http
POST /api/v1/users HTTP/1.1
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30
}
```

**Response:**
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30,
  "createdAt": "2023-12-01T10:30:00Z"
}
```

#### Get User (GET)

**Request:**
```http
GET /api/v1/users/123 HTTP/1.1
```

**Response:**
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30,
  "createdAt": "2023-12-01T10:30:00Z",
  "updatedAt": "2023-12-01T10:30:00Z"
}
```

#### Update User (PUT)

**Request:**
```http
PUT /api/v1/users/123 HTTP/1.1
Content-Type: application/json

{
  "name": "John Smith",
  "email": "john.smith@example.com",
  "age": 31
}
```

**Response:**
```json
{
  "id": 123,
  "name": "John Smith",
  "email": "john.smith@example.com",
  "age": 31,
  "updatedAt": "2023-12-01T11:00:00Z"
}
```

#### Partial Update (PATCH)

**Request:**
```http
PATCH /api/v1/users/123 HTTP/1.1
Content-Type: application/json

{
  "age": 32
}
```

#### Delete User (DELETE)

**Request:**
```http
DELETE /api/v1/users/123 HTTP/1.1
```

**Response:**
```http
HTTP/1.1 204 No Content
```

---

## API Versioning

### Why Version APIs?

- **Backward Compatibility**: Existing clients continue working
- **Evolution**: Add features without breaking changes
- **Migration**: Gradual transition between versions

### Versioning Strategies

| Strategy | Example | Pros | Cons |
|----------|---------|------|------|
| **URI Versioning** | `/api/v1/users` | Clear and explicit | URL pollution |
| **Query Parameter** | `/api/users?version=1` | Easy to implement | Not cache-friendly |
| **Header Versioning** | `Accept: application/vnd.api+json;version=1` | Clean URLs | Complex client setup |
| **Subdomain** | `v1.api.example.com/users` | Clear separation | SSL certificate overhead |

**📌 Recommendation**: URI versioning (`/api/v1/`) is the most widely used and understood.

### Versioning Implementation

```javascript
// Express.js versioning example
app.use('/api/v1', v1Routes);
app.use('/api/v2', v2Routes);

// v1 route
app.get('/api/v1/users', (req, res) => {
  // Version 1 implementation
});

// v2 route with enhanced features
app.get('/api/v2/users', (req, res) => {
  // Version 2 implementation with additional fields
});
```

---

## RESTful URI Design

### Resource Naming Principles

1. **Use Nouns, Not Verbs**: `/users` not `/getUsers`
2. **Use Plurals**: `/users` not `/user`
3. **Use Hierarchical Structure**: `/users/123/orders`
4. **Be Consistent**: Follow patterns throughout API

### URI Structure Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| **Collection** | List of resources | `/users` |
| **Specific Resource** | Single resource | `/users/123` |
| **Nested Resources** | Resource relationships | `/users/123/orders` |
| **Actions** | Non-CRUD operations | `/users/123/deactivate` |

### Common RESTful Patterns

```javascript
// User management
GET    /users              // List all users
POST   /users              // Create new user
GET    /users/123          // Get specific user
PUT    /users/123          // Update entire user
PATCH  /users/123          // Partial update
DELETE /users/123          // Delete user

// User relationships
GET    /users/123/orders   // Get user's orders
POST   /users/123/orders   // Create order for user
GET    /users/123/profile  // Get user profile

// Search and filter
GET    /users?search=john  // Search users
GET    /users?age=30       // Filter by age
GET    /users?sort=name    // Sort by name
```

---

## Pagination

### Why Pagination Matters

- **Performance**: Reduce response sizes
- **Bandwidth**: Save client data transfer
- **User Experience**: Manage large datasets
- **Server Load**: Reduce database queries

### Pagination Types

| Type | Description | When to Use |
|------|-------------|-------------|
| **Offset-Based** | Skip N records, take M | Simple cases, small datasets |
| **Cursor-Based** | Use next/previous pointers | Large datasets, real-time data |
| **Page-Based** | Traditional page numbers | User-facing pagination |

### Offset-Based Pagination

**Request:**
```http
GET /api/v1/users?page=2&limit=10 HTTP/1.1
```

**Response:**
```json
{
  "data": [...],
  "pagination": {
    "page": 2,
    "limit": 10,
    "total": 150,
    "totalPages": 15,
    "hasNext": true,
    "hasPrev": true
  }
}
```

### Cursor-Based Pagination

**Request:**
```http
GET /api/v1/users?limit=10&cursor=eyJpZCI6MTIzfQ==
```

**Response:**
```json
{
  "data": [...],
  "pagination": {
    "nextCursor": "eyJpZCI6MTMzfQ==",
    "hasNext": true,
    "limit": 10
  }
}
```

### Pagination Headers

```http
X-Total-Count: 150
X-Page: 2
X-Total-Pages: 15
X-Per-Page: 10
Link: </api/v1/users?page=1>; rel="prev", </api/v1/users?page=3>; rel="next"
```

---

## Query Parameters and Filtering

### Common Query Patterns

| Parameter | Purpose | Example |
|-----------|---------|---------|
| **Search** | Text search | `?search=john` |
| **Filter** | Filter by field | `?status=active` |
| **Sort** | Order results | `?sort=name,asc` |
| **Fields** | Select fields | `?fields=id,name` |
| **Include** | Include relations | `?include=posts,profile` |

### Advanced Filtering

```http
# Multiple filters
GET /api/v1/users?status=active&age_gt=18&country=US

# Range filtering
GET /api/v1/products?price_gte=100&price_lte=500

# Date filtering
GET /api/v1/posts?created_after=2023-01-01&created_before=2023-12-31

# Multiple values
GET /api/v1/products?category=electronics,books

# Sorting multiple fields
GET /api/v1/users?sort=created_at,desc&sort=name,asc
```

### Filtering Operators

| Operator | Symbol | Example | Meaning |
|----------|--------|---------|---------|
| **Equals** | `=` | `age=30` | Exact match |
| **Not Equal** | `ne=` | `status_ne=deleted` | Not equal to |
| **Greater Than** | `gt=` | `price_gt=100` | Greater than |
| **Less Than** | `lt=` | `age_lt=18` | Less than |
| **Greater/Equal** | `gte=` | `rating_gte=4` | Greater or equal |
| **Less/Equal** | `lte=` | `quantity_lte=10` | Less or equal |
| **Contains** | `contains=` | `name_contains=john` | Contains text |
| **In** | `in=` | `category_in=electronics,books` | In list |
| **Not In** | `nin=` | `status_nin=deleted,banned` | Not in list |

---

## Rate Limiting

### Why Rate Limit?

- **Prevent Abuse**: Stop malicious actors
- **Fair Usage**: Ensure equal access
- **Cost Control**: Manage server resources
- **SLA Compliance**: Meet service level agreements

### Rate Limiting Algorithms

| Algorithm | Description | Pros | Cons |
|-----------|-------------|------|------|
| **Token Bucket** | Fixed capacity tokens refilling over time | Smooth traffic, allows bursts | Complex to implement |
| **Sliding Window** | Track requests in time window | Precise control | Memory intensive |
| **Fixed Window** | Reset count at fixed intervals | Simple implementation | Traffic spikes at reset |
| **Leaky Bucket** | Process requests at fixed rate | Consistent output | Can delay requests |

### Rate Limiting Implementation

```javascript
// Token Bucket Example
const rateLimiter = new Map();

function checkRateLimit(userId, limit, windowMs) {
  const now = Date.now();
  const userLimit = rateLimiter.get(userId) || {
    tokens: limit,
    lastRefill: now
  };

  // Refill tokens based on time elapsed
  const elapsed = now - userLimit.lastRefill;
  const tokensToAdd = Math.floor(elapsed / windowMs) * limit;
  userLimit.tokens = Math.min(limit, userLimit.tokens + tokensToAdd);
  userLimit.lastRefill = now;

  if (userLimit.tokens > 0) {
    userLimit.tokens--;
    rateLimiter.set(userId, userLimit);
    return true;
  }

  return false;
}
```

### Rate Limiting Headers

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1638360000
Retry-After: 60
```

### Rate Limiting Responses

**Success:**
```http
HTTP/1.1 200 OK
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1638360000
```

**Rate Limited:**
```http
HTTP/1.1 429 Too Many Requests
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1638360000
Retry-After: 60

{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Please try again later."
  }
}
```

---

## Error Handling

### Consistent Error Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      },
      {
        "field": "age",
        "message": "Age must be between 18 and 120"
      }
    ],
    "timestamp": "2023-12-01T10:30:00Z",
    "path": "/api/v1/users",
    "requestId": "req_123456789"
  }
}
```

### HTTP Status Code Usage

| Status | When to Use | Example |
|--------|-------------|---------|
| **400 Bad Request** | Invalid request data | Missing required fields |
| **401 Unauthorized** | Authentication required | Missing API key |
| **403 Forbidden** | Insufficient permissions | User cannot access resource |
| **404 Not Found** | Resource doesn't exist | User ID not found |
| **409 Conflict** | Resource conflict | Email already exists |
| **422 Unprocessable Entity** | Valid format but business logic error | Password too weak |
| **429 Too Many Requests** | Rate limit exceeded | Too many API calls |
| **500 Internal Server Error** | Unexpected server error | Database connection failed |

### Validation Error Examples

```json
// Missing required field
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Required fields missing",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      }
    ]
  }
}

// Invalid format
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid field formats",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      },
      {
        "field": "age",
        "message": "Age must be a number"
      }
    ]
  }
}

// Business logic violation
{
  "error": {
    "code": "BUSINESS_ERROR",
    "message": "Email already exists",
    "field": "email"
  }
}
```

---

## Response Standards

### Success Response Format

```json
{
  "success": true,
  "data": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com"
  },
  "meta": {
    "timestamp": "2023-12-01T10:30:00Z",
    "version": "v1"
  }
}
```

### List Response Format

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "John Doe"
    },
    {
      "id": 2,
      "name": "Jane Smith"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 50,
    "totalPages": 5
  },
  "meta": {
    "timestamp": "2023-12-01T10:30:00Z",
    "version": "v1"
  }
}
```

### Response Headers

```http
Content-Type: application/json
Cache-Control: max-age=3600, public
ETag: "abc123"
Last-Modified: Wed, 21 Oct 2015 07:28:00 GMT
X-API-Version: v1
X-Request-ID: req_123456789
```

---

## Security Best Practices

### HTTPS Only

```javascript
// Force HTTPS in Express.js
app.use((req, res, next) => {
  if (!req.secure && req.get('x-forwarded-proto') !== 'https') {
    return res.redirect(301, `https://${req.get('host')}${req.url}`);
  }
  next();
});
```

### Input Validation

```javascript
// Validate email format
function validateEmail(email) {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
}

// Sanitize input
function sanitizeInput(input) {
  return input.trim().replace(/[<>]/g, '');
}
```

### SQL Injection Prevention

```javascript
// Bad: Vulnerable to SQL injection
const query = `SELECT * FROM users WHERE email = '${email}'`;

// Good: Using parameterized queries
const query = 'SELECT * FROM users WHERE email = ?';
db.query(query, [email]);
```

### Rate Limiting by User

```javascript
// Different limits for different user types
const rateLimits = {
  free: { requests: 100, window: 3600000 },    // 100/hour
  premium: { requests: 10000, window: 3600000 }, // 10000/hour
  enterprise: { requests: 100000, window: 3600000 } // 100000/hour
};
```

---

## Performance Optimization

### Database Optimization

1. **Indexing**: Add indexes to frequently queried fields
2. **Query Optimization**: Use efficient queries
3. **Connection Pooling**: Reuse database connections
4. **Caching**: Cache frequently accessed data

### Response Caching

```javascript
// Cache control middleware
function setCacheControl(maxAge) {
  return (req, res, next) => {
    res.set('Cache-Control', `public, max-age=${maxAge}`);
    next();
  };
}

// Apply to static data endpoints
app.get('/api/v1/countries', setCacheControl(86400)); // 1 day
app.get('/api/v1/users', setCacheControl(300));      // 5 minutes
```

### Compression

```javascript
// Enable gzip compression
const compression = require('compression');
app.use(compression());
```

### Pagination for Large Datasets

```javascript
// Efficient pagination with OFFSET/LIMIT
app.get('/api/v1/users', async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = Math.min(parseInt(req.query.limit) || 10, 100);
  const offset = (page - 1) * limit;

  const users = await db.query(
    'SELECT * FROM users LIMIT ? OFFSET ?',
    [limit, offset]
  );

  const total = await db.query('SELECT COUNT(*) FROM users');

  res.json({
    data: users,
    pagination: {
      page,
      limit,
      total: total[0].count,
      totalPages: Math.ceil(total[0].count / limit)
    }
  });
});
```

---

## Interview Questions

### Basic Questions

1. **What are RESTful principles?**
   - Stateless: Each request contains all information
   - Client-Server: Clear separation of concerns
   - Cacheable: Responses can be cached
   - Uniform Interface: Consistent conventions
   - Layered System: Can have intermediate layers

2. **What's the difference between PUT and PATCH?**
   - PUT: Replace entire resource with new data
   - PATCH: Partially update resource with only changed fields

3. **Why do we need API versioning?**
   - Maintain backward compatibility
   - Allow gradual migration
   - Add features without breaking existing clients

### Intermediate Questions

4. **How do you implement pagination?**
   - Offset-based: `page=2&limit=10`
   - Cursor-based: Use next/previous pointers
   - Include pagination metadata in response

5. **What is rate limiting and why is it important?**
   - Limit number of requests per time period
   - Prevent abuse and ensure fair usage
   - Protect server resources

6. **How do you handle errors in REST APIs?**
   - Use appropriate HTTP status codes
   - Consistent error response format
   - Include error codes and messages
   - Provide validation details

### Advanced Questions

7. **What's the difference between offset and cursor pagination?**
   - Offset: Skip N records, problems with real-time data
   - Cursor: Use pointers, better for large datasets and real-time updates

8. **How do you optimize API performance?**
   - Database indexing and query optimization
   - Caching strategies (HTTP cache, Redis)
   - Compression and response size reduction
   - Connection pooling and keep-alive

9. **What are best practices for API security?**
   - HTTPS only
   - Input validation and sanitization
   - Rate limiting
   - Authentication and authorization
   - SQL injection prevention

---

## Summary

### Key Takeaways

1. **CRUD Operations**: Map directly to HTTP methods
2. **RESTful Design**: Use nouns, consistent patterns
3. **Versioning**: Essential for backward compatibility
4. **Pagination**: Handle large datasets efficiently
5. **Error Handling**: Be consistent and informative
6. **Rate Limiting**: Prevent abuse and ensure fairness
7. **Security**: HTTPS, validation, and protection

### Best Practices Checklist

- [ ] Use proper HTTP methods (GET, POST, PUT, PATCH, DELETE)
- [ ] Follow RESTful URI conventions (/users not /getUsers)
- [ ] Implement API versioning (/api/v1/users)
- [ ] Add pagination for list endpoints
- [ ] Use appropriate HTTP status codes
- [ ] Implement consistent error responses
- [ ] Add rate limiting
- [ ] Use HTTPS only
- [ ] Validate all inputs
- [ ] Add caching headers where appropriate

**Next Up**: Chapter 04 explores Authentication Methods, covering how to secure your APIs and verify user identity.