# Frequently Asked Questions (FAQ)

Common questions about Learning Middleware.

---

## General Questions

### What is Learning Middleware?

Learning Middleware is an open-source, AI-powered adaptive learning platform that personalizes educational content for each learner. Unlike traditional Learning Management Systems (LMS) that serve identical content to everyone, our platform dynamically generates unique module content, quizzes, and provides AI tutoring based on individual learning preferences.

### How is this different from Moodle, Canvas, or Blackboard?

**Traditional LMS:**
- Instructors create content once
- All students see identical material
- Static quizzes and assessments
- Limited personalization

**Learning Middleware:**
- AI generates unique content per learner
- Content adapts to learning style, pace, and preferences
- Auto-generated quizzes from materials
- AI tutor powered by course materials
- Scales personalization automatically

### Is it really free and open source?

Yes! Learning Middleware is open source under [your license]. You can:
- ✅ Use it for free
- ✅ Modify the code
- ✅ Deploy on your infrastructure
- ✅ Contribute improvements
- ❌ No vendor lock-in

### Who is using Learning Middleware?

Currently in **v1.0**, early adopters include:
- Universities and colleges
- Online course creators
- Corporate training programs
- Research institutions

[Case studies coming soon]

---

## Getting Started

### What do I need to run Learning Middleware?

**Minimum Requirements:**
- **Hardware**: 16GB RAM, 4 CPU cores, 100GB storage
- **Software**: Docker, Docker Compose
- **LLM Access**: vLLM endpoint, OpenAI API, or similar

**Recommended:**
- **Hardware**: 32GB RAM, 8 CPU cores, 500GB SSD
- **LLM**: Self-hosted vLLM for full control

See [System Requirements](../getting-started/requirements.md) for details.

### Can I use OpenAI's API instead of hosting my own LLM?

Yes! Configure the SME service to use OpenAI:

```bash
VLLM_4B_URL=https://api.openai.com/v1
VLLM_4B_MODEL=gpt-3.5-turbo
VLLM_API_KEY=your-openai-api-key

VLLM_70B_URL=https://api.openai.com/v1
VLLM_70B_MODEL=gpt-4
VLLM_70B_API_KEY=your-openai-api-key
```

**Note**: Consider costs — generating content for 100 students in a 10-module course = significant API usage.

### How long does setup take?

**With Docker Compose:**
- Download/install Docker: 15 minutes
- Clone repo and start services: 5 minutes
- Create first course and upload materials: 10 minutes
- Test content generation: 5 minutes

**Total**: ~35 minutes from zero to first personalized module!

### Is there a hosted/SaaS version?

Not currently. Learning Middleware is self-hosted. However:
- We're exploring managed hosting options
- Join our mailing list for updates
- Cloud deployment guides available

---

## For Learners

### Why do I need to set preferences?

Your preferences tell the AI **how you learn best**. This determines:
- Level of detail in explanations
- Whether you get examples, concepts, or practical applications
- How much technical terminology is used

**Result**: Content that matches your learning style, making learning more effective and enjoyable.

### Can I change my preferences later?

Yes! You can update preferences anytime in your profile. Changes affect:
- ✅ Future modules you haven't started
- ❌ Modules you've already completed (keeps original content for consistency)

To regenerate content for completed modules, contact your instructor to reset your progress.

### Why are quizzes different from my classmate's?

Quizzes are **auto-generated** from module content and course materials. While they test the same learning objectives, the specific questions may vary. This ensures:
- Everyone has a fair but unique assessment
- Reduced opportunity for sharing answers
- Questions tailored to the content you saw

### Can I retake quizzes?

Yes, unlimited times! Your highest score is recorded. We encourage:
1. Take the quiz
2. Review explanations for wrong answers
3. Re-study relevant sections
4. Retake to improve your score

### Will the AI tutor give me quiz answers?

No. The AI tutor is designed to help you **learn**, not to give direct answers to assessments. It will:
- ✅ Explain concepts
- ✅ Provide examples
- ✅ Answer questions about course materials
- ❌ Give quiz answers
- ❌ Do your homework

### How is my data used?

**We collect:**
- Account info (name, email, password hash)
- Learning preferences
- Progress data (modules completed, quiz scores)
- Interaction logs (for improving the system)

**We DON'T:**
- Sell your data
- Share with third parties (except as required by law)
- Use for purposes beyond the platform

See our [Privacy Policy](../legal/privacy.md) for details.

---

## For Instructors

### Do I need to write module content?

No! You only need to:
1. **Upload course materials** (textbooks, slides, notes)
2. **Define learning objectives** (or let AI generate them)
3. **Publish the course**

