# Chapter 4: Authentication Methods

## Basic Auth

### What is Basic Authentication?

Simplest authentication method where credentials are sent as Base64-encoded string in HTTP headers.

### Basic Auth Flow

```mermaid
sequenceDiagram
    participant Client
    participant Server

    Client->>Server: Request without auth
    Server->>Client: 401 Unauthorized + WWW-Authenticate header
    Client->>Server: Request with Authorization header
    Server->>Client: 200 OK + Resource
```

### Basic Auth Request

```http
GET /api/users HTTP/1.1
Host: example.com
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

### Encoding Process

```javascript
// Credentials: username:password
const credentials = "username:password";
const encoded = btoa(credentials); // Base64 encode
// Result: "dXNlcm5hbWU6cGFzc3dvcmQ="
```

### Server Implementation

```javascript
// Express.js Basic Auth Middleware
const basicAuth = (req, res, next) => {
  const authHeader = req.headers.authorization;

  if (!authHeader) {
    res.setHeader('WWW-Authenticate', 'Basic realm="API"');
    return res.status(401).json({ error: 'Authentication required' });
  }

  try {
    const credentials = Buffer.from(
      authHeader.split(' ')[1],
      'base64'
    ).toString('ascii');

    const [username, password] = credentials.split(':');

    // Validate credentials
    if (!validateUser(username, password)) {
      return res.status(401).json({ error: 'Invalid credentials' });
    }

    req.user = { username };
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid authentication format' });
  }
};
```

### Basic Auth Advantages

| Advantage | Description |
|-----------|-------------|
| **Simple** | Easy to implement and understand |
| **Standard** | HTTP standard, built-in browser support |
| **Universal** | Works with any HTTP client |
| **No Server State** | Stateless, easy to scale |

### Basic Auth Disadvantages

| Disadvantage | Description |
|--------------|-------------|
| **Insecure** | Credentials sent with every request |
| **Plain Text** | Base64 is encoding, not encryption |
| **No Logout** | Credentials cached by browser |
| **Limited Control** | Fine-grained access control difficult |

### When to Use Basic Auth

| Use Case | Why Basic Auth Works |
|----------|---------------------|
| **Internal APIs** | Controlled environment |
| **Simple services** | Minimal security needs |
| **Testing/Development** | Quick implementation |
| **Machine-to-machine** | Automated scripts |

---

## Token Based Authentication

### What is Token-Based Auth?

Authentication method where server issues a token after initial login, and client includes this token in subsequent requests.

### Token Authentication Flow

```mermaid
flowchart TD
    A[Client] -->|1. Login with credentials| B[Server]
    B -->|2. Validate credentials| C[Database]
    C -->|3. User valid| B
    B -->|4. Generate token| A
    A -->|5. Store token locally| A
    A -->|6. Request with token| D[API Endpoint]
    D -->|7. Validate token| B
    B -->|8. Token valid| D
    D -->|9. Return resource| A
```

### Token Generation and Storage

#### **Login Endpoint**

```javascript
app.post('/api/auth/login', async (req, res) => {
  const { username, password } = req.body;

  // Validate credentials
  const user = await validateCredentials(username, password);
  if (!user) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  // Generate token
  const token = generateToken(user);

  // Store token (optional)
  await storeToken(token, user.id);

  res.json({
    token,
    user: { id: user.id, username: user.username }
  });
});
```

#### **Token Validation Middleware**

```javascript
const authenticateToken = async (req, res, next) => {
  const authHeader = req.headers.authorization;
  const token = authHeader && authHeader.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'Access token required' });
  }

  try {
    // Validate token
    const user = await validateToken(token);
    if (!user) {
      return res.status(401).json({ error: 'Invalid token' });
    }

    req.user = user;
    next();
  } catch (error) {
    res.status(403).json({ error: 'Invalid token format' });
  }
};
```

#### **Simple Token Implementation**

```javascript
const crypto = require('crypto');
const tokens = new Map(); // In production, use Redis/database

