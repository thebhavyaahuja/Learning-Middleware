# Architecture Overview

Understanding how Learning Middleware works under the hood.

---

## Table of Contents
- [System Architecture](#system-architecture)
- [Design Principles](#design-principles)
- [Service Communication](#service-communication)
- [Data Flow](#data-flow)
- [Technology Choices](#technology-choices)

---

## System Architecture

Learning Middleware uses a **microservices architecture** where each service handles a specific domain:

```
┌──────────────────────────────────────────────────────────────────┐
│                         Frontend Layer                            │
│                         (Next.js App)                            │
│                                                                  │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐│
│  │  Learner Portal │  │ Instructor Portal│  │ Shared Components││
│  │  - Browse       │  │  - Create Courses│  │  - Auth          ││
│  │  - Learn        │  │  - Upload Files  │  │  - Navigation    ││
│  │  - Quiz         │  │  - Analytics     │  │  - Chat          ││
│  └─────────────────┘  └─────────────────┘  └─────────────────┘│
└──────────────────────────────────────────────────────────────────┘
                                │
                    ┌───────────┼───────────┐
                    │           │           │
                    ▼           ▼           ▼
┌──────────────────────────────────────────────────────────────────┐
│                        Backend Services                           │
│                        (FastAPI Apps)                            │
│                                                                  │
│  ┌────────────────┐  ┌────────────────┐  ┌──────────────────┐ │
│  │ Learner Service│  │Instructor Svc. │  │   Orchestrator   │ │
│  │ Port: 8002     │  │ Port: 8003     │  │   Port: 8001     │ │
│  │                │  │                │  │                  │ │
│  │ • Auth & JWT   │  │ • Course CRUD  │  │ • Business Logic │ │
│  │ • Enrollment   │  │ • File Upload  │  │ • Service Coord. │ │
│  │ • Progress     │  │ • LO Gen Trig. │  │ • Preferences    │ │
│  │ • Dashboard    │  │ • Analytics    │  │ • Analytics      │ │
│  └────────────────┘  └────────────────┘  └──────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                          AI/ML Layer                              │
│                        (SME Service)                             │
│                         Port: 8000                               │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │  LO Generator│  │Module Creator│  │ Quiz Creator │         │
│  │              │  │              │  │              │         │
│  │ • Parse LOs  │  │ • RAG Retriev│  │ • RAG Retriev│         │
│  │ • LLM Call   │  │ • LLM Prompt │  │ • Question Gen│         │
│  │              │  │ • Format MD  │  │ • MCQ Format │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│                                                                  │
│  ┌──────────────────────────────────────────┐                  │
│  │         RAG Infrastructure               │                  │
│  │                                          │                  │
│  │  • FAISS Vector Store                   │                  │
│  │  • Sentence Transformers (embeddings)   │                  │
│  │  • LangChain (orchestration)            │                  │
│  │  • vLLM Client (inference)              │                  │
│  └──────────────────────────────────────────┘                  │
└──────────────────────────────────────────────────────────────────┘
                                │
        ┌───────────────────────┴──────────────────────┐
        ▼                                              ▼
┌────────────────────────┐                   ┌──────────────────┐
│    PostgreSQL          │                   │    MongoDB       │
│    Port: 5432          │                   │    Port: 27017   │
│                        │                   │                  │
│  • Instructors         │                   │ • Preferences    │
│  • Learners            │                   │ • File Metadata  │
│  • Courses             │                   │ • Generated LOs  │
│  • Modules             │                   │ • Vector Store   │
│  • Enrollments         │                   │   Status         │
│  • Progress Tracking   │                   │                  │
│  • Generated Content   │                   │                  │
│  • Quizzes & Scores    │                   │                  │
└────────────────────────┘                   └──────────────────┘
```

---

## Design Principles

### 1. Separation of Concerns
Each service has a **single, well-defined responsibility**:

- **Learner Service**: Everything related to learners (auth, enrollment, progress)
- **Instructor Service**: Everything instructors do (courses, uploads, analytics)
- **Orchestrator Service**: Business logic that coordinates multiple services
- **SME Service**: AI/ML operations isolated from business logic

**Why?** This makes services:
- Easier to understand and maintain
- Independently deployable
- Scalable based on demand
- Testable in isolation

### 2. API-First Design
All services expose **RESTful APIs** with:
- OpenAPI/Swagger documentation
- Consistent error handling
- Versioned endpoints (`/api/v1/...`)
- JSON request/response

**Why?** Enables:
- Multiple frontends (web, mobile, desktop)
- Third-party integrations
- Automated testing
- Clear contracts between services

### 3. Database-per-Service Pattern
Each service owns its data:
- **PostgreSQL**: Relational data (courses, users, progress)
- **MongoDB**: Flexible data (preferences, file metadata)
- **Local Files**: Uploaded course materials
- **Vector Stores**: FAISS indexes per course

**Why?** Ensures:
- Services don't create tight coupling through shared databases
- Each service can choose the right database for its needs
- Independent scaling and optimization

### 4. Caching Strategy
**Cache everything expensive to generate:**

```
First Request:            Subsequent Requests:
┌─────────────┐          ┌─────────────┐
│ Generate    │          │ Check Cache │
│ with LLM    │ ──────▶  │ Return Fast │
│ (2-3 min)   │          │ (<100ms)    │
└─────────────┘          └─────────────┘
      │                        ▲
      ▼                        │
┌─────────────┐               │
│ Save to DB  │───────────────┘
└─────────────┘
```

Cached items:
- ✅ Module content (per learner + module)
- ✅ Quizzes (per learner + module)
- ✅ Learning objectives (per course + module)
- ✅ Vector stores (per course)

**Why?** 
- Fast response times
- Cost efficiency (fewer LLM calls)
- Consistent experience (same quiz every time)

### 5. Asynchronous Processing
Long-running tasks run in the background:

```
User Request                Background Processing
     │                             │
     │  Upload Files               │
     ├────────────▶                │
     │                             │
     │  202 Accepted               │
     │◀────────────                │
     │                             │
     │                      Create Vector Store
     │                             │
     │  Poll Status                │
     ├────────────▶                │
     │  "creating"                 │
     │◀────────────                │
     │                             │
     │  Poll Status         ✓ Done │
     ├────────────▶                │
     │  "ready"                    │
     │◀────────────                │
```

**Why?**
- UI stays responsive
- Better user experience
- Can handle large files/datasets

---

## Service Communication

### Synchronous (HTTP)
Most service-to-service communication uses HTTP:

```
┌────────┐  HTTP  ┌──────────────┐  HTTP  ┌─────┐
│   UI   │───────▶│ Orchestrator │───────▶│ SME │
└────────┘        └──────────────┘        └─────┘
                         │
                         │ HTTP
                         ▼
                  ┌──────────────┐
                  │Learner Service│
                  └──────────────┘
```

**When to use:**
- Request/response patterns
- Immediate feedback needed
- Transactional operations

### Patterns Used

**1. Backend for Frontend (BFF)**
The Orchestrator acts as a BFF, aggregating calls:

```javascript
// Instead of UI making 3 separate calls:
const course = await getCourse(courseId)
const progress = await getProgress(learnerId, courseId)
const preferences = await getPreferences(learnerId, courseId)

// Orchestrator provides one call:
const dashboard = await getDashboard(learnerId, courseId)
// Returns: { course, progress, preferences, analytics }
```

**2. Circuit Breaker** (Recommended for production)
Prevent cascade failures:

```python
@circuit_breaker(failure_threshold=5, timeout=60)
async def call_sme_service(endpoint, data):
    try:
        response = await http_client.post(endpoint, json=data)
        return response.json()
    except Timeout:
        # Fall back to cached response or error
        return get_cached_or_error()
```

**3. Retry with Exponential Backoff**
Handle transient failures:

```python
@retry(max_attempts=3, backoff=exponential)
async def generate_module(data):
    return await sme_client.post("/generate-module", json=data)
```

---

## Data Flow

### Example: Learner Completes a Module

```
┌───────┐  1. Complete Module  ┌──────────────┐
│  UI   │─────────────────────▶│ Orchestrator │
└───────┘                      └──────────────┘
                                      │
                                      │ 2. Update Progress
                                      ▼
                               ┌──────────────┐
                               │Learner Service│
                               └──────────────┘
                                      │
                                      │ 3. Save to DB
                                      ▼
                               ┌──────────────┐
                               │  PostgreSQL  │
                               └──────────────┘
                                      │
                                      │ 4. Next Module?
                                      ▼
                               ┌──────────────┐
                               │ Check if     │
                               │ content exists│
                               └──────────────┘
                                      │
                    ┌─────────────────┴──────────────────┐
                    │                                    │
                    ▼ Exists                    Not Exists ▼
            ┌──────────────┐                  ┌──────────┐
            │ Return       │                  │Generate  │
            │ Cached       │                  │with SME  │
            └──────────────┘                  └──────────┘
                    │                                │
                    │                                │ 5. Retrieve Prefs
                    │                          ┌──────────────┐
                    │                          │   MongoDB    │
                    │                          └──────────────┘
                    │                                │
                    │                                │ 6. Generate
                    │                          ┌──────────────┐
                    │                          │SME Service   │
                    │                          │- Retrieve    │
                    │                          │  from Vector │
                    │                          │- Call LLM    │
                    │                          │- Format      │
                    │                          └──────────────┘
                    │                                │
                    │                                │ 7. Save
                    │                          ┌──────────────┐
                    │                          │  PostgreSQL  │
                    │                          └──────────────┘
                    │                                │
                    └────────────────┬───────────────┘
                                     │
                                     │ 8. Return to UI
                                     ▼
                              ┌──────────────┐
                              │  Next Module │
                              │   Content    │
                              └──────────────┘
```

---

## Technology Choices

### Why FastAPI?
✅ **High Performance**: Async/await support, on par with Node.js  
✅ **Auto Documentation**: OpenAPI/Swagger out of the box  
✅ **Type Safety**: Pydantic models catch errors early  
✅ **Modern Python**: Uses Python 3.10+ features  

### Why Next.js?
✅ **Server-Side Rendering**: Fast initial page loads  
✅ **API Routes**: Backend endpoints in the same codebase  
✅ **TypeScript**: Type safety across the stack  
✅ **Great DX**: Hot reload, file-based routing  

### Why PostgreSQL + MongoDB?
✅ **PostgreSQL**: 
- Relational integrity for courses, users, enrollments
- ACID transactions
- Complex queries with JOINs
  
✅ **MongoDB**:
- Flexible schema for preferences (may evolve)
- Fast writes for file metadata
- Document storage for nested data

### Why FAISS?
✅ **Speed**: Sub-second search across millions of vectors  
✅ **Local**: No external dependencies  
✅ **Efficient**: Optimized memory usage  
✅ **Proven**: Used by major AI applications  

### Why Docker Compose?
✅ **Reproducible**: Same environment everywhere  
✅ **Easy Setup**: One command to start everything  
✅ **Isolated**: Services don't interfere  
✅ **Production-Like**: Similar to Kubernetes  

---

## Scalability Considerations

### Horizontal Scaling
Each service can scale independently:

```
                      Load Balancer
                          │
        ┌─────────────────┼─────────────────┐
        ▼                 ▼                 ▼
    Learner-1         Learner-2         Learner-3
    Service           Service           Service
```

**Stateless Services**: All services are stateless (state in DB)  
**Scale Strategy**:
- More Learner/Instructor services → Handle more concurrent users
- More Orchestrator instances → Handle more business logic
- More SME instances → Handle more AI generation (with shared vector stores)

### Database Scaling
- **PostgreSQL**: Read replicas for queries, write to primary
- **MongoDB**: Sharding by course_id for preferences
- **FAISS**: One index per course, loaded on demand

### Caching Layer (Future)
Add Redis for:
- Session storage
- API response caching
- Rate limiting

---

## Security Architecture

### Authentication Flow
```
┌────────┐                          ┌──────────────┐
│   UI   │  1. Login (email/pwd)    │Learner Service│
│        │─────────────────────────▶│              │
│        │                          │ Verify       │
│        │  2. JWT Token            │ Hash         │
│        │◀─────────────────────────│              │
└────────┘                          └──────────────┘
    │
    │ 3. API Request + JWT
    │
    ▼
┌──────────────┐  4. Verify JWT  ┌──────────────┐
│ Orchestrator │◀────────────────│  JWT Library │
└──────────────┘                 └──────────────┘
```

### Security Layers
1. **Transport**: HTTPS in production
2. **Authentication**: JWT tokens (HS256)
3. **Authorization**: Role-based (learner/instructor)
4. **Input Validation**: Pydantic models
5. **SQL Injection**: SQLAlchemy ORM (parameterized queries)
6. **Secrets**: Environment variables, not in code

---

## What's Next?

- **[Core Concepts](./concepts.md)** — Deep dive into personalization and RAG
- **[Service Architecture](./services.md)** — Detailed view of each service
- **[Data Flow](./data-flow.md)** — Complete request/response flows
- **[Database Schema](./database.md)** — Data models explained

---

*Last updated: November 8, 2025*