The AI generates unique module content for each learner based on your materials and their preferences.

### How accurate is the AI-generated content?

**Content is grounded in your materials** using RAG (Retrieval-Augmented Generation):
- AI retrieves relevant passages from your PDFs
- Generates content referencing these passages
- Cannot "hallucinate" information not in your materials

**Quality depends on material quality:**
- ✅ Comprehensive textbooks → Excellent content
- ✅ Detailed lecture notes → Great content
- ⚠️ Only a syllabus → Limited content

### Can I review content before students see it?

**Yes and no:**
- You can test-generate content by creating a learner account
- Each learner's content is unique, so you can't approve every version
- **Best practice**: Upload high-quality materials, test a few samples, trust the AI

### What if the AI generates incorrect content?

**Unlikely but possible**. If this happens:
1. Review your uploaded materials (are they comprehensive?)
2. Check the learning objectives (are they clear?)
3. Test with different preferences
4. Report issues (we continuously improve prompts)

**Mitigation**: High-quality, comprehensive course materials = accurate content.

### How do I handle different learning paces?

The platform automatically handles this:
- **Self-paced**: Learners progress at their own speed
- **No deadlines** (unless you set them externally)
- **Progress tracking**: See who's ahead, behind, or stuck
- **Intervention**: Reach out to struggling learners

### Can I use this for graded courses?

Yes! You can:
- Export quiz scores
- Set passing thresholds
- Track completion rates
- Generate grade reports

**Note**: Current system tracks scores but doesn't assign letter grades. Export data to your gradebook system.

### How much does it cost to run?

**Infrastructure costs depend on scale:**

**Small course** (50 students, 10 modules):
- Cloud hosting: ~$50-100/month
- LLM API (if using OpenAI): ~$100-200/month

**Large course** (500 students, 10 modules):
- Cloud hosting: ~$200-500/month
- Self-hosted LLM (recommended): One-time GPU cost
- LLM API (if using OpenAI): ~$1000+/month

**Cost optimization**: Self-host LLMs to avoid per-request API costs.

---

## Technical Questions

### What LLMs are supported?

**Currently supported:**
- vLLM (any model compatible with OpenAI API)
- OpenAI GPT-3.5, GPT-4
- Anthropic Claude (with adapter)
- Any OpenAI API-compatible endpoint

**Recommended models:**
- Small tasks (LOs): Qwen2.5-7B, Mistral-7B, Llama-3-8B
- Complex tasks (content): Qwen2.5-72B, Llama-3-70B, GPT-4

### Can I use different LLMs for different tasks?

Yes! Configure separate endpoints:

```bash
# Fast model for simple tasks
VLLM_4B_URL=http://fast-server:8000/v1
VLLM_4B_MODEL=Qwen2.5-7B

# Powerful model for complex generation
VLLM_70B_URL=http://powerful-server:8001/v1
VLLM_70B_MODEL=Qwen2.5-72B
```

### What databases do you use and why?

**PostgreSQL** (relational data):
- ✅ Courses, modules, users, enrollments
- ✅ Strong relationships (foreign keys)
- ✅ ACID transactions
- ✅ Complex queries (JOINs, aggregations)

**MongoDB** (flexible data):
- ✅ Learning preferences (may evolve)
- ✅ File metadata (nested structure)
- ✅ Generated content (large text)
- ✅ Fast writes, schema flexibility

**FAISS** (vector search):
- ✅ Fast similarity search
- ✅ RAG retrieval
- ✅ Handles millions of vectors
- ✅ Local (no external service)

### How does RAG work?

**RAG (Retrieval-Augmented Generation)** grounds AI in your materials:

```
1. Upload PDFs → Extract text → Chunk into passages
2. Generate embeddings → Store in vector database (FAISS)
3. User query → Embed query → Search for similar passages
4. Retrieve top-k passages → Pass to LLM with query
5. LLM generates answer using retrieved context
```

**Why RAG?**
- ✅ Accurate: Grounded in real materials
- ✅ Trustworthy: Can cite sources
- ✅ Up-to-date: Uses your latest materials
- ✅ No hallucinations: Only uses provided info

### Can I scale Learning Middleware?

Yes! The microservices architecture scales horizontally:

**Scaling strategies:**
```
Small deployment:  All services on one server
Medium deployment: Services on separate servers
Large deployment:  Multiple instances per service + load balancer

Kubernetes support coming in v1.2
```

**Bottlenecks to scale:**
1. **LLM inference**: Add more vLLM workers
2. **Database**: Use read replicas, connection pooling
3. **Vector search**: Distribute FAISS indexes

