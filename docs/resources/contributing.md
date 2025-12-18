# Contributing to Learning Middleware

Thank you for your interest in contributing to Learning Middleware! This guide will help you get started.

---

## Table of Contents
- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Making Changes](#making-changes)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Submitting Changes](#submitting-changes)
- [Community](#community)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inspiring community for all. Please:
- ✅ Be respectful and inclusive
- ✅ Accept constructive criticism gracefully
- ✅ Focus on what's best for the community
- ✅ Show empathy towards others

### Unacceptable Behavior

- ❌ Harassment, trolling, or discriminatory comments
- ❌ Personal attacks or insults
- ❌ Publishing others' private information
- ❌ Spamming or off-topic discussions

**Enforcement**: Violations may result in temporary or permanent ban from the project.

**Report**: Email [conduct@learningmiddleware.org] for Code of Conduct issues.

---

## How Can I Contribute?

### 1. Report Bugs 🐛

Found a bug? Help us improve!

**Before reporting:**
- Check [existing issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues)
- Try to reproduce with latest version
- Gather relevant information (logs, screenshots, steps to reproduce)

**Submit a bug report:**
1. Go to [Issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues/new)
2. Choose "Bug Report" template
3. Fill in all sections:
   - **Description**: What happened?
   - **Expected Behavior**: What should happen?
   - **Steps to Reproduce**: How to trigger the bug?
   - **Environment**: OS, Docker version, browser, etc.
   - **Logs**: Relevant error messages
   - **Screenshots**: If applicable

**Example:**
```markdown
**Bug**: Quiz generation fails for courses with > 500 pages of material

**Expected**: Quiz should generate successfully regardless of material size

**Steps to Reproduce**:
1. Create course with 600-page textbook PDF
2. Upload materials and create vector store
3. Attempt to generate quiz for Module 1
4. Request times out after 50 minutes

**Environment**:
- OS: Ubuntu 22.04
- Docker: 24.0.7
- RAM: 16GB
- Browser: Chrome 119

**Logs**:
```
[ERROR] SME Service: Quiz generation timeout after 3000 seconds
```

### 2. Suggest Features 💡

Have an idea? We'd love to hear it!

**Before suggesting:**
- Check [existing discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)
- Search closed issues (idea might have been declined)
- Think through implementation feasibility

**Submit a feature request:**
1. Go to [Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions/new)
2. Choose "Ideas" category
3. Describe:
   - **Problem**: What pain point does this solve?
   - **Solution**: Your proposed feature
   - **Alternatives**: Other approaches considered
   - **Use Case**: Who benefits and how?

**Example:**
```markdown
**Feature**: Video content support

**Problem**: Currently only supports text-based materials (PDFs). Many instructors have video lectures they want to use.

**Proposed Solution**:
- Accept video uploads (MP4, WebM)
- Extract audio → transcribe to text
- Generate embeddings from transcript
- Include in RAG vector store
- Display videos in module content with timestamps

**Use Cases**:
- Computer science courses with coding demonstrations
- Language courses with pronunciation videos
- Science courses with lab experiments

**Alternatives Considered**:
- Manual transcript upload (requires instructor effort)
- External video hosting only (no RAG integration)
```

### 3. Improve Documentation 📖

Documentation is crucial! Ways to help:

**Fix typos and errors:**
- Browse docs at `/docs/`
- Submit small fixes directly as PRs

**Clarify confusing sections:**
- Open an issue explaining what's unclear
- Or submit improved version as PR

**Add missing content:**
- Tutorials
- Examples
- Troubleshooting guides
- Translations

**Write blog posts:**
- Deployment guides
- Case studies
- Best practices
- Integration tutorials

### 4. Write Code 💻

Ready to contribute code? Awesome!

**Good first issues:**
- Look for [`good first issue`](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) label
- These are beginner-friendly and well-documented

**Areas needing help:**
- Frontend: UI/UX improvements, new components
- Backend: API endpoints, optimization
- AI/ML: Prompt engineering, model integration
- DevOps: Deployment scripts, monitoring
- Testing: Unit tests, integration tests

### 5. Test & Review 🧪

Help ensure quality:

**Manual testing:**
- Try out PRs and report feedback
- Test edge cases and error handling
- Check across browsers/devices

**Code review:**
- Review open pull requests
- Provide constructive feedback
- Test changes locally

**Performance testing:**
- Benchmark changes
- Report performance regressions
- Suggest optimizations

### 6. Help Others 🤝

Support the community:

**Answer questions:**
- Monitor [Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)
- Help in [Issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues)
- Write FAQ entries

**Create resources:**
- Tutorial videos
- Blog posts
- Sample courses/modules

---

## Development Setup

### Prerequisites

- **Git**: Version control
- **Docker** (20.10+) & **Docker Compose** (2.0+)
- **Python** (3.10+) for backend development
- **Node.js** (18+) & **pnpm** for frontend development
- **Code editor**: VS Code recommended

### Fork and Clone

```bash
# 1. Fork the repository on GitHub
# 2. Clone YOUR fork
git clone https://github.com/YOUR_USERNAME/Learning-Middleware-iREL.git
cd Learning-Middleware-iREL

# 3. Add upstream remote
git remote add upstream https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL.git

# 4. Verify remotes
git remote -v
# origin    https://github.com/YOUR_USERNAME/Learning-Middleware-iREL.git (fetch)
# origin    https://github.com/YOUR_USERNAME/Learning-Middleware-iREL.git (push)
# upstream  https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL.git (fetch)
# upstream  https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL.git (push)
```

### Environment Setup

```bash
# Copy example environment file
cp .env.example .env

# Edit with your configuration
# Especially: LLM endpoints, API keys
nano .env
```

### Start Development Environment

**Full stack (all services):**
```bash
docker compose up -d
```

**Individual service development:**

**Backend (Learner Service):**
```bash
cd learner
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
pip install -r requirements-dev.txt  # Testing/linting tools
uvicorn main:app --reload --port 8002
```

**Frontend (UI):**
```bash
cd ui
pnpm install
pnpm run dev
# Access at http://localhost:3000
```

### Verify Setup

```bash
# Check all services are healthy
docker compose ps

# Test API endpoints
curl http://localhost:8002/health  # Learner
curl http://localhost:8003/health  # Instructor
curl http://localhost:8001/        # Orchestrator
curl http://localhost:8000/        # SME
```

---

## Making Changes

### Branching Strategy

**Main branch**: `main` (production-ready code)  
**Development**: Feature branches from `main`

**Branch naming:**
```bash
# Features
git checkout -b feature/add-video-support

# Bug fixes
git checkout -b fix/quiz-generation-timeout

# Documentation
git checkout -b docs/update-api-guide

# Refactoring
git checkout -b refactor/optimize-database-queries
```

### Development Workflow

```bash
# 1. Sync with upstream
git checkout main
git pull upstream main

# 2. Create feature branch
git checkout -b feature/your-feature-name

# 3. Make changes
# ... edit files ...

# 4. Test changes
# Run tests, manual testing, etc.

# 5. Commit changes
git add .
git commit -m "feat: add video upload support"

# 6. Push to your fork
git push origin feature/your-feature-name

# 7. Open pull request on GitHub
```

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```bash
# Format
<type>(<scope>): <description>

[optional body]

[optional footer]

# Types
feat:     New feature
fix:      Bug fix
docs:     Documentation changes
style:    Code style (formatting, no logic change)
refactor: Code restructuring (no behavior change)
perf:     Performance improvement
test:     Adding or updating tests
chore:    Build process, dependencies, etc.

# Examples
feat(learner): add progress export to CSV
fix(sme): resolve quiz generation timeout
docs(api): update authentication examples
style(ui): format dashboard components
refactor(orchestrator): simplify service calls
perf(vector-store): optimize FAISS index creation
test(instructor): add file upload test cases
chore(deps): update FastAPI to 0.104.0
```

**Good commit:**
```bash
git commit -m "feat(quiz): add support for true/false questions

- Add TrueFalse question type to schema
- Update quiz generation logic
- Add frontend UI components
- Include tests and documentation

Closes #123"
```

**Bad commit:**
```bash
git commit -m "fixed stuff"
git commit -m "WIP"
git commit -m "asdfasdf"
```

---

## Coding Standards

### Python (Backend)

**Style Guide**: [PEP 8](https://peps.python.org/pep-0008/)

**Tools:**
```bash
# Format code
black .

# Sort imports
isort .

# Lint
flake8 .
pylint **/*.py

# Type checking
mypy .
```

**Example:**
```python
from typing import List, Optional

from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.orm import Session

from .database import get_db
from .models import Course
from .schemas import CourseCreate, CourseResponse

router = APIRouter(prefix="/api/v1/courses", tags=["courses"])


@router.post("/", response_model=CourseResponse, status_code=status.HTTP_201_CREATED)
async def create_course(
    course: CourseCreate,
    db: Session = Depends(get_db),
    current_user: User = Depends(get_current_user),
) -> Course:
    """
    Create a new course.

    Args:
        course: Course data
        db: Database session
        current_user: Authenticated user

    Returns:
        Created course

    Raises:
        HTTPException: If course creation fails
    """
    # Check permissions
    if not current_user.is_instructor:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Only instructors can create courses",
        )

    # Create course
    db_course = Course(**course.dict(), instructor_id=current_user.id)
    db.add(db_course)
    db.commit()
    db.refresh(db_course)

    return db_course
```

**Key points:**
- ✅ Type hints everywhere
- ✅ Docstrings for functions/classes
- ✅ Clear variable names
- ✅ Error handling
- ✅ Input validation

### TypeScript/JavaScript (Frontend)

**Style Guide**: [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)

**Tools:**
```bash
# Format
pnpm format

# Lint
pnpm lint

# Type check
pnpm type-check
```

**Example:**
```typescript
import { useState, useEffect } from 'react';
import { toast } from 'sonner';

import { Course } from '@/types';
import { fetchCourses } from '@/lib/api';

interface CourseListProps {
  instructorId?: string;
  limit?: number;
}

/**
 * Display a list of courses with optional filtering
 */
export function CourseList({ instructorId, limit = 10 }: CourseListProps) {
  const [courses, setCourses] = useState<Course[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    async function loadCourses() {
      try {
        setLoading(true);
        const data = await fetchCourses({ instructorId, limit });
        setCourses(data);
      } catch (err) {
        const message = err instanceof Error ? err.message : 'Failed to load courses';
        setError(message);
        toast.error(message);
      } finally {
        setLoading(false);
      }
    }

    loadCourses();
  }, [instructorId, limit]);

  if (loading) return <CourseSkeleton />;
  if (error) return <ErrorDisplay message={error} />;
  if (courses.length === 0) return <EmptyState />;

  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      {courses.map((course) => (
        <CourseCard key={course.id} course={course} />
      ))}
    </div>
  );
}
```

**Key points:**
- ✅ TypeScript for type safety
- ✅ Functional components with hooks
- ✅ Proper error handling
- ✅ Accessible components
- ✅ Responsive design

### SQL

**Style:**
```sql
-- Use uppercase for keywords
-- Indent subqueries
-- Meaningful aliases

