# Quick Start Guide

Get Learning Middleware up and running in under 10 minutes.

---

## Prerequisites

Before you begin, ensure you have:
- **Docker** (20.10+) and **Docker Compose** (2.0+)
- **16GB RAM** minimum (for LLM operations)
- **20GB free disk space**
- An **LLM endpoint** (vLLM, OpenAI, or similar) — [Setup Guide](../advanced/llm-setup.md)

> **Don't have an LLM endpoint?** You can use OpenAI's API or run a local vLLM server. See our [LLM Setup Guide](../advanced/llm-setup.md).

---

## Step 1: Clone the Repository

```bash
git clone https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL.git
cd Learning-Middleware-iREL
```

---

## Step 2: Configure Environment

Create environment files for your LLM endpoints:

```bash
# Create .env file in the root directory
cat > .env << 'EOF'
# Security
SECRET_KEY=change-this-to-a-random-secure-key

# LLM Configuration (Required)
VLLM_4B_URL=http://your-vllm-server:8000/v1
VLLM_4B_MODEL=Qwen/Qwen2.5-4B-Instruct
VLLM_API_KEY=your-api-key-here

VLLM_70B_URL=http://your-vllm-server:8001/v1
VLLM_70B_MODEL=Qwen/Qwen2.5-70B-Instruct
VLLM_70B_API_KEY=your-api-key-here
EOF
```

> **Important**: Replace the placeholder values with your actual LLM endpoint URLs and API keys.

---

## Step 3: Start the Platform

```bash
# Start all services
docker compose up -d

# This will start:
# - PostgreSQL (database)
# - MongoDB (preferences & files)
# - Learner Service (authentication & progress)
# - Instructor Service (course management)
# - Orchestrator Service (business logic)
# - SME Service (AI content generation)
# - UI (web interface)
```

**Check Status:**
```bash
docker compose ps
```

All services should show status `Up (healthy)`:
```
NAME                   STATUS
lmw_postgres           Up (healthy)
lmw_mongo              Up (healthy)
lmw_sme                Up (healthy)
lmw_learner            Up (healthy)
lmw_instructor         Up (healthy)
lmw_learner_orchestrator Up (healthy)
```

---

## Step 4: Access the Platform

Open your browser and navigate to:

**🎓 Learner Portal**: http://localhost:3000  
**👨‍🏫 Instructor Portal**: http://localhost:3000/instructor

---

## Step 5: Create Your First Course

### As an Instructor:

1. **Sign Up**: Go to http://localhost:3000/instructor/signup
2. **Create Course**: Click "New Course" and fill in:
   - Course Name: "Introduction to Programming"
   - Description: "Learn programming fundamentals"
   - Target Audience: "Beginners"
   - Prerequisites: "None"
3. **Add Modules**: Click "Add Module" for each topic:
   - "Variables and Data Types"
   - "Control Flow"
   - "Functions"
4. **Upload Materials**: Upload your course PDFs, textbooks, or lecture notes
5. **Wait for Processing**: The system will create a vector store (2-5 minutes)
6. **Generate Learning Objectives**: Click "Generate LOs" to auto-create learning objectives
7. **Review & Publish**: Review the generated content and click "Publish Course"

Your course is now live! 🎉

---

## Step 6: Experience Personalized Learning

### As a Learner:

1. **Sign Up**: Go to http://localhost:3000/signup
2. **Browse Courses**: Explore available courses
3. **Enroll**: Click "Enroll" on your course
4. **Set Preferences**: On your first module, choose:
   - **Detail Level**: How thorough should explanations be?
   - **Explanation Style**: Do you prefer examples, concepts, or practical applications?
   - **Language**: Simple, technical, or balanced terminology?
5. **Start Learning**: Click "Start Module" to see AI-generated personalized content
6. **Take Quizzes**: Test your knowledge with automatically generated quizzes
7. **Ask Questions**: Use the AI chat to ask questions about the materials

---

## What Just Happened?

Behind the scenes, the platform:

1. **Processed Your Materials**: Extracted text from PDFs, chunked them, and created embeddings
2. **Built a Vector Store**: Created a FAISS index for fast similarity search
3. **Generated Learning Objectives**: Used AI to extract key learning goals from your materials
4. **Created Personalized Content**: For each learner, generated unique module content based on:
   - Learning objectives
   - User preferences
   - Retrieved context from course materials (RAG)
5. **Generated Quizzes**: Created assessment questions grounded in actual course content

---

## Quick Tips

### For Best Results:
- ✅ Upload comprehensive course materials (textbooks, slides, notes)
- ✅ Review and edit AI-generated learning objectives
- ✅ Encourage learners to provide feedback after modules
- ✅ Monitor the chat for common questions

### Performance:
- ⏱️ Vector store creation: 2-5 minutes per course
- ⏱️ Module content generation: 1-3 minutes per module
- ⏱️ Quiz generation: 2-5 minutes per module
- 💾 Content is cached — subsequent visits are instant

### Troubleshooting:
```bash
# View logs for any service
docker compose logs -f <service-name>

# Restart a specific service
docker compose restart <service-name>

# Check service health
curl http://localhost:8000/  # SME Service
curl http://localhost:8002/health  # Learner Service
curl http://localhost:8003/health  # Instructor Service
```

---

## Next Steps

Now that you have a working system:

1. **📖 Read the User Guides**:
   - [Learner Guide](../user-guides/learners.md) — Get the most out of the platform
   - [Instructor Guide](../user-guides/instructors.md) — Create effective courses

2. **🔧 Customize Your Setup**:
   - [Configuration Guide](./configuration.md) — Fine-tune settings
   - [Content Generation Guide](../advanced/content-generation.md) — Customize AI behavior

3. **🚀 Deploy to Production**:
   - [Docker Deployment](../deployment/docker.md) — Production-ready setup
   - [Monitoring & Logging](../deployment/monitoring.md) — Observability

4. **💻 Explore the APIs**:
   - [API Documentation](../api/README.md) — Build integrations

---

## Common Issues

### Services Won't Start
```bash
# Check ports aren't already in use
lsof -i :3000,5432,27017,8000-8003

# Rebuild from scratch
docker compose down -v
docker compose build --no-cache
docker compose up -d
```

### "No vector store found for course"
```bash
# The vector store might still be creating
# Check status via the instructor dashboard or:
curl http://localhost:8003/api/v1/instructor/courses/YOUR_COURSE_ID/vector-store-status \
  -H "Authorization: Bearer YOUR_TOKEN"

# If failed, manually trigger creation:
curl -X POST http://localhost:8003/api/v1/instructor/courses/YOUR_COURSE_ID/create-vector-store \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Content Generation Timeout
```bash
# Check SME service is running and connected to LLM
docker compose logs sme | tail -50

# Test LLM connectivity
docker compose exec sme curl -s $VLLM_70B_URL/v1/models
```

---

## Getting Help

- 📖 **Full Documentation**: [docs/README.md](../README.md)
- 🐛 **Issues**: [GitHub Issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)
- 📧 **Email**: [Add support email]

---

**🎉 Congratulations!** You now have a fully functional adaptive learning platform.

Ready to dive deeper? Check out the [Architecture Overview](../architecture/overview.md) to understand how everything works together.
