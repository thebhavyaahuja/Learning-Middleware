# API Documentation

Complete reference for Learning Middleware APIs.

---

## Overview

Learning Middleware exposes RESTful APIs for all operations. Each microservice provides its own API with OpenAPI/Swagger documentation.

### API Services

| Service | Port | Base URL | Purpose |
|---------|------|----------|---------|
| **Learner API** | 8002 | `/api/v1/learner` | Authentication, enrollment, progress |
| **Instructor API** | 8003 | `/api/v1/instructor` | Course management, file uploads |
| **Orchestrator API** | 8001 | `/api/orchestrator` | Business logic, content generation |
| **SME API** | 8000 | `/` | AI content generation, RAG |

### Interactive Documentation

Each service provides interactive Swagger UI:

- **Learner API**: http://localhost:8002/docs
- **Instructor API**: http://localhost:8003/docs
- **Orchestrator API**: http://localhost:8001/docs
- **SME API**: http://localhost:8000/docs

---

## Quick Start

### Authentication

Most endpoints require JWT authentication:

```bash
# 1. Login to get token
curl -X POST "http://localhost:8002/api/v1/learner/login-json" \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password123"}' \
  | jq -r '.access_token'

# 2. Use token in subsequent requests
curl "http://localhost:8002/api/v1/learner/me" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

### Common Headers

```bash
# JSON requests
Content-Type: application/json

# Authentication
Authorization: Bearer <jwt_token>

# File uploads
Content-Type: multipart/form-data
```

### Response Format

All APIs return JSON:

```json
// Success (200, 201)
{
  "data": {...},
  "message": "Operation successful"
}

// Error (4xx, 5xx)
{
  "detail": "Error message"
}
```

---

## API Guides

### For Learners
- **[Learner API Reference](./learner-api.md)** - Complete endpoint documentation
  - Authentication (signup, login, me)
  - Course browsing & enrollment
  - Progress tracking
  - Module access
  - Quiz submission
  - Dashboard data

### For Instructors
- **[Instructor API Reference](./instructor-api.md)** - Complete endpoint documentation
  - Authentication (signup, login, me)
  - Course CRUD operations
  - File upload & management
  - Vector store management
  - Learning objective generation
  - Analytics

### For Orchestration
- **[Orchestrator API Reference](./orchestrator-api.md)** - Complete endpoint documentation
  - Module content generation
  - Quiz generation
  - Preference management
  - Analytics aggregation
  - Feedback collection

### For AI/ML
- **[SME API Reference](./sme-api.md)** - Complete endpoint documentation
  - Learning objective generation
  - Module content generation
  - Quiz generation (RAG)
  - Chat/tutoring (RAG)
  - Vector store operations

---

## API Design Principles

### RESTful Standards

```
GET    /resources       → List all
POST   /resources       → Create new
GET    /resources/:id   → Get one
PUT    /resources/:id   → Update
DELETE /resources/:id   → Delete
```

### Versioning

All APIs are versioned in the URL:
```
/api/v1/learner/...
/api/v1/instructor/...
```

**Why?** Enables backwards compatibility when making breaking changes.

### HTTP Status Codes

| Code | Meaning | Usage |
|------|---------|-------|
| 200 | OK | Successful GET, PUT, DELETE |
| 201 | Created | Successful POST |
| 400 | Bad Request | Invalid input |
| 401 | Unauthorized | Missing/invalid token |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource doesn't exist |
| 422 | Unprocessable Entity | Validation error |
| 500 | Internal Server Error | Server error |

### Error Handling

Consistent error format:

```json
{
  "detail": "Human-readable error message"
}