SELECT
    c.courseid,
    c.course_name,
    COUNT(e.learnerid) AS enrollment_count,
    AVG(q.score) AS average_quiz_score
FROM course c
LEFT JOIN enrolledcourses e
    ON c.courseid = e.courseid
LEFT JOIN quiz q
    ON e.learnerid = q.learnerid
WHERE c.instructorid = :instructor_id
    AND c.is_published = TRUE
GROUP BY c.courseid, c.course_name
ORDER BY enrollment_count DESC
LIMIT 10;
```

---

## Testing

### Running Tests

```bash
# Backend tests
cd learner
pytest

# With coverage
pytest --cov=. --cov-report=html

# Frontend tests
cd ui
pnpm test

# E2E tests
pnpm test:e2e
```

### Writing Tests

**Backend (pytest):**
```python
import pytest
from fastapi.testclient import TestClient

from .main import app
from .database import Base, engine, get_db

client = TestClient(app)


@pytest.fixture
def test_db():
    """Create test database"""
    Base.metadata.create_all(bind=engine)
    yield
    Base.metadata.drop_all(bind=engine)


def test_create_course(test_db):
    """Test course creation"""
    # Arrange
    course_data = {
        "course_name": "Test Course",
        "coursedescription": "Test description",
        "targetaudience": "Students",
        "prereqs": "None",
    }

    # Act
    response = client.post("/api/v1/instructor/courses", json=course_data)

    # Assert
    assert response.status_code == 201
    data = response.json()
    assert data["course_name"] == course_data["course_name"]
    assert "courseid" in data