function generateToken(user) {
  const token = crypto.randomBytes(32).toString('hex');
  const tokenData = {
    userId: user.id,
    createdAt: new Date(),
    expiresAt: new Date(Date.now() + 24 * 60 * 60 * 1000) // 24 hours
  };

  tokens.set(token, tokenData);
  return token;
}

async function validateToken(token) {
  const tokenData = tokens.get(token);

  if (!tokenData || tokenData.expiresAt < new Date()) {
    tokens.delete(token);
    return null;
  }

  const user = await getUserById(tokenData.userId);
  return user;
}
```

### Token Storage Options

| Method | Pros | Cons | Use Case |
|--------|------|------|----------|
| **Database** | Persistent, queryable | Performance overhead | Small applications |
| **Redis** | Fast, TTL support | Memory usage | High-performance APIs |
| **JWT** | Self-contained, no storage | Larger tokens | Distributed systems |
| **Memory** | Fastest | Not scalable, no persistence | Development/testing |

### Token Management

#### **Logout Endpoint**

```javascript
app.post('/api/auth/logout', authenticateToken, async (req, res) => {
  const token = req.headers.authorization.split(' ')[1];
  await invalidateToken(token);
  res.json({ message: 'Logged out successfully' });
});
```

#### **Token Refresh**

```javascript
app.post('/api/auth/refresh', async (req, res) => {
  const { refreshToken } = req.body;

  // Validate refresh token
  const tokenData = await validateRefreshToken(refreshToken);
  if (!tokenData) {
    return res.status(401).json({ error: 'Invalid refresh token' });
  }

  // Generate new access token
  const newToken = generateToken(tokenData.user);

  res.json({ token: newToken });
});
```

---

## JWT (JSON Web Tokens)

### What is JWT?

Self-contained token format that contains user information and claims, digitally signed for verification.

### JWT Structure

```
Header.Payload.Signature
```

#### **Example JWT**

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

#### **Decoded Structure**

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
  "iat": 1516239022
}

// Signature (not shown - cryptographic hash)
```

### JWT Components

#### **Header**

```json
{
  "alg": "HS256",    // Algorithm (HS256, RS256, ES256)
  "typ": "JWT",      // Token type
  "kid": "key123"    // Key ID (for multiple keys)
}
```

#### **Payload (Claims)**

| Claim Type | Description | Example |
|------------|-------------|---------|
| **Registered** | Standard claims | `sub`, `iss`, `aud`, `exp`, `iat` |
| **Public** | Custom but standardized | `name`, `email` |
| **Private** | Application-specific | `role`, `permissions` |

```json
{
  "sub": "1234567890",           // Subject (user ID)
  "iss": "https://api.example.com", // Issuer
  "aud": "https://client.example.com", // Audience
  "exp": 1516239022,             // Expiration
  "iat": 1516239022,             // Issued at
  "nbf": 1516239022,             // Not before
  "jti": "abc123",               // JWT ID
  "name": "John Doe",            // User name
  "email": "john@example.com",   // User email
  "role": "user",                // User role
  "permissions": ["read", "write"] // Permissions
}
```

### JWT Algorithms

#### **HS256 (HMAC with SHA-256)**
- **Type**: Symmetric
- **Usage**: Same secret for signing and verification
- **Best for**: Single server applications

```javascript
const jwt = require('jsonwebtoken');

const token = jwt.sign(
  { userId: 123, role: 'user' },
  'your-secret-key',
  { expiresIn: '24h' }
);

const decoded = jwt.verify(token, 'your-secret-key');
```

#### **RS256 (RSA with SHA-256)**
- **Type**: Asymmetric
- **Usage**: Private key signs, public key verifies
- **Best for**: Distributed systems, microservices

```javascript
// Sign with private key
const token = jwt.sign(
  { userId: 123, role: 'user' },
  privateKey,
  { algorithm: 'RS256', expiresIn: '24h' }
);

// Verify with public key
const decoded = jwt.verify(token, publicKey);
```

### JWT Implementation

#### **Token Generation**

