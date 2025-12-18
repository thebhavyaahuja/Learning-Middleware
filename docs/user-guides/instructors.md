# Instructor Guide

Complete guide for creating and managing courses on Learning Middleware.

---

## Table of Contents
- [Getting Started](#getting-started)
- [Creating Your First Course](#creating-your-first-course)
- [Uploading Course Materials](#uploading-course-materials)
- [Understanding Vector Stores](#understanding-vector-stores)
- [Generating Learning Objectives](#generating-learning-objectives)
- [Managing Modules](#managing-modules)
- [Publishing Your Course](#publishing-your-course)
- [Monitoring Learner Progress](#monitoring-learner-progress)
- [Best Practices](#best-practices)

---

## Getting Started

### Create Your Instructor Account

1. Navigate to **http://localhost:3000/instructor/signup**
2. Fill in your information:
   - **Name**: Your full name
   - **Email**: Professional email address
   - **Password**: Secure password (8+ characters)
   - **Department**: Your academic department
   - **Bio**: Brief professional background
3. Click **"Create Account"**

### Instructor Dashboard

After logging in, you'll see:
- **My Courses**: Courses you've created
- **Create New Course**: Start a new course
- **Analytics**: Student performance data
- **Settings**: Profile and preferences

---

## Creating Your First Course

### Step-by-Step Course Creation

#### 1. Basic Information

Click **"Create New Course"** and fill in:

```
Course Name *
├─ Example: "Data Structures and Algorithms"
├─ Keep it clear and descriptive
└─ Students search by course name

Course Description *
├─ What will students learn?
├─ Why is this course valuable?
├─ 2-3 paragraphs recommended
└─ Be specific about outcomes

Target Audience *
├─ Who is this course for?
├─ Example: "Undergraduate CS students"
├─ Example: "Working professionals"
└─ Helps students self-select

Prerequisites
├─ What should students know first?
├─ Example: "Basic programming in Python"
├─ Example: "Linear algebra and calculus"
└─ Leave blank if no prerequisites needed
```

**Example:**
```
Course Name: Introduction to Machine Learning
Description: Learn the fundamentals of machine learning, including 
supervised and unsupervised learning, model evaluation, and practical 
applications. You'll implement algorithms from scratch and use industry-
standard libraries.

Target Audience: Students with programming experience (Python) and 
basic understanding of linear algebra and statistics.

Prerequisites: Python programming, Linear algebra, Basic statistics
```

#### 2. Add Modules

Modules are the main learning units. Plan your course structure:

```
Module Planning Strategy:
┌────────────────────────────────────────┐
│ Start Simple → Build Complexity        │
├────────────────────────────────────────┤
│ Module 1: Foundations                  │
│ Module 2: Core Concepts                │
│ Module 3: Intermediate Topics          │
│ Module 4: Advanced Applications        │
│ Module 5: Project/Capstone             │
└────────────────────────────────────────┘
```

For each module:
- **Title**: Clear, descriptive name
- **Description**: What's covered in this module
- **Order**: Sequence matters!

**Example Modules:**
```
Module 1: Introduction to ML
└─ "Overview of machine learning, types of learning, and applications"

Module 2: Linear Regression
└─ "Simple and multiple linear regression, gradient descent"

Module 3: Classification
└─ "Logistic regression, decision trees, evaluation metrics"

Module 4: Neural Networks
└─ "Perceptrons, backpropagation, deep learning basics"

Module 5: Practical ML
└─ "Data preprocessing, feature engineering, real-world project"
```

**Tip**: 5-10 modules is ideal for most courses. Too few = shallow, too many = overwhelming.

#### 3. Save Draft

Click **"Save as Draft"** — you can always come back to edit before publishing.

---

## Uploading Course Materials

### Why Upload Materials?

Your uploaded PDFs, textbooks, and lecture notes serve as the **knowledge base** for:
- ✅ Generating learning objectives
- ✅ Creating personalized module content
- ✅ Generating quiz questions
- ✅ Powering the AI tutor

**Quality materials = Quality AI-generated content**

### What to Upload

**Recommended:**
- 📚 **Textbooks**: Main course textbook (PDF)
- 📊 **Lecture Slides**: Your presentation slides
- 📝 **Lecture Notes**: Written explanations
- 📄 **Papers**: Relevant research papers
- 📖 **Supplementary Readings**: Additional resources

**File Format:**
- ✅ PDF (preferred)
- ❌ DOCX, PPTX (not yet supported)
- ❌ Images of text (OCR not implemented)

**File Size:**
- Individual file: Up to 50MB (configurable)
- Total per course: Up to 500MB (configurable)

### Upload Process

#### Method 1: Web Interface

1. Open your course in the instructor dashboard
2. Click **"Upload Materials"**
3. **Drag and drop** files or **click to browse**
4. **Multiple files at once** are supported
5. Wait for upload to complete
6. **Vector store creation** starts automatically

#### Method 2: API Upload

```bash
# Upload multiple files at once
curl -X POST "http://localhost:8003/api/v1/instructor/courses/COURSE_ID/upload-to-sme" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -F "files=@textbook.pdf" \
  -F "files=@slides.pdf" \
  -F "files=@notes.pdf"
```

### File Organization

Files are stored:
```
uploads/courses/
└── COURSE_ID/
    ├── textbook.pdf
    ├── lecture_slides.pdf
    ├── notes.pdf
    └── supplementary_reading.pdf
```

### Managing Uploads

**View Uploaded Files:**
- See list in course details
- Check file size, upload date
- Delete if needed

**Add More Files:**
- Upload anytime
- Vector store updates automatically
- Existing content unaffected

**Update Files:**
- Delete old version
- Upload new version
- Regenerate vector store

---

## Understanding Vector Stores

### What is a Vector Store?

A vector store is a **searchable index** of your course materials:

```
Your PDFs → Text Extraction → Chunking → Embeddings → FAISS Index
                                                           ↓
                                                    Fast Semantic Search
```

**Why it matters:**
- Enables AI to **find relevant information** quickly
- Powers **RAG** (Retrieval-Augmented Generation)
- Makes AI responses **accurate and grounded**

### Vector Store Creation

**When Created:**
- Automatically after uploading files
- Can be triggered manually
- Recreated when adding new files

**Process:**
```
┌─────────────────────────────────────────┐
│  Vector Store Creation Pipeline         │
└─────────────────────────────────────────┘
         │
         ├─ 1. Extract text from PDFs (2-5 min)
         │    • Parse all uploaded PDFs
         │    • Handle multi-column layouts
         │    • Preserve structure
         │
         ├─ 2. Chunk text (1 min)
         │    • Split into 512-token chunks
         │    • 50-token overlap (continuity)
         │    • ~300 chunks per 100-page PDF
         │
         ├─ 3. Generate embeddings (3-10 min)
         │    • Convert text to vectors
         │    • 768-dimensional vectors
         │    • ~10 chunks/second
         │
         └─ 4. Build FAISS index (1 min)
              • Organize vectors for fast search
              • IndexFlatL2 (exact search)
              • Save to disk

Total Time: 5-20 minutes depending on material size
```

### Checking Vector Store Status

**In Dashboard:**
- Green checkmark ✅ = Ready
- Yellow spinner 🔄 = Creating
- Red X ❌ = Failed

**Via API:**
```bash
curl "http://localhost:8003/api/v1/instructor/courses/COURSE_ID/vector-store-status" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Response:**
```json
{
  "course_id": "COURSE_123",
  "status": "ready",
  "message": "Vector store created successfully",
  "started_at": "2025-11-08T10:05:00Z",
  "completed_at": "2025-11-08T10:12:34Z"
}
```

**Status Values:**
- `not_started`: No files uploaded yet
- `creating`: In progress (wait)
- `ready`: Good to go! ✅
- `failed`: Check logs, retry

### Troubleshooting Vector Store Issues

**Problem: Creation Failed**
```
Possible causes:
1. PDF is corrupted or password-protected
2. PDF contains only images (needs OCR)
3. Not enough disk space
4. System resource limits

Solution:
- Check file is valid PDF
- Ensure disk space available
- Review logs: docker logs lmw_sme
- Retry: Trigger creation manually
```

**Problem: Creation Stuck**
```
If showing "creating" for > 30 minutes:
1. Check SME service logs
2. Check system resources (CPU, memory)
3. Restart SME service
4. Retry creation
```

**Manual Retry:**
```bash
curl -X POST "http://localhost:8003/api/v1/instructor/courses/COURSE_ID/create-vector-store" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

---

## Generating Learning Objectives

### What are Learning Objectives?

Learning objectives (LOs) define **what students will be able to do** after completing a module.

**Good LO format:**
```
[Action Verb] + [Content] + [Context/Condition]

Examples:
✅ "Implement binary search on a sorted array"
✅ "Explain the difference between classification and regression"
✅ "Analyze algorithm time complexity using Big O notation"
✅ "Apply gradient descent to optimize a cost function"

Bad examples:
❌ "Learn about sorting" (too vague)
❌ "Understand trees" (not measurable)
❌ "Know databases" (not specific)
```

### AI-Generated Learning Objectives

The platform can **automatically generate** LOs from your module names and course materials.

**How it works:**
```
Module Name → RAG Retrieval → LLM Generation → Learning Objectives
     ↓              ↓                ↓                  ↓
"Linear      Find relevant     Analyze content    "1. Implement
Regression"  textbook sections + Generate LOs"       simple linear
                                                      regression
                                                   2. Calculate
                                                      cost function
                                                   3. Apply gradient
                                                      descent..."
```

### Generating LOs

**Prerequisites:**
- ✅ Course materials uploaded
- ✅ Vector store status = `ready`
- ✅ Module names defined

**Steps:**
1. Go to your course
2. Click **"Generate Learning Objectives"**
3. Select modules to generate for
4. Choose number of LOs per module (default: 5-6)
5. Click **"Generate"**
6. Wait 2-3 minutes
7. Review generated LOs

**Via API:**
```bash
curl -X POST "http://localhost:8003/api/v1/instructor/courses/COURSE_ID/generate-los" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "courseid": "COURSE_123",
    "module_names": ["Linear Regression", "Logistic Regression"],
    "n_los": 6
  }'
```

### Reviewing & Editing LOs

**Always review AI-generated LOs!**

Check for:
- ✅ **Specificity**: Are they concrete and measurable?
- ✅ **Relevance**: Do they match course materials?
- ✅ **Difficulty**: Are they appropriate for learners?
- ✅ **Coverage**: Do they cover all key concepts?
- ✅ **Clarity**: Are they understandable?

**Editing:**
1. Click **"Edit Learning Objectives"** on any module
2. Modify, add, or remove LOs
3. Reorder by dragging
4. Save changes

**Example Edit:**
```
Before (Generated):
"Understand linear regression"

After (Edited):
"Implement simple linear regression using gradient descent
and calculate the mean squared error"
```

### Manual Entry

Prefer to write your own? Skip generation and enter manually:
1. Open module
2. Click **"Add Learning Objectives"**
3. Type each objective
4. Save

**Tip**: Mix approaches! Generate LOs as a starting point, then refine them.

---

## Managing Modules

### Module Lifecycle

```
Created → LOs Added → Content Generated (by learners) → Analytics
```

**As instructor, you:**
- ✅ Create module structure
- ✅ Define learning objectives
- ✅ Provide source materials
- ❌ Don't write content (AI does that per learner!)

### Editing Module Details

**What you can edit:**
- Title
- Description
- Learning objectives
- Order/sequence

**What you can't edit:**
- Generated learner content (it's personalized!)
- Quiz questions (auto-generated per learner)

**To edit:**
1. Go to course dashboard
2. Click module to edit
3. Click **"Edit"**
4. Make changes
5. **Save**

**Impact of edits:**
- Title/description: UI updates only
- Learning objectives: New content generated for learners who haven't started yet

### Reordering Modules

**Why reorder?**
- Fix logical progression
- Insert prerequisites earlier
- Move advanced topics later

**How:**
1. Course dashboard → **"Reorder Modules"**
2. Drag and drop modules
3. **Save new order**

**Effect on learners:**
- Enrolled learners see new order
- Completed modules remain completed
- In-progress learners can continue

### Deleting Modules

**Caution**: This affects enrolled learners!

**Before deleting:**
- Check if learners have completed it
- Consider archiving instead

**To delete:**
1. Module details → **"Delete"**
2. Confirm deletion
3. ✅ Module removed

**What happens:**
- Module removed from course
- Learner progress data retained (for records)
- Content no longer accessible

---

## Publishing Your Course

### Draft vs. Published

**Draft Mode:**
- ✅ Visible only to you
- ✅ Edit freely
- ✅ Test content generation
- ❌ Not visible to learners

**Published Mode:**
- ✅ Visible to all learners
- ✅ Appears in course catalog
- ⚠️ Edits affect enrolled learners
- ✅ Analytics available

### Pre-Publishing Checklist

Before publishing, ensure:

```
☐ Course details complete
  ☐ Name, description, audience, prerequisites

☐ All modules created
  ☐ Titles and descriptions
  ☐ Logical order

☐ Course materials uploaded
  ☐ PDFs uploaded
  ☐ Vector store = ready

☐ Learning objectives defined
  ☐ Generated and reviewed
  ☐ Edited where needed
  ☐ 5-6 LOs per module

☐ Test run completed
  ☐ Create test learner account
  ☐ Enroll in course
  ☐ Generate content for 1-2 modules
  ☐ Take quizzes
  ☐ Verify content quality

☐ Ready to publish!
```

### Publishing

1. Course dashboard → **"Publish Course"**
2. Review final checklist
3. Click **"Publish"**
4. ✅ Course is live!

**What happens:**
- Course appears in learner catalog
- Students can enroll immediately
- Content generation begins on first access

### Unpublishing

Need to make major changes?

1. Course settings → **"Unpublish"**
2. Confirm action
3. ✅ Course back to draft

**Effect:**
- Removed from catalog
- Enrolled learners keep access
- No new enrollments allowed

---

## Monitoring Learner Progress

### Course Analytics Dashboard

Access via **"Analytics"** in course menu.

**Metrics Available:**

#### 1. Enrollment Stats
```
Total Enrollments: 47
Active Learners: 42 (89%)
Completed Course: 12 (26%)
Dropped Out: 5 (11%)
```

#### 2. Module Completion Rates
```
Module 1: ▓▓▓▓▓▓▓▓▓░ 90% (42/47)
Module 2: ▓▓▓▓▓▓▓░░░ 70% (33/47)
Module 3: ▓▓▓▓▓░░░░░ 50% (24/47)
Module 4: ▓▓░░░░░░░░ 20% (9/47)
Module 5: ░░░░░░░░░░  5% (2/47)
```

**Insights:**
- Drop-off between modules? → Content too hard?
- Low completion rates? → Module too long?

#### 3. Quiz Performance
```
Module  Avg Score  Pass Rate  Retakes
───────────────────────────────────────
Mod 1   85%       95%        1.2
Mod 2   78%       88%        1.8
Mod 3   72%       80%        2.4  ← Challenging
Mod 4   81%       92%        1.5
```

**Insights:**
- Low scores? → Objectives too ambitious?
- High retakes? → Questions unclear?

#### 4. Learner Progress Distribution
```
Progress          Learners
0-20%:            ▓▓░░░ 8
20-40%:           ▓▓▓▓░ 12
40-60%:           ▓▓▓▓▓ 15
60-80%:           ▓▓▓░░ 9
80-100%:          ▓░░░░ 3
```

#### 5. Time Metrics
```
Avg time per module: 2.5 hours
Fastest completion: 8 days
Avg completion: 21 days
Slowest in-progress: 45 days (and counting)
```

#### 6. Preference Trends
```
Detail Level:
Brief:     ▓▓░░░ 15%
Moderate:  ▓▓▓▓▓ 60%  ← Most popular
Detailed:  ▓▓░░░ 25%

Explanation Style:
Examples:   ▓▓▓▓░ 45%  ← Most popular
Conceptual: ▓▓░░░ 20%
Practical:  ▓▓▓░░ 30%
Visual:     ▓░░░░ 5%
```

### Individual Learner Progress

Click any learner to see:
- Completed modules
- Quiz scores per module
- Time spent
- Current status
- Learning preferences

**Use cases:**
- Identify struggling learners
- Reach out to offer help
- See who's excelling

### Exporting Data

**CSV Export:**
- All enrollments
- Module completion
- Quiz scores

**Use for:**
- Grade books
- Institutional reporting
- Further analysis (Excel, Python, R)

---

## Best Practices

### Course Design

**1. Start with Learning Outcomes**
```
Don't: "I'll teach sorting algorithms"
Do:   "After this course, students will be able to:
       1. Implement 5 sorting algorithms
       2. Analyze their time/space complexity
       3. Choose the right algorithm for different scenarios"
```

**2. Structure for Progression**
```
Beginner → Intermediate → Advanced → Applied

Module 1: Fundamentals (concepts, basic examples)
Module 2: Core Skills (practice, variations)
Module 3: Advanced Techniques (edge cases, optimization)
Module 4: Real-World Applications (projects, case studies)
```

**3. Chunk Content Appropriately**
```
Too small: 20 modules of 10 minutes each → overwhelming navigation
Too large:  3 modules of 3 hours each → intimidating, no checkpoints
Just right: 6-10 modules of 30-60 minutes each
```

### Material Selection

**Upload Comprehensive Materials:**
- ✅ Primary textbook (covers all topics)
- ✅ Your original lecture slides/notes
- ✅ Supplementary readings
- ❌ Don't: Only upload syllabus
- ❌ Don't: Upload unrelated materials

**Material Quality > Quantity:**
- Better: One excellent 200-page textbook
- Worse: 20 random blog posts and PDFs

**Keep Materials Updated:**
- Review materials annually
- Replace outdated content
- Add new research/findings

### Learning Objectives

**Use Action Verbs (Bloom's Taxonomy):**
```
Remember:    List, Define, Identify, Label
Understand:  Explain, Describe, Summarize, Interpret
Apply:       Implement, Use, Solve, Demonstrate
Analyze:     Compare, Contrast, Differentiate, Examine
Evaluate:    Assess, Justify, Critique, Evaluate
Create:      Design, Develop, Construct, Formulate
```

**Be Specific:**
```
Vague:    "Understand sorting"
Specific: "Implement bubble sort with O(n²) time complexity"

Vague:    "Learn about databases"
Specific: "Design a normalized relational database schema following 3NF"
```

**Make Them Measurable:**
```
Not measurable: "Appreciate the importance of algorithms"
Measurable:     "Calculate and compare the time complexity of three algorithms"
```

### Supporting Learners

**1. Monitor Analytics Weekly**
- Check completion rates
- Identify struggling modules
- Reach out to at-risk learners

**2. Respond to Common Questions**
- If multiple learners ask the same thing → clarify in materials
- Add supplementary resources if needed

**3. Iterate Based on Feedback**
```
Low quiz scores on Module 3?
→ Review LOs: too ambitious?
→ Check materials: sufficient coverage?
→ Add more examples or clarification
```

**4. Keep Materials Fresh**
- Update annually
- Add new examples
- Incorporate latest research

### Technical Tips

**Optimize PDF Uploads:**
```bash
# Reduce PDF size before uploading
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook \
   -dNOPAUSE -dQUIET -dBATCH \
   -sOutputFile=compressed.pdf input.pdf
```

**Batch Operations:**
```bash
# Upload multiple files at once
curl -X POST "http://localhost:8003/api/v1/instructor/courses/$COURSE_ID/upload-to-sme" \
  -H "Authorization: Bearer $TOKEN" \
  -F "files=@file1.pdf" \
  -F "files=@file2.pdf" \
  -F "files=@file3.pdf"
```

**Regular Backups:**
- Export course data monthly
- Download uploaded materials
- Keep offline copies

---

## Frequently Asked Questions

**Q: Can I edit content that's already generated for learners?**  
A: No. Content is personalized per learner. You can edit LOs, which affects future content generation.

**Q: What if I need to significantly change a published course?**  
A: Consider versioning: create a new version, migrate learners, archive old version.

**Q: How do I know if my materials are good enough?**  
A: Test with a sample module. If generated content is relevant and accurate, you're good!

**Q: Can learners see the PDFs I uploaded?**  
A: No. PDFs are used for content generation only. Learners see generated content and chat references.

**Q: What if the vector store keeps failing?**  
A: Check PDF quality (not scanned images), ensure files aren't password-protected, review logs for specific errors.

**Q: How do I handle course updates?**  
A: Upload new materials, regenerate vector store. Enrolled learners continue with current content unless you reset their progress.

**Q: Can I copy modules from another course?**  
A: Not currently. You can reuse materials by uploading the same PDFs to multiple courses.

---

## Getting Help

**Technical Issues:**
- 📖 [Operations Guide](../operations/README.md)
- 🐛 [GitHub Issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues)
- 📧 [Email Support]

**Pedagogical Guidance:**
- 📚 [Course Design Resources]
- 👥 [Instructor Community Forum]

**Feature Requests:**
- 💡 [GitHub Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)

---

**Ready to create your first course?** Start at your [Instructor Dashboard](http://localhost:3000/instructor)!

---

*Last updated: November 8, 2025*
