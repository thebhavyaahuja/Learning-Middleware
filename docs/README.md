# Learning Middleware iREL Documentation v1.0

**Welcome to the Learning Middleware project** — an open-source, AI-powered adaptive learning platform that personalizes educational content for every learner.

---

## 📚 Documentation Index

### Getting Started
- **[Quick Start Guide](./getting-started/quick-start.md)** - Get up and running in 10 minutes
- **[Installation Guide](./getting-started/installation.md)** - Detailed setup instructions
- **[System Requirements](./getting-started/requirements.md)** - What you need to run the platform
- **[Configuration Guide](./getting-started/configuration.md)** - Environment setup and customization

### Architecture & Concepts
- **[Architecture Overview](./architecture/overview.md)** - How the system works
- **[Core Concepts](./architecture/concepts.md)** - Understanding personalization, RAG, and more
- **[Service Architecture](./architecture/services.md)** - Microservices breakdown
- **[Data Flow](./architecture/data-flow.md)** - How data moves through the system
- **[Database Schema](./architecture/database.md)** - Data models and relationships

### User Guides
- **[For Learners](./user-guides/learners.md)** - How to use the platform as a student
- **[For Instructors](./user-guides/instructors.md)** - Creating and managing courses
- **[For Administrators](./user-guides/administrators.md)** - System administration

### Developer Documentation
- **[API Reference](./api/README.md)** - Complete API documentation
  - [Learner API](./api/learner-api.md)
  - [Instructor API](./api/instructor-api.md)
  - [Orchestrator API](./api/orchestrator-api.md)
  - [SME API](./api/sme-api.md)
- **[Development Guide](./development/README.md)** - Contributing to the project
- **[Frontend Development](./development/frontend.md)** - UI architecture and components
- **[Backend Development](./development/backend.md)** - Service development
- **[AI/ML Components](./development/ai-ml.md)** - Working with LLMs and RAG

### Deployment
- **[Docker Deployment](./deployment/docker.md)** - Production deployment with Docker
- **[Kubernetes Guide](./deployment/kubernetes.md)** - Scaling with Kubernetes
- **[Cloud Deployment](./deployment/cloud.md)** - AWS, GCP, Azure guides
- **[Monitoring & Logging](./deployment/monitoring.md)** - Observability setup

### Operations
- **[Operations Guide](./operations/README.md)** - Day-to-day operations
- **[Troubleshooting](./operations/troubleshooting.md)** - Common issues and solutions
- **[Performance Tuning](./operations/performance.md)** - Optimization strategies
- **[Backup & Recovery](./operations/backup.md)** - Data protection

### Advanced Topics
- **[Customizing Content Generation](./advanced/content-generation.md)** - Fine-tuning the AI
- **[Vector Store Optimization](./advanced/vector-stores.md)** - RAG performance
- **[Multi-tenancy](./advanced/multi-tenancy.md)** - Supporting multiple organizations
- **[Security Best Practices](./advanced/security.md)** - Hardening your deployment

### Resources
- **[FAQ](./resources/faq.md)** - Frequently asked questions
- **[Glossary](./resources/glossary.md)** - Terms and definitions
- **[Contributing](./resources/contributing.md)** - How to contribute
- **[Changelog](./resources/changelog.md)** - Version history
- **[Roadmap](./resources/roadmap.md)** - Future plans

---

## 🎯 What is Learning Middleware?

Learning Middleware is a complete, production-ready platform for adaptive learning that:

- **Personalizes content** for each learner based on their learning style, pace, and preferences
- **Generates educational content** dynamically using state-of-the-art Large Language Models
- **Creates assessments** automatically from course materials using Retrieval-Augmented Generation
- **Provides AI tutoring** through an intelligent chatbot that references your course materials
- **Tracks progress** with comprehensive analytics for both learners and instructors

Unlike traditional Learning Management Systems (LMS) that serve the same content to everyone, Learning Middleware adapts the content itself — creating unique learning experiences tailored to each individual.

---

## 🚀 Quick Links

- **Live Demo**: [Coming Soon]
- **GitHub Repository**: [github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL)
- **Community Forum**: [Coming Soon]
- **Issue Tracker**: [GitHub Issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues)

---

## 💡 Why Learning Middleware?

### For Learners
- **Personalized Experience**: Content adapts to your learning style — whether you prefer examples, conceptual explanations, or visual aids
- **Pace Control**: Learn at your own speed with content difficulty adjusting to your feedback
- **AI Tutor**: Get instant answers to questions about course materials
- **Clear Progress Tracking**: See exactly where you are and what's next

### For Instructors
- **Scalable Personalization**: Provide individualized learning experiences to hundreds of students simultaneously
- **Automated Content Creation**: Generate learning objectives, module content, and quizzes from your materials
- **Rich Analytics**: Understand how students are learning and where they struggle
- **Time Savings**: Focus on teaching while the platform handles content personalization