```javascript
const jwt = require('jsonwebtoken');
const crypto = require('crypto');

function generateJWT(user) {
  const payload = {
    sub: user.id,
    username: user.username,
    email: user.email,
    role: user.role,
    permissions: user.permissions,
    jti: crypto.randomUUID() // Unique token ID
  };

  const options = {
    issuer: 'https://api.example.com',
    audience: 'https://client.example.com',
    expiresIn: '1h',
    notBefore: 0
  };

  return jwt.sign(payload, process.env.JWT_SECRET, options);
}
```

#### **Token Validation Middleware**

```javascript
const authenticateJWT = (req, res, next) => {
  const authHeader = req.headers.authorization;
  const token = authHeader && authHeader.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'Access token required' });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    if (error.name === 'TokenExpiredError') {
      return res.status(401).json({
        error: 'Token expired',
        code: 'TOKEN_EXPIRED'
      });
    }

    if (error.name === 'JsonWebTokenError') {
      return res.status(401).json({
        error: 'Invalid token',
        code: 'TOKEN_INVALID'
      });
    }

    res.status(403).json({ error: 'Token verification failed' });
  }
};
```

### JWT Best Practices

| Practice | Implementation |
|----------|----------------|
| **Short expiration** | 15 minutes for access tokens |
| **Refresh tokens** | Separate long-lived tokens |
| **Secure storage** | HttpOnly cookies for web |
| **HTTPS only** | Prevent token interception |
| **Strong secrets** | Minimum 256-bit secrets |
| **Rotation** | Regular key rotation schedule |

### JWT vs Regular Tokens

| Feature | JWT | Regular Token |
|---------|-----|--------------|
| **State** | Self-contained | Requires database lookup |
| **Size** | Larger (claims included) | Smaller |
| **Verification** | Cryptographic signature | Database validation |
| **Performance** | Fast (no DB lookup) | Slower (DB required) |
| **Revocation** | Difficult | Easy (remove from DB) |
| **Distribution** | Easy (stateless) | Complex (shared DB) |

---

## OAuth 2.0

### What is OAuth 2.0?

Authorization framework that allows applications to obtain limited access to user accounts on an HTTP service.

### OAuth 2.0 Roles

| Role | Description | Example |
|------|-------------|---------|
| **Resource Owner** | User who owns the data | Facebook user |
| **Client Application** | App requesting access | Photo editing app |
| **Authorization Server** | Issues access tokens | Facebook OAuth server |
| **Resource Server** | Hosts protected resources | Facebook API |

### OAuth 2.0 Grant Types

#### **1. Authorization Code Grant**

```mermaid
sequenceDiagram
    participant User
    participant Client as Client App
    participant Auth as Authorization Server
    participant Resource as Resource Server

    User->>Client: Access protected resource
    Client->>Auth: Redirect to authorize endpoint
    Auth->>User: Show consent screen
    User->>Auth: Grant permission
    Auth->>Client: Redirect with authorization code
    Client->>Auth: Exchange code for access token
    Auth->>Client: Return access token
    Client->>Resource: Request resource with token
    Resource->>Client: Return protected resource
```

**Implementation Example:**

```javascript
// Step 1: Authorization Request
app.get('/auth/facebook', (req, res) => {
  const authUrl = 'https://www.facebook.com/v18.0/dialog/oauth?' +
    'client_id=' + process.env.FACEBOOK_CLIENT_ID +
    '&redirect_uri=' + encodeURIComponent('http://localhost:3000/auth/facebook/callback') +
    '&scope=email,public_profile' +
    '&response_type=code';

  res.redirect(authUrl);
});

// Step 2: Handle Callback
app.get('/auth/facebook/callback', async (req, res) => {
  const { code } = req.query;

  // Exchange code for access token
  const tokenResponse = await axios.post('https://graph.facebook.com/v18.0/oauth/access_token', {
    client_id: process.env.FACEBOOK_CLIENT_ID,
    client_secret: process.env.FACEBOOK_CLIENT_SECRET,
    redirect_uri: 'http://localhost:3000/auth/facebook/callback',
    code: code
  });

  const { access_token, refresh_token } = tokenResponse.data;

  // Get user info
  const userResponse = await axios.get('https://graph.facebook.com/me', {
    params: {
      fields: 'id,name,email',
      access_token: access_token
    }
  });

  // Create/update user and issue JWT
  const user = await findOrCreateUser(userResponse.data);
  const jwtToken = generateJWT(user);

  res.json({ token: jwtToken, user });
});
```