def test_create_course_invalid_data(test_db):
    """Test course creation with invalid data"""
    course_data = {"course_name": ""}  # Missing required fields

    response = client.post("/api/v1/instructor/courses", json=course_data)

    assert response.status_code == 422  # Validation error
```

**Frontend (Jest + React Testing Library):**
```typescript
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { CourseList } from './course-list';

// Mock API
jest.mock('@/lib/api', () => ({
  fetchCourses: jest.fn(),
}));

describe('CourseList', () => {
  it('renders courses successfully', async () => {
    // Arrange
    const mockCourses = [
      { id: '1', name: 'Course 1', description: 'Desc 1' },
      { id: '2', name: 'Course 2', description: 'Desc 2' },
    ];
    (fetchCourses as jest.Mock).mockResolvedValue(mockCourses);

    // Act
    render(<CourseList />);

    // Assert
    await waitFor(() => {
      expect(screen.getByText('Course 1')).toBeInTheDocument();
      expect(screen.getByText('Course 2')).toBeInTheDocument();
    });
  });

  it('handles errors gracefully', async () => {
    // Arrange
    (fetchCourses as jest.Mock).mockRejectedValue(new Error('Network error'));

    // Act
    render(<CourseList />);

    // Assert
    await waitFor(() => {
      expect(screen.getByText(/failed to load/i)).toBeInTheDocument();
    });
  });
});
```

### Test Coverage

**Minimum requirements:**
- ✅ All new features must have tests
- ✅ Bug fixes must include regression tests
- ✅ Maintain >80% code coverage

---

## Submitting Changes

### Creating a Pull Request

1. **Push your branch** to your fork
   ```bash
   git push origin feature/your-feature-name
   ```

2. **Go to GitHub** and create pull request

3. **Fill in PR template:**
   - **Description**: What does this PR do?
   - **Motivation**: Why is this change needed?
   - **Testing**: How was it tested?
   - **Screenshots**: For UI changes
   - **Checklist**: Complete all items

**Example PR:**
```markdown
## Description
Adds support for true/false quiz questions in addition to multiple choice.