// Validation errors
{
  "detail": [
    {
      "loc": ["body", "email"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

### Pagination

For list endpoints with many results:

```bash
GET /api/v1/learner/courses?skip=0&limit=20

# Response includes
{
  "items": [...],
  "total": 150,
  "skip": 0,
  "limit": 20
}
```

### Filtering & Sorting

```bash
# Filter by field
GET /api/v1/instructor/courses?status=published

# Sort by field
GET /api/v1/learner/courses?sort=created_at&order=desc

# Combine
GET /api/v1/instructor/courses?status=published&sort=enrollments&order=desc
```

---

## Authentication Flow

### JWT Token Authentication

```
┌─────────┐                              ┌─────────────┐
│ Client  │                              │   Server    │
└─────────┘                              └─────────────┘
     │                                           │
     │  POST /login                              │
     │  {email, password}                        │
     ├──────────────────────────────────────────>│
     │                                           │
     │                                    Verify credentials
     │                                    Generate JWT
     │                                           │
     │  200 OK                                   │
     │  {access_token, token_type}               │
     │<──────────────────────────────────────────┤
     │                                           │
     │  Store token                              │
     │                                           │
     │  GET /protected-resource                  │
     │  Authorization: Bearer <token>            │
     ├──────────────────────────────────────────>│
     │                                           │
     │                                    Verify JWT
     │                                    Extract user_id
     │                                    Process request
     │                                           │
     │  200 OK                                   │
     │  {data}                                   │
     │<──────────────────────────────────────────┤
```

### Token Structure

```javascript
// JWT Payload
{
  "sub": "user@example.com",  // Subject (user identifier)
  "exp": 1699459200,           // Expiration timestamp
  "iat": 1699372800,           // Issued at timestamp
  "type": "learner"            // User type
}
```

### Token Refresh

**Current**: Tokens expire after 30 minutes  
**Future Enhancement**: Refresh token flow

```bash
# When token expires, login again
curl -X POST "http://localhost:8002/api/v1/learner/login-json" \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password123"}'
```

---

## Rate Limiting

**Current**: No rate limiting  
**Recommended for Production**: Implement rate limiting per endpoint

```python
# Example rate limit configuration
rate_limits = {
    "auth": "5/minute",      # Login attempts
    "content": "100/hour",   # Content generation
    "api": "1000/hour"       # General API calls
}
```

---

## CORS Configuration

Cross-Origin Resource Sharing (CORS) is enabled for frontend access:

```python
# Current configuration
origins = [
    "http://localhost:3000",  # Development
    "http://localhost:3001",  # Alternative dev port
]

# Production: Configure with your domain
origins = [
    "https://yourdomain.com",
    "https://www.yourdomain.com"
]
```

---

## API Client Examples

### Python

```python
import requests

class LearnerClient:
    def __init__(self, base_url, email, password):
        self.base_url = base_url
        self.token = self._login(email, password)
    
    def _login(self, email, password):
        response = requests.post(
            f"{self.base_url}/api/v1/learner/login-json",
            json={"email": email, "password": password}
        )
        return response.json()["access_token"]
    
    def _headers(self):
        return {"Authorization": f"Bearer {self.token}"}
    
    def get_courses(self):
        response = requests.get(
            f"{self.base_url}/api/v1/learner/courses",
            headers=self._headers()
        )
        return response.json()
    
    def enroll(self, course_id):
        response = requests.post(
            f"{self.base_url}/api/v1/learner/enroll",
            json={"courseid": course_id},
            headers=self._headers()
        )
        return response.json()

# Usage
client = LearnerClient("http://localhost:8002", "user@example.com", "password")
courses = client.get_courses()
client.enroll("COURSE_101")
```

### JavaScript/TypeScript

```typescript
class LearnerAPI {
  private baseURL: string;
  private token: string | null = null;

  constructor(baseURL: string) {
    this.baseURL = baseURL;
  }

  async login(email: string, password: string): Promise<void> {
    const response = await fetch(`${this.baseURL}/api/v1/learner/login-json`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password })
    });
    const data = await response.json();
    this.token = data.access_token;
  }

  private getHeaders(): HeadersInit {
    return {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${this.token}`
    };
  }

  async getCourses(): Promise<Course[]> {
    const response = await fetch(`${this.baseURL}/api/v1/learner/courses`, {
      headers: this.getHeaders()
    });
    return response.json();
  }

  async enroll(courseId: string): Promise<any> {
    const response = await fetch(`${this.baseURL}/api/v1/learner/enroll`, {
      method: 'POST',
      headers: this.getHeaders(),
      body: JSON.stringify({ courseid: courseId })
    });
    return response.json();
  }
}

// Usage
const api = new LearnerAPI('http://localhost:8002');
await api.login('user@example.com', 'password');
const courses = await api.getCourses();
await api.enroll('COURSE_101');
```

### cURL

```bash
#!/bin/bash

# Configuration
BASE_URL="http://localhost:8002"
EMAIL="user@example.com"
PASSWORD="password123"

# Login and get token
TOKEN=$(curl -s -X POST "$BASE_URL/api/v1/learner/login-json" \
  -H "Content-Type: application/json" \
  -d "{\"email\":\"$EMAIL\",\"password\":\"$PASSWORD\"}" \
  | jq -r '.access_token')

echo "Token: $TOKEN"

# Get courses
curl -s "$BASE_URL/api/v1/learner/courses" \
  -H "Authorization: Bearer $TOKEN" \
  | jq '.'

# Enroll in course
curl -s -X POST "$BASE_URL/api/v1/learner/enroll" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"courseid":"COURSE_101"}' \
  | jq '.'
