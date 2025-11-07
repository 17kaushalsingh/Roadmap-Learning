# Chapter 9: APIs and Web Services

## API Fundamentals

### What is an API?
- **API (Application Programming Interface)**: Contract between different software components
- **Purpose**: Allow different applications to communicate with each other
- **Importance**: Foundation of modern web development, microservices, integration

### API Types
```mermaid
graph TD
    A["API Types"] --> B["Web APIs"]
    A --> C["Library APIs"]
    A --> D["OS APIs"]
    A --> E["Database APIs"]

    B --> F["REST"]
    B --> G["SOAP"]
    B --> H["GraphQL"]

    I["Web API Protocols"] --> J["HTTP/HTTPS"]
    I --> K["WebSocket"]
    I --> L["gRPC"]
```

## HTTP/HTTPS Fundamentals

### HTTP Request-Response Cycle
```mermaid
graph TD
    A["Client"] --> B["HTTP Request"]
    B --> C["Server"]
    C --> D["Process Request"]
    D --> E["HTTP Response"]
    E --> F["Client"]

    G["HTTP Request"] --> H["Method"]
    G --> I["URL"]
    G --> J["Headers"]
    G --> K["Body"]

    L["HTTP Response"] --> M["Status Code"]
    L --> N["Headers"]
    L --> O["Body"]
```

### HTTP Methods
| Method | Purpose | Idempotent | Safe |
|--------|---------|------------|------|
| **GET** | Retrieve data | Yes | Yes |
| **POST** | Create data | No | No |
| **PUT** | Update/replace data | Yes | No |
| **PATCH** | Partial update | No | No |
| **DELETE** | Remove data | Yes | No |
| **HEAD** | Headers only | Yes | Yes |
| **OPTIONS** | Supported methods | Yes | Yes |

### HTTP Status Codes
```mermaid
graph TD
    A["Status Codes"] --> B["1xx: Informational"]
    A --> C["2xx: Success"]
    A --> D["3xx: Redirection"]
    A --> E["4xx: Client Error"]
    A --> F["5xx: Server Error"]

    C --> G["200: OK"]
    C --> H["201: Created"]
    C --> I["204: No Content"]

    E --> J["400: Bad Request"]
    E --> K["401: Unauthorized"]
    E --> L["403: Forbidden"]
    E --> M["404: Not Found"]

    F --> N["500: Internal Server Error"]
    F --> O["502: Bad Gateway"]
    F --> P["503: Service Unavailable"]
```

## REST API

### REST Principles
```mermaid
graph TD
    A["REST Principles"] --> B["Client-Server Architecture"]
    A --> C["Stateless"]
    A --> D["Cacheable"]
    A --> E["Uniform Interface"]
    A --> F["Layered System"]

    E --> G["Resource Identification"]
    E --> H["Representations"]
    E --> I["Self-Descriptive Messages"]
    E --> J["Hypermedia as Engine"]
```

### REST API Design

#### Resource Naming
```mermaid
graph TD
    A["REST Resource Design"] --> B["Use Nouns"]
    A --> C["Plural Names"]
    A --> D["Hierarchical Structure"]

    E["Good Examples"] --> F["/users"]
    E --> G["/users/{id}/orders"]
    E --> H["/products/{id}/reviews"]

    I["Bad Examples"] --> J["/getAllUsers"]
    I --> K["/user-data"]
    I --> L["/users_list.php"]
```

**Resource Naming Conventions**:
- **Use nouns, not verbs**: `/users` not `/getUsers`
- **Use plural**: `/users` not `/user`
- **Hierarchical**: `/users/{id}/orders`
- **Consistent**: Same pattern across resources

#### HTTP Method Usage
```mermaid
graph TD
    A["HTTP Methods for Resources"] --> B["GET /users"]
    A --> C["POST /users"]
    A --> D["GET /users/{id}"]
    A --> E["PUT /users/{id}"]
    A --> F["DELETE /users/{id}"]

    B --> G["List all users"]
    C --> H["Create new user"]
    D --> I["Get specific user"]
    E --> J["Update entire user"]
    F --> K["Delete user"]
```

### Request and Response Formats

