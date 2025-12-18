# Glossary

Definitions of key terms and concepts in Learning Middleware.

---

## A

**Adaptive Learning**  
An educational method where content, pace, and learning paths adjust to individual learner needs and performance.

**API (Application Programming Interface)**  
A set of rules and protocols that allow software applications to communicate with each other.

**Authentication**  
The process of verifying the identity of a user, typically through credentials like email and password.

**Authorization**  
The process of determining what actions an authenticated user is allowed to perform.

---

## B

**Backend**  
The server-side of an application that handles business logic, database operations, and API endpoints. In Learning Middleware: FastAPI services.

**Bloom's Taxonomy**  
A classification of learning objectives into cognitive levels: Remember, Understand, Apply, Analyze, Evaluate, Create.

---

## C

**Cache**  
Temporary storage of data to speed up future requests. Learning Middleware caches generated content for fast retrieval.

**Circuit Breaker**  
A design pattern that prevents cascading failures by stopping requests to a failing service temporarily.

**Content Generation**  
The process of using AI/LLMs to create personalized learning materials based on course content and learner preferences.

**CORS (Cross-Origin Resource Sharing)**  
A security feature that controls which domains can access your API from a web browser.

**Course**  
A structured collection of learning modules on a specific topic, created by an instructor.

---

## D

**Dashboard**  
A user interface showing key information at a glance. Learners see progress; instructors see analytics.

**Database**  
Persistent storage for application data. Learning Middleware uses PostgreSQL (relational) and MongoDB (document-based).

**Detail Level**  
A learner preference indicating how thorough explanations should be: Brief, Moderate, or Detailed.

**Docker**  
A platform for developing, shipping, and running applications in containers (isolated environments).

**Docker Compose**  
A tool for defining and running multi-container Docker applications using a YAML file.

---

## E

**Embedding**  
A vector (array of numbers) representing text in a high-dimensional space, where similar meanings have similar vectors.

**Enrollment**  
The process of a learner joining a course, enabling access to modules and content.

**Explanation Style**  
A learner preference for how concepts should be taught: Examples-Heavy, Conceptual, Practical, or Visual.

---

## F

**FAISS (Facebook AI Similarity Search)**  
A library for efficient similarity search in large collections of vectors, used for RAG in Learning Middleware.

**FastAPI**  
A modern Python web framework for building APIs, used for all backend services in Learning Middleware.

**Frontend**  
The client-side of an application that users interact with. In Learning Middleware: Next.js web application.

---

## G

**Generated Content**  
Module content or quizzes created by AI specifically for a learner based on their preferences.

**GPU (Graphics Processing Unit)**  
Specialized hardware for parallel computation, commonly used for LLM inference.

---

## H

**Hallucination**  
When an AI model generates false or fabricated information not present in its training data. RAG helps prevent this.

**Horizontal Scaling**  
Adding more machines/instances to handle increased load, rather than making existing machines more powerful.

---

## I

**Instructor**  
A user who creates and manages courses, uploads materials, and monitors learner progress.

---

## J

**JWT (JSON Web Token)**  
A compact, URL-safe means of representing claims to be transferred between two parties. Used for authentication in Learning Middleware.

---

## K

**Kubernetes (K8s)**  
An open-source system for automating deployment, scaling, and management of containerized applications.

---

## L

**Language Complexity**  
A learner preference for terminology level: Simple, Technical, or Balanced.

**Learner**  
A user who enrolls in courses and accesses personalized learning content.

**Learning Objective (LO)**  
A statement describing what a learner should be able to do after completing a module, typically starting with an action verb.

**LLM (Large Language Model)**  
An AI model trained on vast amounts of text, capable of understanding and generating human-like text. Examples: GPT-4, Claude, Llama.

**LMS (Learning Management System)**  
Software for administering, delivering, and tracking educational courses. Examples: Moodle, Canvas, Blackboard.

---

## M

**Microservices**  
An architectural style where an application is composed of small, independent services that communicate via APIs.

**Module**  
A unit of learning within a course, covering specific topics and learning objectives.

**MongoDB**  
A NoSQL document database used in Learning Middleware for flexible data storage (preferences, metadata).

---

## N

**Next.js**  
A React framework for building web applications with server-side rendering, used for the Learning Middleware UI.

---

## O

**OpenAPI**  
A specification for defining RESTful APIs, enabling automatic documentation generation (Swagger UI).

**Orchestrator**  
The service that coordinates business logic between UI, learner service, and SME service.

**ORM (Object-Relational Mapping)**  
A technique for converting data between incompatible type systems. Learning Middleware uses SQLAlchemy.

---

## P

**Pagination**  
Dividing large result sets into pages to improve performance and usability.

**Personalization**  
Tailoring content to individual learners based on their preferences, progress, and feedback.

**PostgreSQL**  
An open-source relational database management system used for structured data in Learning Middleware.

**Preference**  
A learner's choice for how content should be generated (Detail Level, Explanation Style, Language).

**Prompt**  
Input text given to an LLM to generate a specific output. Prompt engineering is the art of crafting effective prompts.

**Pydantic**  
A Python library for data validation using Python type annotations, used in FastAPI.

---

## Q

**Quiz**  
An assessment consisting of multiple-choice questions, auto-generated from module content and course materials.

---

## R

**RAG (Retrieval-Augmented Generation)**  
An AI technique that retrieves relevant documents before generating responses, grounding outputs in factual sources.