#### **2. Client Credentials Grant**

```javascript
// For machine-to-machine communication
app.post('/auth/token', async (req, res) => {
  const { client_id, client_secret, grant_type } = req.body;

  // Validate client credentials
  const client = await validateClient(client_id, client_secret);
  if (!client) {
    return res.status(401).json({ error: 'Invalid client credentials' });
  }

  // Issue access token
  const accessToken = generateAccessToken(client);

  res.json({
    access_token: accessToken,
    token_type: 'Bearer',
    expires_in: 3600
  });
});
```

#### **3. Resource Owner Password Credentials Grant**

```javascript
app.post('/auth/password', async (req, res) => {
  const { username, password, client_id, client_secret } = req.body;

  // Validate client
  const client = await validateClient(client_id, client_secret);
  if (!client) {
    return res.status(401).json({ error: 'Invalid client credentials' });
  }

  // Validate user credentials
  const user = await validateUserCredentials(username, password);
  if (!user) {
    return res.status(401).json({ error: 'Invalid user credentials' });
  }

  // Issue tokens
  const accessToken = generateAccessToken(user);
  const refreshToken = generateRefreshToken(user);

  res.json({
    access_token: accessToken,
    refresh_token: refreshToken,
    token_type: 'Bearer',
    expires_in: 3600
  });
});
```

### OAuth 2.0 Implementation

#### **Access Token Endpoint**

```javascript
app.post('/oauth/token', async (req, res) => {
  const { grant_type, code, redirect_uri, client_id, client_secret } = req.body;

  if (grant_type !== 'authorization_code') {
    return res.status(400).json({ error: 'unsupported_grant_type' });
  }

  // Validate authorization code
  const authCode = await validateAuthorizationCode(code, client_id);
  if (!authCode) {
    return res.status(400).json({ error: 'invalid_grant' });
  }

  // Generate access token
  const accessToken = generateAccessToken(authCode.userId);
  const refreshToken = generateRefreshToken(authCode.userId);

  // Invalidate authorization code
  await invalidateAuthorizationCode(code);

  res.json({
    access_token: accessToken,
    token_type: 'Bearer',
    expires_in: 3600,
    refresh_token: refreshToken,
    scope: authCode.scope
  });
});
```

#### **Token Introspection**

```javascript
app.post('/oauth/introspect', async (req, res) => {
  const { token } = req.body;

  const tokenData = await validateAccessToken(token);

  if (!tokenData) {
    return res.json({ active: false });
  }

  res.json({
    active: true,
    scope: tokenData.scope,
    client_id: tokenData.clientId,
    username: tokenData.username,
    exp: tokenData.expiresAt.getTime() / 1000,
    iat: tokenData.createdAt.getTime() / 1000
  });
});
```

### OAuth 2.0 vs JWT

| Feature | OAuth 2.0 | JWT |
|---------|------------|-----|
| **Purpose** | Authorization framework | Token format |
| **Scope** | Delegated access | Claims and metadata |
| **Revocation** | Built-in revocation | Difficult (blacklist needed) |
| **Token Types** | Access + refresh tokens | Single token type |
| **Complexity** | Complex implementation | Simple |
| **Use Case** | Third-party access | API authentication |

---

## Session Based Authentication

### What is Session-Based Auth?

Authentication method where server creates a session after login and stores session data, returning a session identifier to the client.

### Session Authentication Flow