#### JSON Format
```json
// Request Example
POST /api/users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30
}

// Response Example
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30,
  "createdAt": "2024-01-15T10:30:00Z"
}
```

#### Error Response Format
```json
// Error Response Example
HTTP/1.1 400 Bad Request
Content-Type: application/json

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
        "message": "Age must be between 18 and 100"
      }
    ]
  }
}
```

## API Design Best Practices

### Versioning
```mermaid
graph TD
    A["API Versioning Strategies"] --> B["URL Path Versioning"]
    A --> C["Query Parameter Versioning"]
    A --> D["Header Versioning"]
    A --> E["Content Negotiation"]

    B --> F["/api/v1/users"]
    C --> G["/api/users?version=1"]
    D --> H["Accept: application/vnd.api.v1+json"]
    E --> I["Content-Type: application/vnd.api.v1+json"]
```

**Versioning Examples**:
- **URL Path**: `/api/v1/users`, `/api/v2/users`
- **Query Parameter**: `/api/users?version=1`
- **Header**: `Accept: application/vnd.api.v1+json`

### Authentication and Authorization

#### API Key Authentication
```http
// Request with API Key
GET /api/users
X-API-Key: your-api-key-here
```

#### JWT (JSON Web Token) Authentication
```mermaid
graph TD
    A["Login Request"] --> B["Validate Credentials"]
    B --> C["Generate JWT"]
    C --> D["Return Token"]
    D --> E["Client Stores Token"]
    E --> F["Include Token in Requests"]
    F --> G["Server Validates Token"]
    G --> H["Process Request"]
```

```http
// Login Request
POST /api/auth/login
Content-Type: application/json

{
  "username": "john@example.com",
  "password": "password123"
}

// Login Response
HTTP/1.1 200 OK
Content-Type: application/json

{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 3600
}

// Authenticated Request
GET /api/users
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Rate Limiting
```mermaid
graph TD
    A["Client Request"] --> B["Check Rate Limit"]
    B --> C{"Within Limit?"}
    C -->|Yes| D["Process Request"]
    C -->|No| E["Return 429 Too Many Requests"]
    E --> F["Include Rate Limit Headers"]

    G["Rate Limit Headers"] --> H["X-RateLimit-Limit"]
    G --> I["X-RateLimit-Remaining"]
    G --> J["X-RateLimit-Reset"]
```

**Rate Limiting Strategies**:
- **Fixed Window**: Reset at fixed intervals
- **Sliding Window**: Rolling time window
- **Token Bucket**: Tokens refill over time
- **Leaky Bucket**: Process at constant rate

### Pagination
```mermaid
graph TD
    A["Pagination Strategies"] --> B["Offset-Based"]
    A --> C["Cursor-Based"]
    A --> D["Page-Based"]

    B --> E["?page=2&limit=20"]
    C --> F["?cursor=next_page_token"]
    D --> G["?page=2&per_page=20"]
```

**Pagination Response**:
```json
{
  "data": [...],
  "pagination": {
    "page": 2,
    "limit": 20,
    "total": 150,
    "totalPages": 8,
    "hasNext": true,
    "hasPrev": true,
    "nextPage": 3,
    "prevPage": 1
  }
}
```

## GraphQL

### GraphQL vs REST
```mermaid
graph TD
    A["Comparison"] --> B["REST"]
    A --> C["GraphQL"]

    B --> D["Multiple Endpoints"]
    B --> E["Fixed Data Structure"]
    B --> F["Over-fetching/Under-fetching"]

    C --> G["Single Endpoint"]
    C --> H["Flexible Data Structure"]
    C --> I["Exact Data Requirements"]
```

| Feature | REST | GraphQL |
|---------|------|---------|
| **Endpoints** | Multiple | Single (/graphql) |
| **Data Fetching** | Fixed structure | Client specifies |
| **Over-fetching** | Common | No over-fetching |
| **Versioning** | URL versioning | Schema evolution |
| **Caching** | HTTP caching | Complex caching |
| **Learning Curve** | Simple | Steeper |

### GraphQL Query Example
```graphql
# Query
query GetUser($id: ID!) {
  user(id: $id) {
    name
    email
    posts {
      title
      createdAt
      comments {
        author {
          name
        }
        content
      }
    }
  }
}

# Variables
{
  "id": "123"
}