### For Institutions
- **Open Source**: No vendor lock-in, full control over your platform
- **Modern Architecture**: Microservices design that scales horizontally
- **Cost Effective**: Run on your infrastructure or any cloud provider
- **Standards Based**: RESTful APIs integrate with existing systems

---

## 🏗️ Architecture at a Glance

```
┌─────────────────────────────────────────────────────────────┐
│                    Frontend (Next.js)                        │
│  Learner Portal  │  Instructor Portal  │  Shared Components │
└──────────────────┴──────────────────────┴───────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────────┐
│   Learner    │  │  Instructor  │  │     Learner      │
│   Service    │  │   Service    │  │  Orchestrator    │
│              │  │              │  │                  │
│ • Auth       │  │ • Courses    │  │ • Business Logic │
│ • Progress   │  │ • Uploads    │  │ • Coordination   │
│ • Enrollment │  │ • Analytics  │  │ • Preferences    │
└──────────────┘  └──────────────┘  └──────────────────┘
        │                   │                   │
        └───────────────────┼───────────────────┘
                            ▼
                    ┌──────────────┐
                    │     SME      │
                    │   (AI/ML)    │
                    │              │
                    │ • LLM Gen    │
                    │ • RAG        │
                    │ • Vector DB  │
                    └──────────────┘
                            │
        ┌───────────────────┴───────────────────┐
        ▼                                       ▼
┌──────────────┐                        ┌──────────────┐
│  PostgreSQL  │                        │   MongoDB    │
│              │                        │              │
│ • Courses    │                        │ • Prefs      │
│ • Users      │                        │ • Files      │
│ • Progress   │                        │ • Content    │
└──────────────┘                        └──────────────┘
```

---

## 🌟 Key Features

### 1. Adaptive Content Generation
Every learner receives unique module content tailored to their:
- **Detail Level**: Brief, moderate, or detailed explanations
- **Explanation Style**: Example-heavy, conceptual, practical, or visual
- **Language Complexity**: Simple, technical, or balanced terminology

### 2. Intelligent Assessment Creation
Quizzes are automatically generated from module content and course materials using RAG, ensuring:
- Questions are grounded in actual course content
- Difficulty matches the learner's level
- Comprehensive coverage of learning objectives

### 3. RAG-Powered Tutoring
An AI chatbot that:
- Answers questions using only course-provided materials
- Cites sources from uploaded documents
- Provides contextual, course-specific guidance

### 4. Comprehensive Analytics
Track learning with:
- Module completion rates
- Quiz performance trends
- Time spent per module
- Learning preference patterns

### 5. Instructor Tools
Empower educators with:
- AI-assisted learning objective generation
- Automated module scaffolding
- Upload and vectorize course materials
- Real-time learner progress monitoring

---

## 🛠️ Technology Stack

| Layer | Technologies |
|-------|-------------|
| **Frontend** | Next.js 14, React 18, TypeScript, Tailwind CSS, shadcn/ui |
| **Backend** | FastAPI, Python 3.10+, SQLAlchemy, Pydantic |
| **Databases** | PostgreSQL 15, MongoDB 7 |
| **AI/ML** | LangChain, FAISS, vLLM, Sentence Transformers |
| **Infrastructure** | Docker, Docker Compose, Kubernetes (optional) |
| **APIs** | RESTful, OpenAPI 3.0 |

---

## 📊 Project Status

**Version**: 1.0.0  
**Status**: Production Ready  
**License**: [Add Your License]  
**Last Updated**: November 2025

### Maturity
- ✅ **Core Features**: Production ready
- ✅ **API Stability**: Stable, versioned APIs
- ✅ **Documentation**: Comprehensive
- ⚠️ **Production Deployments**: Early adopters
- 🔄 **Active Development**: Regular updates

---

## 🤝 Community & Support

### Get Help
- 📖 **Documentation**: You're reading it!
- 💬 **Discussions**: [GitHub Discussions](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/discussions)
- 🐛 **Bug Reports**: [GitHub Issues](https://github.com/InformationRetrievalExtractionLab/Learning-Middleware-iREL/issues)
- 📧 **Email**: [Add contact email]

### Contribute
We welcome contributions! See our [Contributing Guide](./resources/contributing.md) for:
- Code contributions
- Documentation improvements
- Bug reports and feature requests
- Community support

### Acknowledgments
Built by the Information Retrieval & Extraction Lab (iREL) at IIIT Hyderabad.

Special thanks to:
- The LangChain community for RAG tools
- The FastAPI team for the excellent framework
- All our contributors and early adopters

---

## 📝 License

[Add your license information here]

---

## 🎉 Get Started

Ready to build your adaptive learning platform?

**👉 [Start with the Quick Start Guide](./getting-started/quick-start.md)**

Have questions? Check our **[FAQ](./resources/faq.md)** or join the community discussions.

---

*Last updated: November 8, 2025*
