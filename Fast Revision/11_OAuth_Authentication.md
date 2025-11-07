# Chapter 11: OAuth and Authentication

## Authentication Fundamentals

### What is Authentication?
- **Authentication**: Process of verifying identity (who are you?)
- **Authorization**: Process of granting permissions (what can you do?)
- **Importance**: Security foundation for web applications and APIs

### Authentication vs Authorization
```mermaid
graph TD
    A["Authentication"] --> B["Verify Identity"]
    A --> C["Login Process"]
    A --> D["Credentials Check"]

    E["Authorization"] --> F["Grant Permissions"]
    E --> G["Access Control"]
    E --> H["Role Management"]

    I["Process Flow"] --> J["Authentication"]
    J --> K["Authorization"]
    K --> L["Resource Access"]
```

### Authentication Methods
```mermaid
graph TD
    A["Authentication Methods"] --> B["Password-Based"]
    A --> C["Token-Based"]
    A --> D["Biometric"]
    A --> E["Multi-Factor"]
    A --> F["Certificate-Based"]

    B --> G["Username/Password"]
    C --> H["JWT, OAuth Tokens"]
    D --> I["Fingerprint, Face ID"]
    E --> J["SMS, Authenticator Apps"]
    F --> K["SSL Certificates"]
```

## Traditional Authentication

### Session-Based Authentication
```mermaid
graph TD
    A["User Login"] --> B["Server Validates Credentials"]
    B --> C["Create Session"]
    C --> D["Store Session ID"]
    D --> E["Send Session Cookie"]
    E --> F["Browser Stores Cookie"]
    F --> G["Subsequent Requests Include Cookie"]
    G --> H["Server Validates Session"]
```

**Session Authentication Process**:
1. **Login**: User submits username/password
2. **Validation**: Server verifies credentials
3. **Session Creation**: Server creates session record
4. **Cookie Storage**: Session ID stored in browser cookie
5. **Validation**: Each request validates session ID

#### Pros and Cons
| Aspect | Session Authentication |
|--------|---------------------|
| **Pros** | Simple to implement, server-controlled |
| **Cons** | Server memory usage, scaling issues |
| **Scalability** | Requires session synchronization |
| **Security** | Vulnerable to session hijacking |

### Cookie-Based Authentication
```http
// Set-Cookie Header Example
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict; Max-Age=3600

// Cookie in Request Headers
Cookie: session_id=abc123
```

**Cookie Security Attributes**:
- **HttpOnly**: Prevents JavaScript access
- **Secure**: Only sent over HTTPS
- **SameSite**: CSRF protection
- **Max-Age**: Cookie expiration time

## Token-Based Authentication

### JSON Web Tokens (JWT)
```mermaid
graph TD
    A["JWT Structure"] --> B["Header"]
    A --> C["Payload"]
    A --> D["Signature"]

    E["Header"] --> F["Algorithm"]
    E --> G["Token Type"]

    H["Payload"] --> I["Claims"]
    H --> J["User Data"]
    H --> K["Metadata"]

    L["Signature"] --> M["HMAC"]
    L --> N["RSA"]
```

#### JWT Structure
```json
// Header
{
  "alg": "HS256",
  "typ": "JWT"
}

// Payload
{
  "sub": "1234567890",
  "name": "John Doe",
  "email": "john@example.com",
  "iat": 1516239022,
  "exp": 1516242622
}

// Complete JWT
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiZW1haWwiOiJqb2huQGV4YW1wbGUuY29tIiwiaWF0IjoxNTE2MjM5MDIyLCJleHAiOjE1MTYyNDI2MjJ9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

#### JWT Claims
| Claim | Description | Example |
|-------|-------------|---------|
| **iss** | Issuer | `"https://api.example.com"` |
| **sub** | Subject (user ID) | `"12345"` |
| **aud** | Audience | `"myapp"` |
| **exp** | Expiration time | `1516242622` |
| **iat** | Issued at time | `1516239022` |
| **jti** | JWT ID | Unique identifier |

## OAuth 2.0

### OAuth 2.0 Overview
```mermaid
graph TD
    A["OAuth 2.0"] --> B["Authorization Framework"]
    A --> C["Delegated Access"]
    A --> D["Third-Party Access"]
    A --> E["Token-Based"]

    F["OAuth Roles"] --> G["Resource Owner"]
    F --> H["Client"]
    F --> I["Authorization Server"]
    F --> J["Resource Server"]