```mermaid
sequenceDiagram
    participant Client
    participant Server
    participant SessionStore

    Client->>Server: Login with credentials
    Server->>SessionStore: Create session with session ID
    SessionStore-->>Server: Return session ID
    Server->>Client: Set-Cookie: sessionId=abc123
    Client->>Server: Request with session cookie
    Server->>SessionStore: Lookup session by ID
    SessionStore-->>Server: Return session data
    Server-->>Client: Response
```

### Session Implementation

#### **Express.js Session Setup**

```javascript
const express = require('express');
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redis = require('redis');

const app = express();

// Redis client for session storage
const redisClient = redis.createClient({
  host: 'localhost',
  port: 6379
});

// Session middleware
app.use(session({
  store: new RedisStore({ client: redisClient }),
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: process.env.NODE_ENV === 'production',
    httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000 // 24 hours
  }
}));
```

#### **Login Handler**

```javascript
app.post('/api/auth/login', async (req, res) => {
  const { username, password } = req.body;

  // Validate credentials
  const user = await validateCredentials(username, password);
  if (!user) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  // Create session
  req.session.userId = user.id;
  req.session.username = user.username;
  req.session.role = user.role;
  req.session.loginTime = new Date();

  res.json({
    message: 'Login successful',
    user: { id: user.id, username: user.username }
  });
});
```

#### **Authentication Middleware**

```javascript
const requireAuth = (req, res, next) => {
  if (!req.session || !req.session.userId) {
    return res.status(401).json({ error: 'Authentication required' });
  }

  // Optionally refresh session
  req.session.touch();
  next();
};
```

#### **Logout Handler**

```javascript
app.post('/api/auth/logout', (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      return res.status(500).json({ error: 'Logout failed' });
    }

    res.clearCookie('sessionId');
    res.json({ message: 'Logged out successfully' });
  });
});
```

### Session Storage Options

| Storage Type | Pros | Cons | Use Case |
|--------------|------|------|----------|
| **Memory** | Fastest | Not scalable, server restart loses data | Development |
| **Database** | Persistent, scalable | Performance overhead | Small applications |
| **Redis** | Fast, scalable, TTL support | Memory usage | Production apps |
| **File System** | Simple | Performance issues, scaling problems | Small sites |

### Redis Session Store Implementation

```javascript
const Redis = require('ioredis');
const { promisify } = require('util');

class SessionStore {
  constructor(options = {}) {
    this.redis = new Redis(options.redis);
    this.prefix = options.prefix || 'sess:';
    this.ttl = options.ttl || 86400; // 24 hours
  }

  async get(sid) {
    const data = await this.redis.get(this.prefix + sid);
    return data ? JSON.parse(data) : null;
  }

  async set(sid, sess, ttl = this.ttl) {
    await this.redis.setex(
      this.prefix + sid,
      ttl,
      JSON.stringify(sess)
    );
  }

  async destroy(sid) {
    await this.redis.del(this.prefix + sid);
  }

  async touch(sid, sess, ttl = this.ttl) {
    await this.redis.setex(
      this.prefix + sid,
      ttl,
      JSON.stringify(sess)
    );
  }
}

// Usage
const sessionStore = new SessionStore({
  redis: { host: 'localhost', port: 6379 },
  prefix: 'myapp:sess:',
  ttl: 3600
});
```

### Session Security

#### **Session Fixation Prevention**

```javascript
app.post('/api/auth/login', async (req, res) => {
  // Regenerate session to prevent fixation
  req.session.regenerate((err) => {
    if (err) return res.status(500).json({ error: 'Session error' });

    // Set session data
    req.session.userId = user.id;
    req.session.loginTime = new Date();

    res.json({ message: 'Login successful' });
  });
});
```

#### **Session Hijacking Protection**

```javascript
app.use((req, res, next) => {
  // Check IP address
  if (req.session.ipAddress && req.session.ipAddress !== req.ip) {
    req.session.destroy();
    return res.status(401).json({ error: 'Session invalidated' });
  }

  // Check User-Agent
  if (req.session.userAgent && req.session.userAgent !== req.get('User-Agent')) {
    req.session.destroy();
    return res.status(401).json({ error: 'Session invalidated' });
  }

  // Store session info
  req.session.ipAddress = req.ip;
  req.session.userAgent = req.get('User-Agent');

  next();
});
```