**REST (Representational State Transfer)**  
An architectural style for APIs using HTTP methods (GET, POST, PUT, DELETE) and stateless communication.

---

## S

**Scaling**  
The ability to handle increased load by adding resources. Can be vertical (bigger machines) or horizontal (more machines).

**Semantic Search**  
Finding information based on meaning rather than exact keyword matching, enabled by vector embeddings.

**SME (Subject Matter Expert) Service**  
The AI/ML service responsible for content generation, RAG, and LLM operations.

**SQLAlchemy**  
A Python SQL toolkit and ORM used for database operations in Learning Middleware.

**Swagger UI**  
An interactive documentation interface for RESTful APIs, auto-generated from OpenAPI specifications.

---

## T

**Token (Authentication)**  
A piece of data used to verify a user's identity, typically a JWT in Learning Middleware.

**Token (LLM)**  
A unit of text processed by an LLM, roughly corresponding to a word or sub-word. LLM pricing and limits are often measured in tokens.

---

## U

**UI (User Interface)**  
The visual elements and controls that users interact with in an application.

---

## V

**Vector**  
An array of numbers representing data (like text) in a multi-dimensional space, used for similarity comparisons.

**Vector Store**  
A database optimized for storing and searching vectors. Learning Middleware uses FAISS.

**vLLM**  
A fast and easy-to-use library for LLM inference, supporting various open-source models.

---

## W

**WebSocket**  
A protocol providing full-duplex communication channels over a single TCP connection, enabling real-time updates.

---

## Acronyms

| Acronym | Full Form |
|---------|-----------|
| API | Application Programming Interface |
| CORS | Cross-Origin Resource Sharing |
| CPU | Central Processing Unit |
| CRUD | Create, Read, Update, Delete |
| CSV | Comma-Separated Values |
| DB | Database |
| FAISS | Facebook AI Similarity Search |
| FAQ | Frequently Asked Questions |
| GPU | Graphics Processing Unit |
| HTTP | Hypertext Transfer Protocol |
| HTTPS | HTTP Secure |
| JWT | JSON Web Token |
| LLM | Large Language Model |
| LMS | Learning Management System |
| LO | Learning Objective |
| ML | Machine Learning |
| NLP | Natural Language Processing |
| OCR | Optical Character Recognition |
| ORM | Object-Relational Mapping |
| PDF | Portable Document Format |
| RAG | Retrieval-Augmented Generation |
| REST | Representational State Transfer |
| SaaS | Software as a Service |
| SME | Subject Matter Expert |
| SQL | Structured Query Language |
| UI | User Interface |
| URL | Uniform Resource Locator |
| UUID | Universally Unique Identifier |

---

## Common Phrases

**"Content is cached"**  
Once generated, personalized content is stored in the database for instant retrieval on subsequent visits.

**"Grounded in course materials"**  
AI responses reference actual uploaded documents rather than general knowledge, thanks to RAG.

**"Per-learner personalization"**  
Each learner receives unique content tailored to their preferences, not generic content for everyone.

**"Vector store creation"**  
The process of converting course PDFs into a searchable index of embeddings for RAG.

**"Zero-shot generation"**  
LLM generating output without task-specific training, relying on general capabilities and retrieved context.

---

## Related Concepts

**Adaptive Testing**  
Assessments that adjust difficulty based on learner performance. (Future enhancement for Learning Middleware)

**Bloom's Taxonomy Levels**
1. **Remember**: Recall facts
2. **Understand**: Explain concepts
3. **Apply**: Use in new situations
4. **Analyze**: Break down and examine
5. **Evaluate**: Make judgments
6. **Create**: Produce original work

**Cognitive Load**  
The mental effort required to learn new information. Good instructional design minimizes extraneous load.

**Formative Assessment**  
Ongoing evaluation during learning (quizzes in Learning Middleware).

**Summative Assessment**  
Final evaluation at the end (course completion).

**Mastery Learning**  
Approach where learners must demonstrate mastery before progressing. Enabled by Learning Middleware's quiz requirements.

**Scaffolding**  
Providing support structures that are gradually removed as learners gain competence. Learning Middleware's adaptive content serves this purpose.

**Zone of Proximal Development (ZPD)**  
The difference between what a learner can do independently and with guidance. Personalization aims to keep content in this zone.

---

## Technical Jargon

**Async/Await**  
Programming pattern for handling asynchronous operations without blocking execution.

**Batch Processing**  
Processing multiple items together rather than one at a time, improving efficiency.

**Connection Pool**  
A cache of database connections maintained for reuse, improving performance.

**Containerization**  
Packaging software with its dependencies into a standardized unit (container) for deployment.

**Dependency Injection**  
A design pattern where dependencies are provided to a component rather than created by it.

**Endpoint**  
A specific URL in an API where requests can be sent.

**Environment Variable**  
Configuration values set outside the application code, often for secrets or deployment-specific settings.

**Idempotent**  
An operation that produces the same result no matter how many times it's executed.

**Middleware** (software concept)  
Software that acts as a bridge between different applications or layers.

**Payload**  
The actual data transmitted in an API request or response, excluding headers and metadata.

**Schema**  
The structure defining how data is organized in a database or API.

**Stateless**  
A design where each request contains all information needed to process it, without relying on server-stored state.

---

*Last updated: November 8, 2025*