```

### OAuth 2.0 Roles
- **Resource Owner**: User who owns the data
- **Client**: Application requesting access
- **Authorization Server**: Issues access tokens
- **Resource Server**: Hosts protected resources

### OAuth 2.0 Grant Types
```mermaid
graph TD
    A["OAuth 2.0 Grant Types"] --> B["Authorization Code"]
    A --> C["Implicit"]
    A --> D["Client Credentials"]
    A --> E["Resource Owner Password Credentials"]

    F["Use Cases"] --> G["Web Apps"]
    F --> H["Single-Page Apps"]
    F --> I["Machine-to-Machine"]
    F --> J["Trusted Apps"]
```

#### 1. Authorization Code Grant
```mermaid
graph TD
    A["User"] --> B["Client App"]
    B --> C["Authorization Request"]
    C --> D["Authorization Server"]
    D --> E["User Consent"]
    E --> F["Authorization Code"]
    F --> G["Client App"]
    G --> H["Token Request"]
    H --> I["Access Token"]
    I --> J["Resource Server"]
```

**Authorization Code Flow**:
1. **Authorization Request**: Client redirects to authorization server
2. **User Consent**: User grants permission
3. **Authorization Code**: Server returns authorization code
4. **Token Exchange**: Client exchanges code for access token
5. **Resource Access**: Client uses token to access resources

#### 2. Client Credentials Grant
```mermaid
graph TD
    A["Client Application"] --> B["Token Request"]
    B --> C["Authorization Server"]
    C --> D["Access Token"]
    D --> E["Resource Server"]

    F["Use Case"] --> G["Machine-to-Machine"]
    F --> H["No User Interaction"]
    F --> I["Service Authentication"]
```

#### 3. Implicit Grant (Legacy)
- **Note**: No longer recommended due to security concerns
- **Alternative**: Use Authorization Code with PKCE

## OAuth 2.0 Implementation

### Authorization Code Flow Example
```mermaid
graph TD
    A["User clicks 'Login with Google'"] --> B["Redirect to Google"]
    B --> C["User authenticates with Google"]
    C --> D["Google redirects back with code"]
    D --> E["App exchanges code for tokens"]
    E --> F["App accesses Google APIs"]
```

**Implementation Steps**:
1. **Register Application**: Get client ID and secret
2. **Authorization Request**: Redirect user to auth server
3. **Handle Callback**: Receive authorization code
4. **Token Exchange**: Exchange code for access token
5. **API Calls**: Use access token for requests

### Token Storage and Security
```mermaid
graph TD
    A["Token Storage"] --> B["Server-Side"]
    A --> C["Client-Side"]

    B --> D["Database"]
    B --> E["Redis Cache"]

    C --> F["Local Storage"]
    C --> G["Session Storage"]
    C --> H["Memory"]

    I["Security Considerations"] --> J["HTTPS Only"]
    I --> K["Short Expiration"]
    I --> L["Secure Storage"]
    I --> M["Token Refresh"]
```

## API Security Best Practices

### Authentication Security
```mermaid
graph TD
    A["Authentication Security"] --> B["Password Policies"]
    A --> C["Multi-Factor Authentication"]
    A --> D["Rate Limiting"]
    A --> E["Account Lockout"]
    A --> F["Secure Transmission"]

    G["Password Security"] --> H["Strong Requirements"]
    G --> I["Hashing and Salting"]
    G --> J["Regular Expiration"]
```

### Password Security
#### Password Hashing
```python
# Python Example (bcrypt)
import bcrypt

# Hash password
password = "user_password123"
hashed = bcrypt.hashpw(password.encode('utf-8'), bcrypt.gensalt())

# Verify password
if bcrypt.checkpw(password.encode('utf-8'), hashed):
    print("Password matches")
else:
    print("Password doesn't match")