### Session vs Token Authentication

| Feature | Session-Based | Token-Based |
|---------|---------------|-------------|
| **State** | Server-side | Client-side |
| **Scalability** | Requires shared session store | Stateless |
| **Storage** | Database/Redis | Client storage |
| **Revocation** | Easy (remove from store) | Difficult (blacklist) |
| **Performance** | Slower (DB lookup) | Faster (signature verification) |
| **Mobile** | Cookie limitations | Header-based works well |
| **CORS** | Complex with cookies | Simple with headers |

### Session Management Best Practices

| Practice | Implementation |
|----------|----------------|
| **Secure cookies** | `secure: true`, `httpOnly: true` |
| **Session timeout** | Set reasonable TTL |
| **Regenerate on login** | Prevent session fixation |
| **HTTPS only** | Prevent session hijacking |
| **Store minimal data** | Reduce memory usage |
| **Clean expired sessions** | Prevent memory leaks |

---

## Authentication Method Comparison

### Decision Matrix

| Factor | Basic Auth | Token Auth | JWT | OAuth 2.0 | Session Auth |
|--------|------------|------------|-----|-----------|--------------|
| **Security** | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Scalability** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| **Complexity** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ |
| **State Management** | Stateless | Server state | Self-contained | Tokens + server | Server state |
| **Mobile Support** | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| **Third-party Access** | ❌ | ❌ | ❌ | ✅ | ❌ |

### When to Use Each Method

#### **Basic Auth**
- **Internal APIs** with controlled access
- **Simple scripts** and automation
- **Development environments**
- **Legacy system integration**

#### **Token-Based Auth**
- **Single-page applications**
- **Mobile applications**
- **Microservices architecture**
- **API-first development**

#### **JWT**
- **Distributed systems**
- **Stateless architectures**
- **Cross-service authentication**
- **Mobile and web applications**

#### **OAuth 2.0**
- **Third-party integration**
- **Social login** (Google, Facebook, GitHub)
- **API access delegation**
- **Enterprise SSO**

#### **Session-Based Auth**
- **Traditional web applications**
- **Server-rendered pages**
- **Simple authentication needs**
- **Admin panels**

### Security Best Practices

| Practice | Implementation |
|----------|----------------|
| **HTTPS Only** | Prevent credential interception |
| **Strong Passwords** | Minimum complexity requirements |
| **Rate Limiting** | Prevent brute force attacks |
| **Account Lockout** | After failed login attempts |
| **Multi-Factor Auth** | Additional security layer |
| **Audit Logging** | Track authentication events |
| **Token Expiration** | Short-lived access tokens |
| **Secure Storage** | Don't store credentials in code |

---

## Interview Questions

### **Q1: What is the difference between Authentication and Authorization?**
**Answer:**
- **Authentication**: Who you are (verify identity)
- **Authorization**: What you can do (check permissions)
- **Analogy**: Authentication = showing your ID, Authorization = checking if you have the right key for a room

### **Q2: When would you use JWT over session-based authentication?**
**Answer:**
**Use JWT when:**
- Microservices architecture (stateless)
- Mobile applications
- Cross-domain authentication
- Self-contained tokens needed
- High scalability required

**Use Sessions when:**
- Traditional web applications
- Need for immediate token revocation
- Simple authentication flow
- Server-rendered pages

### **Q3: What are the main components of OAuth 2.0?**
**Answer:**
OAuth 2.0 has four main roles:
1. **Resource Owner**: User who owns the data
2. **Client Application**: App requesting access
3. **Authorization Server**: Issues access tokens
4. **Resource Server**: Hosts protected resources

Common grant types: Authorization Code, Client Credentials, Resource Owner Password Credentials.