```

---

## Testing APIs

### Postman Collection

Import the Postman collection:
```bash
docs/postman_collection.json
```

Features:
- Pre-configured requests for all endpoints
- Environment variables for tokens
- Example requests and responses

### Automated Testing

```bash
# Run API tests
cd tests
pytest api_tests/

# Test specific service
pytest api_tests/test_learner_api.py
pytest api_tests/test_instructor_api.py
```

### Manual Testing

Use Swagger UI for interactive testing:
1. Go to http://localhost:8002/docs
2. Click **"Authorize"**
3. Enter JWT token
4. Try any endpoint

---

## Performance Considerations

### Response Times

**Target latencies:**
```
Authentication:        < 200ms
Data retrieval (GET):  < 500ms
Data creation (POST):  < 1000ms
Content generation:    2-3 minutes (async)
Quiz generation:       2-5 minutes (async)
Vector store creation: 5-20 minutes (async)
```

### Optimization Tips

**1. Cache aggressively**
```python
# Example: Cache course list
from functools import lru_cache

@lru_cache(maxsize=128)
def get_all_courses():
    return db.query(Course).all()
```

**2. Use pagination**
```bash
# Don't fetch all at once
GET /courses?skip=0&limit=20

# Implement infinite scroll
GET /courses?skip=20&limit=20
```

**3. Async operations**
```python
# Don't block on slow operations
POST /generate-quiz
→ Returns 202 Accepted immediately
→ Check status later with GET /quiz-status/{id}
```

**4. Batch requests**
```bash
# Instead of N individual requests
for course in courses:
    GET /courses/{course.id}

# Make one request
POST /courses/batch
{"course_ids": ["1", "2", "3", ...]}
```

---

## Security Best Practices

### API Security Checklist

- ✅ **HTTPS in production** (TLS 1.2+)
- ✅ **JWT token authentication**
- ✅ **Input validation** (Pydantic models)
- ✅ **SQL injection prevention** (SQLAlchemy ORM)
- ✅ **CORS configuration** (restrict origins)
- ⚠️ **Rate limiting** (recommended)
- ⚠️ **API key rotation** (for LLM endpoints)
- ⚠️ **Request logging** (for audit trails)

### Securing Endpoints

```python
# Require authentication
@router.get("/protected")
async def protected_route(current_user: User = Depends(get_current_user)):
    return {"user": current_user}

# Role-based access
@router.post("/admin-only")
async def admin_route(current_user: User = Depends(get_current_admin)):
    return {"action": "admin_stuff"}
```

### Token Security

```bash
# Store tokens securely
# ✅ In httpOnly cookies (web)
# ✅ In secure storage (mobile)
# ❌ Never in localStorage (XSS risk)
# ❌ Never in URL parameters
# ❌ Never commit to git
```

---

## API Changelog

### v1.0.0 (Current)
- Initial stable release
- All core endpoints
- JWT authentication
- OpenAPI documentation

### Future Versions

**v1.1.0 (Planned)**
- Token refresh endpoint
- Batch operations
- Webhook support

**v2.0.0 (Future)**
- GraphQL API option
- Realtime updates (WebSockets)
- Advanced filtering

---

## Support & Resources

- **📖 Full API Docs**: See individual service pages
- **🐛 Report Issues**: [GitHub Issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues)
- **💬 Ask Questions**: [Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)
- **📧 Email**: [Support Email]

---

## Next Steps

Explore detailed API documentation for each service:

- 👨‍🎓 **[Learner API](./learner-api.md)** - For building learner-facing features
- 👨‍🏫 **[Instructor API](./instructor-api.md)** - For building course management tools
- 🔀 **[Orchestrator API](./orchestrator-api.md)** - For complex workflows
- 🤖 **[SME API](./sme-api.md)** - For AI/ML integrations

---

*Last updated: November 8, 2025*