# Response
{
  "data": {
    "user": {
      "name": "John Doe",
      "email": "john@example.com",
      "posts": [
        {
          "title": "Hello World",
          "createdAt": "2024-01-15T10:30:00Z",
          "comments": [
            {
              "author": {
                "name": "Jane Smith"
              },
              "content": "Great post!"
            }
          ]
        }
      ]
    }
  }
}
```

## Web Security

### Common Security Threats
```mermaid
graph TD
    A["Web API Security Threats"] --> B["Injection Attacks"]
    A --> C["Authentication Issues"]
    A --> D["Data Exposure"]
    A --> E["Rate Limiting Bypass"]

    B --> F["SQL Injection"]
    B --> G["NoSQL Injection"]
    B --> H["Command Injection"]

    C --> I["Weak Authentication"]
    C --> J["Session Management"]
    C --> K["Token Security"]
```

### Security Best Practices

#### Input Validation
```mermaid
graph TD
    A["Input Validation"] --> B["Server-Side Validation"]
    A --> C["Type Checking"]
    A --> D["Range Checking"]
    A --> E["Format Validation"]

    F["Validation Layers"] --> G["Client-Side (UX)"]
    F --> H["Server-Side (Security)"]
    F --> I["Database Level"]
```

#### HTTPS and TLS
- **HTTPS**: HTTP over TLS/SSL encryption
- **TLS**: Transport Layer Security for data encryption
- **Certificate**: Server identity verification
- **HSTS**: HTTP Strict Transport Security

#### CORS (Cross-Origin Resource Sharing)
```http
// CORS Headers
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 86400
```

## API Testing

### Testing Strategies
```mermaid
graph TD
    A["API Testing"] --> B["Unit Testing"]
    A --> C["Integration Testing"]
    A --> D["End-to-End Testing"]
    A --> E["Performance Testing"]

    B --> F["Individual Functions"]
    C --> G["Component Integration"]
    D --> H["Full Workflow"]
    E --> I["Load and Stress"]