### **Q4: What are the security risks of Basic Authentication and how to mitigate them?**
**Answer:**
**Risks:**
- Credentials sent in plain text (Base64 encoding, not encryption)
- Credentials cached by browsers
- No logout mechanism
- Susceptible to man-in-the-middle attacks

**Mitigation:**
- Always use HTTPS
- Implement rate limiting
- Use strong passwords
- Consider two-factor authentication
- Add IP-based restrictions

### **Q5: What is the difference between access token and refresh token?**
**Answer:**
**Access Token:**
- Short-lived (minutes to hours)
- Used for API requests
- Can be self-contained (JWT)
- Higher risk if compromised

**Refresh Token:**
- Long-lived (days to months)
- Used to get new access tokens
- Stored securely on server side
- Lower risk, can be revoked

### **Q6: How do you implement token expiration and refresh?**
**Answer:**
```javascript
// Generate tokens with different expirations
const accessToken = jwt.sign(payload, secret, { expiresIn: '15m' });
const refreshToken = jwt.sign({ userId }, refreshSecret, { expiresIn: '7d' });

// Refresh endpoint
app.post('/refresh', (req, res) => {
  const { refreshToken } = req.body;
  const decoded = jwt.verify(refreshToken, refreshSecret);
  const newAccessToken = jwt.sign({ userId: decoded.userId }, secret, { expiresIn: '15m' });
  res.json({ accessToken: newAccessToken });
});
```

### **Q7: What are the security considerations for JWT implementation?**
**Answer:**
**Key Security Points:**
- Use strong signing keys (minimum 256-bit)
- Choose appropriate algorithm (RS256 for distributed systems)
- Implement short expiration times
- Use refresh tokens for longer sessions
- Validate token structure and claims
- Implement token blacklisting for revocation
- Secure token storage (HttpOnly cookies, secure storage)

---

## Quick Tips & Best Practices

### **General Authentication**
✅ Always use HTTPS for authentication
✅ Implement proper error handling without information leakage
✅ Use strong password policies
✅ Implement rate limiting to prevent brute force attacks
✅ Log authentication attempts for security monitoring

### **Token Management**
✅ Use short-lived access tokens
✅ Implement refresh tokens for better security
✅ Store tokens securely on client side
✅ Include expiration and issuer information
✅ Implement token revocation mechanisms

### **JWT Specific**
✅ Use appropriate signing algorithm
✅ Include necessary claims only
✅ Validate all token fields
✅ Implement proper key management
✅ Consider token size for performance

### **Session Management**
✅ Use secure, HttpOnly cookies
✅ Implement session timeout
✅ Regenerate session IDs on login
✅ Store minimal session data
✅ Implement session cleanup

### **OAuth 2.0**
✅ Use PKCE for public clients
✅ Validate redirect URIs
✅ Implement proper scope management
✅ Use state parameter to prevent CSRF
✅ Secure client credentials

---

## Chapter Summary

Chapter 4 explores different authentication methods for securing APIs:

### **Authentication Methods Overview**

| Method | Best For | Key Features |
|--------|----------|--------------|
| **Basic Auth** | Internal APIs, simple needs | HTTP standard, easy implementation |
| **Token-Based** | SPAs, mobile apps | Stateless, scalable, custom storage |
| **JWT** | Distributed systems | Self-contained, claims-based, stateless |
| **OAuth 2.0** | Third-party access | Delegated authorization, social login |
| **Session-Based** | Traditional web apps | Server-side state, browser cookies |

### **Key Concepts**

- **Authentication vs Authorization**: Identity verification vs permission checking
- **Token Management**: Access tokens, refresh tokens, expiration
- **Security Considerations**: HTTPS, storage, revocation, rate limiting
- **State vs Stateless**: Server-side sessions vs self-contained tokens
- **Scalability**: Distributed authentication needs

### **Implementation Patterns**

Each authentication method provides different trade-offs between security, complexity, and scalability. Choose based on your specific application requirements, security needs, and infrastructure constraints.

Authentication is the foundation of API security, providing the mechanism to verify client identities before enforcing access control policies.