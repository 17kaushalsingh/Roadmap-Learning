# Chapter 2: System Design Fundamentals

## System Design Basics for Freshers

### What is System Design?
- **Definition**: Process of defining architecture, components, and relationships of a system
- **Purpose**: Create scalable, maintainable, and efficient systems
- **Fresher Focus**: Low-Level Design (LLD) rather than High-Level Design (HLD)

### Key System Design Concepts

#### 1. Scalability
**Definition**: System's ability to handle growth in users, data, and requests

```mermaid
graph TD
    A["Single Server"] --> B["Load Balancer"]
    B --> C["Server 1"]
    B --> D["Server 2"]
    B --> E["Server 3"]
    C --> F["Shared Database"]
    D --> F
    E --> F
```

**Types of Scaling**:
- **Vertical Scaling**: Add more resources to single server
  - Pros: Simple to implement
  - Cons: Limited by hardware capacity
- **Horizontal Scaling**: Add more servers
  - Pros: Unlimited growth, better fault tolerance
  - Cons: Complex coordination, higher costs

#### 2. Load Balancing
**Purpose**: Distribute incoming requests across multiple servers

```mermaid
graph TD
    A["Users"] --> B["Load Balancer"]
    B --> C["Server A"]
    B --> D["Server B"]
    B --> E["Server C"]

    F["Load Balancing Algorithms"] --> G["Round Robin"]
    F --> H["Least Connections"]
    F --> I["IP Hash"]
```

**Common Algorithms**:
- **Round Robin**: Requests distributed evenly
- **Least Connections**: Send to least busy server
- **IP Hash**: Same user always goes to same server

#### 3. Caching
**Purpose**: Store frequently accessed data to improve performance

```mermaid
graph TD
    A["User Request"] --> B["Cache"]
    B --> C{"Cache Hit?"}
    C -->|Yes| D["Return Cached Data"]
    C -->|No| E["Fetch from Database"]
    E --> F["Update Cache"]
    F --> G["Return Data"]
```

**Cache Types**:
- **Client-Side**: Browser cache, CDN
- **Server-Side**: Redis, Memcached
- **Database**: Query caching

#### 4. Database Design Choices
```mermaid
graph TD
    A["Application Needs"] --> B{"Structured Data?"}
    B -->|Yes| C["SQL Database"]
    B -->|No| D["NoSQL Database"]

    C --> E["MySQL, PostgreSQL"]
    D --> F["MongoDB, Cassandra"]

    G["ACID Required?"] -->|Yes| C
    G -->|No| D
```

| Database Type | When to Use | Examples |
|-------------|-------------|---------|
| **SQL** | Structured data, relationships, transactions | Banking, e-commerce |
| **NoSQL** | Unstructured data, high scalability | Social media, IoT |

## Basic Architecture Patterns

### 1. Monolithic Architecture
**Definition**: Single, unified application with all functionality

```mermaid
graph TD
    A["Web Layer"] --> B["Business Logic"]
    B --> C["Database Layer"]
    B --> D["Auth Module"]
    B --> E["Payment Module"]
    B --> F["Notification Module"]
```

**Pros**: Simple to develop, test, and deploy
**Cons**: Difficult to scale, technology limitations

### 2. Microservices Architecture
**Definition**: Collection of loosely coupled, independently deployable services

```mermaid
graph TD
    A["API Gateway"] --> B["User Service"]
    A --> C["Order Service"]
    A --> D["Product Service"]
    A --> E["Payment Service"]

    B --> F["User Database"]
    C --> G["Order Database"]
    D --> H["Product Database"]
```

**Pros**: Independent scaling, technology flexibility
**Cons**: Complex coordination, network latency

### 3. Client-Server Architecture
**Definition**: Clients request services, servers provide them

```mermaid
graph TD
    A["Client Devices"] --> B["Network"]
    B --> C["Server"]
    C --> D["Database"]
    C --> E["File Storage"]
    C --> F["External Services"]
```

**Components**:
- **Client**: User interface, application logic
- **Server**: Business logic, data processing
- **Database**: Data storage and retrieval

## Common Fresher System Design Questions

### 1. Design a URL Shortener

#### Requirements
- Convert long URLs to short URLs
- Redirect short URLs to original URLs
- Handle high traffic efficiently
- Track click statistics