## Motivation
Instructors requested simpler question types for quick comprehension checks.
True/false questions are easier to answer and faster to generate.

## Changes
- Added `TrueFalse` question type to database schema
- Updated quiz generation logic in SME service
- Created UI components for T/F questions
- Added tests for new functionality

## Testing
- [ ] Manual testing with sample course
- [ ] Unit tests added and passing
- [ ] Integration tests passing
- [ ] Tested on multiple browsers

## Screenshots
[Include screenshots of new UI]

## Checklist
- [x] Code follows style guidelines
- [x] Tests added/updated
- [x] Documentation updated
- [x] Commits follow conventional commits
- [x] No breaking changes (or migration guide included)
```

### Review Process

**What happens next:**
1. **Automated checks**: CI/CD runs tests and linting
2. **Code review**: Maintainers review your code
3. **Feedback**: You may be asked to make changes
4. **Approval**: Once approved, PR is merged
5. **Release**: Change included in next release

**Response time:**
- Small fixes: 1-2 days
- Medium features: 3-7 days
- Large features: 1-2 weeks

**Be patient and responsive** — reviewers are volunteers!

### After Merge

**Celebrate!** 🎉 You've contributed to open source!

**Keep contributing:**
- Tackle more issues
- Help review other PRs
- Update documentation
- Support community

---

## Community

### Communication Channels

- **GitHub Issues**: Bug reports, feature requests
- **GitHub Discussions**: Questions, ideas, announcements
- **Discord** (coming soon): Real-time chat
- **Mailing List** (coming soon): Monthly updates

### Getting Recognition

**Contributors are recognized:**
- Listed in `CONTRIBUTORS.md`
- Mentioned in release notes
- Featured in monthly highlights (for significant contributions)

**Ways to stand out:**
- Quality over quantity
- Help others
- Write documentation
- Review PRs
- Long-term involvement

### Becoming a Maintainer

**Interested in deeper involvement?**

**Path to maintainer:**
1. Consistent, quality contributions (6+ months)
2. Active community participation
3. Demonstrate expertise in specific area
4. Invited by existing maintainers

**Maintainer responsibilities:**
- Review and merge PRs
- Triage issues
- Guide contributors
- Shape project direction
- Release management

---

## Questions?

**Still unsure?**
- Ask in [Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)
- Tag `@maintainers` for help
- Email: [contributors@learningmiddleware.org]

**Thank you for contributing!** Every contribution, no matter how small, helps make Learning Middleware better for everyone.

---

*Last updated: November 8, 2025*