```

### Common API Testing Tools
| Tool | Type | Features |
|------|------|----------|
| **Postman** | GUI Client | Request building, testing, documentation |
| **cURL** | Command Line | HTTP requests, automation |
| **JMeter** | Performance | Load testing, stress testing |
| **Swagger** | Documentation | API spec, testing interface |

### API Testing Example (Postman)
```json
{
  "info": {
    "name": "User API Tests",
    "description": "Test collection for User API"
  },
  "item": [
    {
      "name": "Create User",
      "request": {
        "method": "POST",
        "header": [
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "body": {
          "mode": "raw",
          "raw": "{\"name\":\"John Doe\",\"email\":\"john@example.com\"}"
        },
        "url": {
          "raw": "{{baseUrl}}/api/users",
          "host": ["{{baseUrl}}"],
          "path": ["api", "users"]
        }
      },
      "test": [
        "pm.test(\"Status code is 201\", function () {",
        "    pm.response.to.have.status(201);",
        "});",
        "pm.test(\"Response has id\", function () {",
        "    const jsonData = pm.response.json();",
        "    pm.expect(jsonData).to.have.property('id');",
        "});"
      ]
    }
  ]
}
```

## Microservices and APIs

### Microservice Communication
```mermaid
graph TD
    A["Microservice Communication"] --> B["Synchronous"]
    A --> C["Asynchronous"]

    B --> D["REST APIs"]
    B --> E["GraphQL"]
    B --> F["gRPC"]

    C --> G["Message Queues"]
    C --> H["Event Streaming"]
    C --> I["Publish-Subscribe"]
```

### API Gateway
```mermaid
graph TD
    A["Client"] --> B["API Gateway"]
    B --> C["User Service"]
    B --> D["Order Service"]
    B --> E["Product Service"]
    B --> F["Payment Service"]

    G["API Gateway Functions"] --> H["Request Routing"]
    G --> I["Authentication"]
    G --> J["Rate Limiting"]
    G --> K["Response Aggregation"]
```

## Common Interview Questions

### Basic Questions

**Q1: What is the difference between REST and SOAP?**
```mermaid
graph TD
    A["REST"] --> B["Lightweight"]
    A --> C["HTTP/HTTPS"]
    A --> D["JSON/XML"]
    A --> E["Stateless"]

    F["SOAP"] --> G["Heavyweight"]
    F --> H["Multiple Protocols"]
    F --> I["XML Only"]
    F --> J["Can be stateful"]
```

**Q2: Explain HTTP status codes categories**
- **1xx**: Informational (request received)
- **2xx**: Success (request processed)
- **3xx**: Redirection (further action needed)
- **4xx**: Client error (bad request)
- **5xx**: Server error (server failed)

**Q3: What is the difference between PUT and PATCH?**
- **PUT**: Replace entire resource with new data
- **PATCH**: Partial update of resource
- **Idempotency**: PUT is idempotent, PATCH may not be

### Intermediate Questions

**Q4: How would you design a RESTful API for a blog system?**
```mermaid
graph TD
    A["Blog API Resources"] --> B["/posts"]
    A --> C["/posts/{id}/comments"]
    A --> D["/users"]
    A --> E["/categories"]

    F["HTTP Methods"] --> G["GET /posts - List posts"]
    F --> H["POST /posts - Create post"]
    F --> I["PUT /posts/{id} - Update post"]
    F --> J["DELETE /posts/{id} - Delete post"]
```

**Q5: What are the best practices for API security?**
- **Authentication**: JWT, API keys, OAuth
- **Authorization**: Role-based access control
- **Input Validation**: Server-side validation
- **HTTPS**: Encrypt all communication
- **Rate Limiting**: Prevent abuse
- **CORS**: Control cross-origin access

### Advanced Questions

**Q6: How does GraphQL differ from REST?**
- **Single Endpoint**: GraphQL uses `/graphql`, REST has multiple endpoints
- **Data Fetching**: GraphQL gets exact data needed, REST may over-fetch
- **Strong Typing**: GraphQL has schema, REST is loosely typed
- **Real-time**: GraphQL supports subscriptions natively

**Q7: Explain microservices architecture and API communication**
```mermaid
graph TD
    A["Microservices"] --> B["Independent Services"]
    A --> C["API Communication"]
    A --> D["Separate Databases"]
    A --> E["Individual Deployment"]

    F["Communication Patterns"] --> G["Synchronous (REST/gRPC)"]
    F --> H["Asynchronous (Message Queues)"]
```

## Quick Reference

### HTTP Methods Summary
| Method | Purpose | Example |
|--------|---------|---------|
| **GET** | Retrieve data | `GET /api/users` |
| **POST** | Create data | `POST /api/users` |
| **PUT** | Replace data | `PUT /api/users/123` |
| **PATCH** | Update data | `PATCH /api/users/123` |
| **DELETE** | Remove data | `DELETE /api/users/123` |

### Common Status Codes
| Code | Meaning | Use Case |
|------|---------|---------|
| **200** | OK | Successful GET/PUT/PATCH |
| **201** | Created | Successful POST |
| **204** | No Content | Successful DELETE |
| **400** | Bad Request | Invalid input |
| **401** | Unauthorized | Authentication required |
| **404** | Not Found | Resource doesn't exist |
| **500** | Internal Error | Server failure |

### API Design Principles
| Principle | Description | Implementation |
|-----------|-------------|-------------|
| **Stateless** | No client state stored | Include all data in request |
| **Cacheable** | Responses can be cached | Set appropriate cache headers |
| **Uniform Interface** | Consistent API design | Use standard HTTP methods |
| **Client-Server** | Separation of concerns | Clear API boundaries |

### Interview Preparation Tips

1. **Understand HTTP fundamentals** (methods, status codes, headers)
2. **Design REST APIs** following best practices
3. **Know security principles** (authentication, authorization, HTTPS)
4. **Practice API testing** with tools like Postman
5. **Understand microservices** and API communication patterns

### Common Mistakes to Avoid

1. **Using wrong HTTP methods** (GET for data changes)
2. **Not implementing proper error handling**
3. **Ignoring security best practices**
4. **Poor API documentation**
5. **Inconsistent naming conventions**

---

**Important Note**: APIs are the backbone of modern web applications. Understanding REST principles, HTTP fundamentals, and security best practices is crucial for any software development role. Practice designing and consuming APIs regularly.