```

#### Password Policy Requirements
- **Length**: Minimum 8-12 characters
- **Complexity**: Uppercase, lowercase, numbers, special characters
- **History**: Prevent reuse of recent passwords
- **Expiration**: Regular password changes

### Multi-Factor Authentication (MFA)
```mermaid
graph TD
    A["MFA Factors"] --> B["Knowledge"]
    A --> C["Possession"]
    A --> D["Inherence"]

    B --> E["Password", "PIN", "Security Questions"]
    C --> F["Phone", "Hardware Token", "Authenticator App"]
    D --> G["Fingerprint", "Face Recognition", "Voice"]

    H["MFA Flow"] --> I["Primary Authentication"]
    I --> J["Second Factor Verification"]
    J --> K["Access Granted"]
```

**MFA Implementation Types**:
- **SMS-based**: One-time codes via text message
- **Authenticator Apps**: TOTP (Google Authenticator, Authy)
- **Hardware Tokens**: Physical devices (YubiKey)
- **Biometric**: Fingerprint, face recognition

### Session Management
```mermaid
graph TD
    A["Session Security"] --> B["Session ID Generation"]
    A --> C["Session Storage"]
    A --> D["Session Expiration"]
    A --> E["Session Invalidation"]

    F["Session Threats"] --> G["Session Hijacking"]
    F --> H["Session Fixation"]
    F --> I["Cross-Site Scripting"]
```

#### Session Security Best Practices
- **Secure Session IDs**: Cryptographically random
- **HTTPOnly Cookies**: Prevent JavaScript access
- **Secure Flag**: Only transmit over HTTPS
- **Session Timeout**: Automatic expiration
- **Session Re-generation**: Prevent fixation attacks

## API Security Implementation

### Authentication Middleware
```javascript
// Node.js Express Middleware Example
const jwt = require('jsonwebtoken');

function authenticateToken(req, res, next) {
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1];

    if (!token) {
        return res.status(401).json({
            error: 'Access token required'
        });
    }

    jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
        if (err) {
            return res.status(403).json({
                error: 'Invalid or expired token'
            });
        }

        req.user = user;
        next();
    });
}

// Protected route
app.get('/api/profile', authenticateToken, (req, res) => {
    res.json({ user: req.user });
});
```

### API Key Authentication
```mermaid
graph TD
    A["API Key Authentication"] --> B["Key Generation"]
    A --> C["Key Distribution"]
    A --> D["Key Validation"]
    A --> E["Key Rotation"]

    F["API Key Methods"] --> G["Header-based"]
    F --> H["Query Parameter"]
    F --> I["Client Certificate"]
```

#### API Key Implementation
```javascript
// API Key Middleware
function validateApiKey(req, res, next) {
    const apiKey = req.headers['x-api-key'];

    if (!apiKey) {
        return res.status(401).json({
            error: 'API key required'
        });
    }

    // Validate API key against database
    if (!isValidApiKey(apiKey)) {
        return res.status(401).json({
            error: 'Invalid API key'
        });
    }

    req.apiKey = apiKey;
    next();
}
```

### Rate Limiting
```mermaid
graph TD
    A["Rate Limiting"] --> B["Fixed Window"]
    A --> C["Sliding Window"]
    A --> D["Token Bucket"]
    A --> E["Leaky Bucket"]

    F["Implementation"] --> G["Redis-based"]
    F --> H["Memory-based"]
    F --> I["Database-based"]
```

#### Rate Limiting Implementation
```javascript
// Rate Limiting Middleware (express-rate-limit)
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
    message: {
        error: 'Too many requests, please try again later'
    },
    standardHeaders: true,
    legacyHeaders: false
});

app.use('/api/', limiter);
```

## Security Threats and Prevention

### Common Authentication Threats
```mermaid
graph TD
    A["Authentication Threats"] --> B["Brute Force Attacks"]
    A --> C["Credential Stuffing"]
    A --> D["Phishing"]
    A --> E["Session Hijacking"]
    A --> F["Man-in-the-Middle"]

    G["Prevention Strategies"] --> H["Rate Limiting"]
    G --> I["Account Lockout"]
    G --> J["Multi-Factor Authentication"]
    G --> K["Secure Transmission"]