See [Scaling Guide](../deployment/scaling.md) for details.

### What about data privacy and security?

**Security measures:**
- 🔒 **Password hashing**: bcrypt
- 🔒 **JWT authentication**: HS256 (configurable to RS256)
- 🔒 **HTTPS**: Recommended in production
- 🔒 **Input validation**: Pydantic models
- 🔒 **SQL injection**: Prevented by SQLAlchemy ORM
- 🔒 **CORS**: Configured, restrictable to your domain

**Data storage:**
- Database: Your control (self-hosted or managed)
- Course materials: Local filesystem or S3
- LLM: Self-hosted or API (check provider's terms)

**Compliance:**
- GDPR: Export/delete user data capabilities
- FERPA: Secure student data handling
- Audit logs: Available (enable in config)

---

## Content Generation

### How long does content generation take?

**Typical times:**
- Learning objectives: 30-60 seconds per module
- Module content: 1-3 minutes per module
- Quiz generation: 2-5 minutes per module
- Vector store creation: 5-20 minutes per course

**Factors affecting speed:**
- LLM inference speed (model size, GPU)
- Content length (preferences)
- Network latency (if using remote LLM)
- Number of parallel requests

### Why does the first module take so long?

The first time a learner opens a module:
1. Retrieve their preferences
2. Query vector store (find relevant material)
3. Generate content with LLM
4. Format as Markdown
5. Save to database

**Subsequent visits**: Content loads from cache instantly (<100ms).

### Can I speed up generation?

**Yes:**
1. **Use faster LLMs**: 7B models vs 70B models
2. **Reduce content length**: Choose "brief" default preference
3. **Optimize prompts**: Shorter prompts = faster generation
4. **Parallel processing**: Enable in quiz generation
5. **Better hardware**: GPUs for LLM inference

**Tradeoff**: Speed vs quality. We recommend 70B models for best content.

### What if generation fails?

**Reasons for failure:**
1. LLM timeout (content too long)
2. LLM service down
3. Vector store not ready
4. Network issues

**Solutions:**
- Check SME service logs: `docker logs lmw_sme`
- Verify LLM endpoint: `curl $VLLM_70B_URL/v1/models`
- Retry generation
- Reduce batch size (for quizzes)

### Can I customize the AI's behavior?

**Yes, several ways:**

**1. Prompt Engineering** (advanced):
```python
# Edit prompts in sme/module_gen/main.py
PROMPT_TEMPLATE = """
You are an expert educator...
[Customize instructions here]
"""
```

**2. Model Parameters**:
```yaml
# sme/conf/config.yaml
module_gen:
  temperature: 0.7  # Higher = more creative
  top_p: 0.9
  max_tokens: 4000
```

**3. Default Preferences**:
```python
# Set system-wide defaults
DEFAULT_PREFERENCES = {
    "detail_level": "moderate",
    "explanation_style": "examples-heavy",
    "language": "simple"
}
```

---

## Troubleshooting

### Services won't start

**Check:**
```bash
# Are ports available?
lsof -i :3000,5432,27017,8000-8003

# Are Docker and Docker Compose installed?
docker --version
docker compose version

# Check logs
docker compose logs
```

**Common fixes:**
```bash
# Kill processes using ports
sudo lsof -ti:3000 | xargs kill -9

# Restart Docker
sudo systemctl restart docker

# Rebuild services
docker compose down
docker compose build --no-cache
docker compose up -d
```

### "No vector store found for course"

**Cause**: Vector store hasn't been created yet.

**Solution:**
```bash
# Check status
curl "http://localhost:8003/api/v1/instructor/courses/COURSE_ID/vector-store-status" \
  -H "Authorization: Bearer TOKEN"

# If status = "failed", check logs
docker logs lmw_sme

# Manually trigger creation
curl -X POST "http://localhost:8003/api/v1/instructor/courses/COURSE_ID/create-vector-store" \
  -H "Authorization: Bearer TOKEN"
```

### Content generation times out

**Cause**: LLM taking too long, network issues, or batch size too large.

**Solutions:**
```bash
# 1. Check LLM is responsive
curl $VLLM_70B_URL/health

# 2. Reduce quiz batch size
{
  "batch_size": 2,
  "questions_per_batch": 2
}

# 3. Increase timeout (if needed)
# In orchestrator config:
SME_TIMEOUT = 5000  # 5000 seconds
```

### Database connection errors

**Check database health:**
```bash
# PostgreSQL
docker exec lmw_postgres pg_isready

# MongoDB
docker exec lmw_mongo mongosh --eval "db.stats()"
```

**Restart databases:**
```bash
docker compose restart postgres mongodb

# Wait for services to reconnect
docker compose restart learner instructor orchestrator
```

### UI not loading

**Check frontend:**
```bash
# View logs
docker logs lmw_ui

# Check environment variables
cat ui/.env.local
```

**Check backend connectivity:**
```bash
# Test from browser console
fetch('http://localhost:8002/api/v1/learner/courses')
  .then(r => r.json())
  .then(console.log)
```

---

## Contributions & Community

### How can I contribute?

We welcome all contributions!

**Ways to contribute:**
- 🐛 Report bugs
- 💡 Suggest features
- 📖 Improve documentation
- 💻 Submit code
- 🧪 Write tests
- 🎨 Design UI/UX
- 🌍 Translate to other languages

See [Contributing Guide](../resources/contributing.md) for details.

### Where can I get help?

**Resources:**
- 📖 **Documentation**: You're reading it!
- 💬 **Discussions**: [GitHub Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)
- 🐛 **Issues**: [GitHub Issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues)
- 💬 **Chat**: [Discord/Slack] (Coming soon)
- 📧 **Email**: [Support email]

### Is there a roadmap?

Yes! See our [Roadmap](../resources/roadmap.md) for upcoming features:

**v1.1** (Q1 2026):
- Improved LO generation
- More content formats (video, interactive)
- Better analytics

**v1.2** (Q2 2026):
- Kubernetes deployment
- Advanced learner analytics
- Content versioning

**v2.0** (2026):
- Multi-modal learning (images, video, audio)
- Collaborative learning features
- Advanced adaptive algorithms

### Can I hire someone to help with deployment?

While we don't provide official consulting:
- Community members may offer services
- Check our discussions for recommendations
- Post in "Help Wanted" section

---

## Comparison with Other Platforms

### vs. Traditional LMS (Moodle, Canvas)

| Feature | Traditional LMS | Learning Middleware |
|---------|----------------|---------------------|
| Content Creation | Manual by instructor | AI-generated per learner |
| Personalization | Minimal (course paths) | Deep (content itself adapts) |
| Quiz Generation | Manual | Automatic from materials |
| Scalability | Limited by instructor time | Scales with AI |
| Setup Time | Days to weeks | Hours |
| Cost | Software + instructor time | Infrastructure + LLM |

**When to use traditional LMS:**
- Need extensive non-course features (forums, calendars)
- Require certification/compliance integrations
- Prefer 100% manual content control

**When to use Learning Middleware:**
- Want personalized learning at scale
- Have limited instructor time
- Value adaptive content
- Tech-savvy organization

### vs. Adaptive Platforms (Knewton, DreamBox)

| Feature | Commercial Adaptive | Learning Middleware |
|---------|---------------------|---------------------|
| Open Source | No | Yes |
| Self-Hosted | No | Yes |
| Cost | Subscription per user | Infrastructure only |
| Customization | Limited | Full control |
| LLM Choice | Proprietary | Any model |
| Data Ownership | Vendor | You |

**When to use commercial:**
- Want fully managed solution
- Need extensive support
- Prefer SaaS model

**When to use Learning Middleware:**
- Want data control
- Need customization
- Prefer open source
- Have technical capacity

### vs. AI Tutors (Khanmigo, ChatGPT)

| Feature | AI Tutors | Learning Middleware |
|---------|-----------|---------------------|
| Structured Courses | Limited | Full LMS capabilities |
| Progress Tracking | Basic | Comprehensive |
| Custom Content | Generic | Your materials |
| Grading | No | Yes |
| Multi-User | Limited | Unlimited |

**Use both!** Learning Middleware provides the platform, AI tutor provides assistance.

---

## Future Plans

### What's coming next?

See our [Roadmap](../resources/roadmap.md) for details. Highlights:

**Short term:**
- Improved content generation prompts
- More quiz question types
- Better analytics dashboard

**Medium term:**
- Mobile apps (iOS, Android)
- Video content integration
- Collaborative features (study groups)

**Long term:**
- Multi-modal learning (audio, video, interactive)
- Advanced NLP (auto-grading essays)
- Federated learning (privacy-preserving)

### Can I request features?

Absolutely! 
- **GitHub Discussions**: Propose and discuss
- **GitHub Issues**: Submit formal requests
- **Vote**: 👍 on existing requests

**Most-requested features get priority.**

---

## Still have questions?

- 📖 Check the full [documentation](../README.md)
- 💬 Ask in [GitHub Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)
- 📧 Email us at [support email]

---

*Last updated: November 8, 2025*