#### System Components
```mermaid
graph TD
    A["User"] --> B["URL Shortener Service"]
    B --> C["Hash Function"]
    B --> D["Database"]
    B --> E["Cache (Redis)"]

    F["Short URL Request"] --> G["Database Lookup"]
    G --> H["Redirect to Long URL"]

    I["Analytics"] --> J["Click Tracking"]
    I --> K["Statistics Dashboard"]
```

#### Key Design Decisions

**URL Generation**:
- **Base 62 Encoding**: Uses 0-9, a-z, A-Z (62 characters)
- **Hash Function**: Converts long URL to unique short code
- **Collision Handling**: Regenerate if code already exists

```mermaid
graph TD
    A["Long URL Input"] --> B["Generate Hash"]
    B --> C{"Collision?"}
    C -->|Yes| D["Regenerate Hash"]
    C -->|No| E["Store Mapping"]
    D --> B
```

**Database Schema**:
```sql
CREATE TABLE url_mappings (
    short_code VARCHAR(10) PRIMARY KEY,
    long_url TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    click_count INT DEFAULT 0
);

CREATE TABLE analytics (
    id INT PRIMARY KEY AUTO_INCREMENT,
    short_code VARCHAR(10),
    ip_address VARCHAR(45),
    user_agent TEXT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (short_code) REFERENCES url_mappings(short_code)
);
```

**API Design**:
```mermaid
graph TD
    A["POST /shorten"] --> B["Request Body: long_url"]
    A --> C["Response: short_url"]

    D["GET /:shortCode"] --> E["Redirect: 301 Moved Permanently"]

    F["GET /analytics/:shortCode"] --> G["Response: click_count, analytics"]
```

### 2. Design a Parking Lot System

#### Requirements
- Multiple vehicle types (Car, Motorcycle, Truck)
- Different parking spot sizes
- Entry/exit management
- Payment calculation

#### System Architecture
```mermaid
graph TD
    A["Entrance Gate"] --> B["Parking Lot System"]
    C["Exit Gate"] --> B

    B --> D["Spot Management"]
    B --> E["Vehicle Management"]
    B --> F["Payment System"]

    D --> G["Compact Spots"]
    D --> H["Large Spots"]
    D --> I["Motorcycle Spots"]

    E --> J["Vehicle Entry"]
    E --> K["Vehicle Exit"]
    E --> L["Spot Assignment"]
```

#### Key Components

**Vehicle Types and Spot Matching**:
```mermaid
graph TD
    A["Vehicle"] --> B["Vehicle Type"]
    B --> C{"Required Spot Size"}

    C -->|Motorcycle| D["Motorcycle/Compact Spot"]
    C -->|Car| E["Compact/Large Spot"]
    C -->|Truck| F["Large Spot Only"]
```

**Entry/Exit Flow**:
```mermaid
graph TD
    A["Vehicle Arrives"] --> B["Generate Ticket"]
    B --> C["Find Available Spot"]
    C --> D{"Spot Found?"}
    D -->|Yes| E["Assign Spot"]
    D -->|No| F["Reject Entry"]
    E --> G["Record Entry Time"]
    G --> H["Raise Gate"]

    I["Vehicle Exits"] --> J["Scan Ticket"]
    J --> K["Calculate Duration"]
    K --> L["Calculate Payment"]
    L --> M["Process Payment"]
    M --> N["Update Spot Status"]
```

**Core Classes** (Interview Focus):
- `Vehicle`: License plate, type, entry time
- `ParkingSpot`: Spot ID, type, availability
- `ParkingLot`: Collection of spots, assignment logic
- `Ticket`: Vehicle info, spot assignment, entry/exit times

### 3. Design a Chat Application

#### Requirements
- Real-time messaging
- Multiple chat rooms
- User online/offline status
- Message history

#### System Architecture
```mermaid
graph TD
    A["Client 1"] --> B["WebSocket Server"]
    C["Client 2"] --> B
    D["Client 3"] --> B

    B --> E["Message Queue"]
    E --> F["Database"]
    E --> G["Notification Service"]

    H["Web App"] --> I["Load Balancer"]
    I --> J["Multiple WebSocket Servers"]
```

#### Key Design Components

**Real-time Communication**:
- **WebSocket**: Bidirectional communication
- **Message Queue**: Handle high message volume
- **Database**: Store message history
- **Load Balancer**: Distribute connections