```

### OWASP Authentication Top 10
1. **Broken Authentication**: Poor session management
2. **Sensitive Data Exposure**: Unencrypted credentials
3. **Broken Access Control**: Authorization failures
4. **Security Misconfiguration**: Default credentials
5. **Cross-Site Scripting**: Session token theft

### Security Headers
```http
// Important Security Headers
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Content-Security-Policy: default-src 'self'
```

## Common Interview Questions

### Basic Questions

**Q1: What is the difference between authentication and authorization?**
```mermaid
graph TD
    A["Authentication"] --> B["Who are you?"]
    A --> C["Identity Verification"]

    D["Authorization"] --> E["What can you do?"]
    D --> F["Permission Granting"]
```

**Q2: Explain JWT structure and its components**
- **Header**: Algorithm and token type
- **Payload**: Claims and user data
- **Signature**: Cryptographic signature for verification

**Q3: What is OAuth 2.0 and why is it used?**
- **Purpose**: Delegated authorization framework
- **Use Case**: Third-party applications access user data
- **Benefit**: No need to share user credentials

### Intermediate Questions

**Q4: Compare session-based vs token-based authentication**
```mermaid
graph TD
    A["Session-Based"] --> B["Server-side storage"]
    A --> C["Cookie-based"]
    A --> D["Stateful"]

    E["Token-Based"] --> F["Client-side storage"]
    E --> G["Stateless"]
    E --> H["JWT/OAuth"]
```

**Q5: How would you implement secure password storage?**
- **Hashing**: One-way cryptographic hash (bcrypt, Argon2)
- **Salting**: Random salt added to each password
- **Pepper**: Server-wide secret added to all passwords
- **Iteration**: Multiple hashing rounds

### Advanced Questions

**Q6: Explain OAuth 2.0 authorization code flow**
```mermaid
graph TD
    A["Client"] --> B["Authorization Request"]
    B --> C["Authorization Server"]
    C --> D["User Consent"]
    D --> E["Authorization Code"]
    E --> F["Client"]
    F --> G["Token Exchange"]
    G --> H["Access Token"]
```

**Q7: How would you secure REST APIs?**
- **HTTPS**: Encrypt all communication
- **Authentication**: JWT or OAuth 2.0
- **Authorization**: Role-based access control
- **Rate Limiting**: Prevent abuse
- **Input Validation**: Prevent injection attacks
- **Security Headers**: Additional protection layers

## Quick Reference

### Authentication Methods Comparison
| Method | Pros | Cons | Use Case |
|--------|------|------|----------|
| **Session** | Simple, server-controlled | Memory intensive, scaling issues | Traditional web apps |
| **JWT** | Stateless, scalable | Token size, no revocation | APIs, microservices |
| **OAuth 2.0** | Third-party access | Complex implementation | Social login, APIs |
| **MFA** | Enhanced security | User friction | High-security apps |

### Common HTTP Status Codes for Auth
| Code | Meaning | Use Case |
|------|---------|---------|
| **200** | Success | Valid credentials |
| **201** | Created | New user registration |
| **400** | Bad Request | Invalid input |
| **401** | Unauthorized | Missing/invalid credentials |
| **403** | Forbidden | Insufficient permissions |
| **429** | Too Many Requests | Rate limit exceeded |
| **500** | Internal Error | Server error |

### Security Checklist
| Area | Item | Status |
|------|------|--------|
| **Passwords** | Hashed and salted | ✅ |
| **Transmission** | HTTPS only | ✅ |
| **Tokens** | Short expiration | ✅ |
| **Rate Limiting** | Implemented | ✅ |
| **MFA** | Available | ✅ |
| **Security Headers** | Configured | ✅ |

### Interview Preparation Tips

1. **Understand authentication fundamentals** clearly
2. **Know different authentication methods** and use cases
3. **Practice OAuth 2.0 flows** and implementations
4. **Understand security best practices** and threats
5. **Be able to compare** different approaches

### Common Mistakes to Avoid

1. **Storing passwords in plaintext**
2. **Not using HTTPS for authentication**
3. **Ignoring rate limiting and abuse prevention**
4. **Improper token storage and handling**
5. **Neglecting security headers and best practices**

---

**Important Note**: Authentication and authorization are critical security components. Always follow security best practices and stay updated with the latest security standards and threats. Focus on understanding the concepts behind the implementations rather than just memorizing code.