**Message Flow**:
```mermaid
graph TD
    A["User Sends Message"] --> B["WebSocket Server"]
    B --> C["Message Queue"]
    C --> D["Database Storage"]
    C --> E["Recipients"]
    E --> F["WebSocket Servers"]
    F --> G["Receive Message"]
```

**Room Management**:
- Users can create/join rooms
- Messages delivered to room participants
- Online status tracking per room

### 4. Design a Library Management System

#### Requirements
- Book catalog management
- Member registration
- Book issue/return
- Fine calculation

#### System Components
```mermaid
graph TD
    A["Librarian"] --> B["Library System"]
    C["Member"] --> B

    B --> D["Book Management"]
    B --> E["Member Management"]
    B --> F["Issue Management"]

    D --> G["Add/Remove Books"]
    D --> H["Search Books"]
    D --> I["Track Availability"]

    F --> J["Issue Books"]
    F --> K["Return Books"]
    F --> L["Calculate Fines"]
```

#### Key Entities
```mermaid
erDiagram
    BOOK ||--o{ BOOK_ISSUE : issued_to
    MEMBER ||--o{ BOOK_ISSUE : issues
    BOOK {
        string isbn PK
        string title
        string author
        bool is_available
        int total_copies
        int available_copies
    }
    MEMBER {
        string member_id PK
        string name
        string email
        string phone
        int max_books_allowed
    }
    BOOK_ISSUE {
        string issue_id PK
        string isbn FK
        string member_id FK
        date issue_date
        date due_date
        date return_date
        decimal fine_amount
    }
```

## Design Principles for Interviews

### 1. Single Responsibility Principle
**Definition**: Each class should have one reason to change

**Example**: Separate user authentication from user profile management

### 2. Open/Closed Principle
**Definition**: Open for extension, closed for modification

**Example**: Use interfaces/abstract classes instead of modifying existing code

### 3. Dependency Inversion
**Definition**: Depend on abstractions, not concretions

**Example**: Program to interfaces, not implementations

## Interview Approach for System Design

### Step-by-Step Method

#### 1. Clarify Requirements
- Ask about functional requirements
- Understand scale (users, requests per second)
- Identify constraints (budget, timeline, technology)

#### 2. High-Level Design
- Identify major components
- Define data flow between components
- Choose appropriate architecture pattern

#### 3. Deep Dive
- Design each component in detail
- Consider data structures
- Handle edge cases

#### 4. Discuss Trade-offs
- Performance vs Complexity
- Consistency vs Availability (CAP theorem)
- Cost vs Scalability

### Common Interview Questions

#### Basic Questions
- "What is the difference between SQL and NoSQL databases?"
- "Why would you use caching?"
- "What is a load balancer and why is it important?"

#### Intermediate Questions
- "How would you handle database failover?"
- "What are the pros and cons of microservices?"
- "How would you design for 1 million concurrent users?"

#### Advanced Questions
- "How would you implement real-time features?"
- "What are the security considerations?"
- "How would you monitor system health?"

## Quick Reference

### Key Concepts Summary
| Concept | Definition | Interview Focus |
|---------|-------------|-----------------|
| **Scalability** | Handle growth | Vertical vs Horizontal scaling |
| **Load Balancing** | Distribute traffic | Algorithms and benefits |
| **Caching** | Store frequently accessed data | Cache strategies |
| **Database Design** | Data storage and retrieval | SQL vs NoSQL choices |

### Architecture Patterns
| Pattern | When to Use | Key Characteristics |
|---------|-------------|-------------------|
| **Monolithic** | Simple applications, small teams | Single codebase, easier deployment |
| **Microservices** | Large scale, independent teams | Service independence, scalability |
| **Client-Server** | Distributed systems | Clear separation of concerns |

### Interview Tips

1. **Start with requirements clarification**
2. **Draw diagrams** to communicate design
3. **Explain trade-offs** (pros/cons)
4. **Consider edge cases** and error handling
5. **Think about scalability** from the beginning

### Common Mistakes to Avoid

1. **Jumping to code** without understanding requirements
2. **Ignoring scalability** and performance concerns
3. **Not considering** different failure scenarios
4. **Over-engineering** simple solutions
5. **Forgetting to discuss** trade-offs and alternatives

---

**Important Note**: System design interviews focus on your thought process and problem-solving approach. Practice explaining your design decisions clearly and considering multiple perspectives. Always ask clarifying questions before starting the design.