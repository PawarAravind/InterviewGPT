# InterviewGPT 🎙️

> **AI-powered, personalized voice interview preparation platform**

InterviewGPT is a full-stack AI mock interview platform designed to simulate realistic technical interviews based on a candidate's **resume, target role, and job description (JD)**.

Instead of presenting a fixed list of questions, InterviewGPT analyzes the candidate's background and interview context, conducts an **adaptive voice-based interview**, evaluates spoken answers, and generates personalized feedback highlighting strengths, weaknesses, missing concepts, and areas for improvement.

The system also implements a **multi-provider AI fallback architecture** using **Gemini → Groq → Ollama (Gemma 3:4b)** so that interview generation and evaluation can continue when a cloud provider reaches a quota, rate limit, or availability issue.

---

## 🚀 Project Overview

Traditional mock-interview platforms generally provide predefined questions or generic AI conversations.

InterviewGPT focuses on:

* Resume-aware questioning
* Job-description-aware questioning
* Role-specific interview strategies
* Adaptive follow-up questions
* Voice-based interaction
* Speech-to-text transcription
* AI-based answer evaluation
* Technical and behavioral assessment
* DSA questions for relevant technical roles
* Personalized final feedback
* Interview history and performance tracking
* AI provider fallback and error resilience

### Core workflow

```text
Resume + Job Description + Target Role
                  │
                  ▼
          Resume / JD Analysis
                  │
                  ▼
        Candidate Understanding
                  │
                  ▼
          Interview Strategy
                  │
                  ▼
         AI Interview Question
                  │
                  ▼
             Text-to-Speech
                  │
                  ▼
          Candidate Hears Question
                  │
                  ▼
           Candidate Speaks
                  │
                  ▼
             Audio Recording
                  │
                  ▼
          Audio Normalization
                FFmpeg
                  │
                  ▼
          Speech-to-Text
                  │
                  ▼
          Answer Transcript
                  │
                  ▼
          AI Answer Evaluation
                  │
                  ▼
        Adaptive Next Question
                  │
                  ▼
             Repeat
                  │
                  ▼
          Final Evaluation
                  │
                  ▼
      Personalized Interview Report
```

---

# ✨ Key Features

## 1. Personalized Interview Generation

The interview is generated using:

* Candidate resume
* Target job description
* Target role
* Candidate skills
* Projects
* Previous answers
* Previous questions
* Interview strategy
* Difficulty and question type

This allows the system to ask questions that are relevant to the candidate instead of relying only on generic question banks.

Example:

```text
Resume:
Implemented a multithreaded Sudoku validator

          ↓

AI Interviewer:

"Why did you choose your particular
thread-distribution strategy?"

          ↓

Follow-up:

"What happens if the workload becomes
imbalanced across threads?"
```

This creates a more realistic interview flow.

---

# 🎙️ 2. Voice-to-Voice Interview

The primary interview experience is voice-based.

### AI → Candidate

```text
AI generates question
        ↓
Text-to-Speech
        ↓
Audio
        ↓
Candidate hears question
```

### Candidate → AI

```text
Candidate speaks
        ↓
Browser MediaRecorder
        ↓
Audio Blob
        ↓
Backend
        ↓
FFmpeg
        ↓
Speech-to-Text
        ↓
Transcript
        ↓
AI evaluation
```

The candidate does not need to manually type answers.

The interview therefore behaves more like a real interviewer interaction rather than a traditional chatbot.

---

# 🧠 3. Adaptive Interview Engine

InterviewGPT maintains interview context and uses previous interactions to determine what should be asked next.

The interview engine considers:

```text
Candidate Profile
        +
Job Description
        +
Target Role
        +
Interview Strategy
        +
Previous Questions
        +
Previous Answers
        +
Current Performance
        ↓
Next Question
```

This enables:

* Follow-up questions
* Difficulty adaptation
* Topic exploration
* Resume-based probing
* Technical deep dives
* Behavioral questions
* DSA questions for relevant roles
* Unexpected but relevant questions

---

# 💻 4. Role-Specific Technical Interviews

The interview can adapt to different technical roles.

Examples:

### Software Engineering

* C++
* DSA
* OOP
* Operating Systems
* DBMS
* Computer Networks
* System Design
* Multithreading

### ML / AI Roles

* Machine Learning fundamentals
* Algorithms
* Model evaluation
* Data preprocessing
* Problem solving
* Project-based ML questions

### Project-Based Questions

The interviewer can investigate technologies and projects mentioned in the candidate's resume.

---

# 🧩 5. Resume Intelligence

Candidates can provide their resume for interview personalization.

The backend processes the uploaded resume and extracts useful candidate information such as:

* Skills
* Projects
* Education
* Experience
* Technologies
* Achievements

The extracted information is then used by the interview engine.

---

# 📄 6. Job Description Intelligence

The job description provides the target context for the interview.

The system uses the JD to understand:

* Required technical skills
* Role expectations
* Relevant technologies
* Technical areas
* Interview priorities

This allows the same resume to produce different interviews for different roles.

For example:

```text
Same Candidate
       │
       ├── SDE JD
       │      ↓
       │   DSA + C++ + OS + System Design
       │
       └── ML JD
              ↓
           ML + Python + Algorithms + Projects
```

---

# 📊 7. AI Answer Evaluation

Each candidate answer is evaluated independently.

The evaluation considers:

* Technical correctness
* Technical depth
* Problem-solving ability
* Communication
* Clarity
* Missing concepts
* Weak reasoning
* Behavioral answer structure
* Areas requiring clarification

The result can include:

```text
Correctness Score
Technical Score
Communication Score
Problem-Solving Score

Strengths
Weaknesses
Missing Points
Feedback
Improvement Areas
```

---

# 📈 8. Final Interview Evaluation

After completing the interview, InterviewGPT aggregates the interview performance.

The final report contains:

* Overall score
* Technical score
* Communication score
* Problem-solving score
* Strengths
* Weaknesses
* Topics to revise
* Role readiness
* Recommended next steps

Example:

```text
Overall Score: 78/100

Technical:
82/100

Communication:
74/100

Problem Solving:
79/100

Strong Areas:
✓ DSA fundamentals
✓ Project explanation

Needs Improvement:
✗ Multithreading
✗ System Design

Recommended Revision:
• Race conditions
• Synchronization
• Distributed caching
```

---

# 🤖 9. Multi-Provider AI Architecture

A major engineering feature of InterviewGPT is the AI provider abstraction and fallback system.

Instead of coupling the application to a single AI provider:

```text
Interview Logic
       │
       ▼
   AI Service
       │
       ▼
Provider Abstraction
```

The application uses:

```text
                AI Request
                    │
                    ▼
                 Gemini
                PRIMARY
                    │
             failure / quota
                    ▼
                  Groq
                SECONDARY
                    │
             failure / limit
                    ▼
                 Ollama
                TERTIARY
                    │
                    ▼
              Gemma 3:4b
              Local Model
```

### Why?

Cloud AI providers can experience:

* Rate limits
* Quota exhaustion
* Request-size limits
* Temporary outages
* Account restrictions

A single-provider architecture would cause the entire interview to fail.

The provider abstraction allows the interview engine to remain independent of the underlying AI provider.

---

# 🦙 10. Local LLM Fallback — Ollama

Ollama provides the final local fallback.

```text
Backend
   │
   ▼
localhost:11434
   │
   ▼
Gemma 3:4b
```

This avoids depending entirely on cloud APIs for text generation.

The local model is used for:

* Interview question generation
* Answer evaluation
* Final evaluation

when the cloud providers are unavailable.

Ollama structured outputs are used to request schema-constrained JSON responses instead of relying only on prompt instructions.

---

# 🧱 11. Backend Architecture

The backend follows a layered architecture:

```text
HTTP Request
     │
     ▼
   Routes
     │
     ▼
 Controllers
     │
     ▼
 Services
     │
     ├── AI
     ├── Resume Processing
     ├── Speech Processing
     └── Business Logic
     │
     ▼
Database / External APIs
```

### Main layers

#### Routes

Responsible for API endpoint definitions.

#### Controllers

Handle:

* Request validation
* Authentication context
* Calling services
* HTTP responses

#### Services

Contain reusable business logic.

Examples:

```text
aiService.js
resumeParser.js
speechToText.js
textToSpeech.js
```

#### Models

Represent persistent MongoDB data using Mongoose.

---

# 🗄️ 12. Database

The application uses:

**MongoDB + Mongoose**

Interview data can contain nested information such as:

```text
Interview
├── Candidate
├── Role
├── Job Description
├── Interview State
├── Questions
│   ├── Question
│   ├── Answer
│   ├── Transcript
│   └── Evaluation
└── Final Evaluation
```

MongoDB's document model fits this type of evolving interview data well.

---

# 🔐 13. Authentication & Security

Authentication uses:

* JWT
* Password hashing
* Protected routes
* Authorization middleware
* Environment variables

Passwords are never stored as plaintext.

Sensitive credentials are stored in `.env` and excluded from version control.

Security-related middleware includes:

* CORS
* Helmet
* Rate limiting
* Request validation
* File upload limits

---

# 🎧 14. Audio Processing

Browser-generated audio can vary in:

* Container format
* Codec
* Sample rate
* Encoding

Therefore the backend uses FFmpeg as an audio normalization layer.

```text
Browser Audio
      │
      ▼
   FFmpeg
      │
      ▼
Normalized Audio
      │
      ▼
Speech-to-Text
```

This makes speech processing more predictable across different browsers and devices.

---

# 🛠️ Technology Stack

## Frontend

* React
* JavaScript
* HTML
* CSS
* React Router
* Browser MediaRecorder API
* REST API integration

## Backend

* Node.js
* Express.js
* REST APIs
* Middleware-based architecture

## Database

* MongoDB
* Mongoose

## AI / ML

* Google Gemini API
* Groq API
* Ollama
* Gemma 3:4b
* Structured JSON / JSON Schema

## Voice

* Speech-to-Text
* Text-to-Speech
* Browser MediaRecorder
* FFmpeg

## Authentication & Security

* JWT
* bcrypt
* Helmet
* CORS
* Express Rate Limit
* Environment variables

---

# 📁 Project Architecture

```text
InterviewGPT/
│
├── client/
│   └── React frontend
│
├── server/
│   │
│   ├── config/
│   │   ├── env.js
│   │   ├── db.js
│   │   ├── gemini.js
│   │   ├── groq.js
│   │   └── ollama.js
│   │
│   ├── controllers/
│   │   ├── authController.js
│   │   └── interviewController.js
│   │
│   ├── middleware/
│   │   ├── authMiddleware.js
│   │   └── errorMiddleware.js
│   │
│   ├── models/
│   │   ├── User.js
│   │   └── Interview.js
│   │
│   ├── routes/
│   │   ├── authRoutes.js
│   │   └── interviewRoutes.js
│   │
│   ├── services/
│   │   ├── aiService.js
│   │   ├── resumeParser.js
│   │   ├── speechToText.js
│   │   └── textToSpeech.js
│   │
│   ├── utils/
│   │   └── apiError.js
│   │
│   └── server.js
│
└── README.md
```

---

# 🔄 Complete End-to-End Data Flow

```text
                    USER
                     │
                     ▼
             React Interview UI
                     │
                     ▼
                REST API
                     │
                     ▼
              Express Backend
                     │
             ┌───────┴────────┐
             │                │
             ▼                ▼
         MongoDB          AI Service
                              │
                     ┌────────┼────────┐
                     │        │        │
                     ▼        ▼        ▼
                  Gemini    Groq     Ollama
                                      │
                                  Gemma 3:4b
                     │
                     ▼
               Interview Question
                     │
                     ▼
                     TTS
                     │
                     ▼
                  Candidate
                     │
                  Speaks
                     │
                     ▼
                MediaRecorder
                     │
                     ▼
                  Backend
                     │
                     ▼
                  FFmpeg
                     │
                     ▼
                   STT
                     │
                     ▼
                Transcript
                     │
                     ▼
                AI Evaluation
                     │
                     ▼
                MongoDB
                     │
                     ▼
              Next Question
                     │
                     ▼
                  Repeat
                     │
                     ▼
             Final Evaluation
                     │
                     ▼
               Results Page
```

---

# 🧠 Important Engineering Decisions

## Provider Abstraction

AI functionality is isolated behind an AI service instead of being tightly coupled to a specific provider.

This makes provider replacement easier.

---

## Fallback Architecture

The application does not depend on one AI provider.

```text
Gemini
  ↓
Groq
  ↓
Ollama
```

This improves resilience against quota and availability problems.

---

## Structured AI Responses

The interview engine requires predictable JSON responses.

Cloud providers use structured response mechanisms where supported.

Ollama uses JSON Schema structured outputs.

This allows application code to consume AI responses programmatically instead of relying on unstructured text.

---

## Separation of Concerns

The project separates:

```text
HTTP handling
Business logic
AI integration
Audio processing
Database access
Authentication
```

This keeps the codebase easier to maintain and extend.

---

# 🐛 Engineering Problems Encountered

The project was developed iteratively and several real integration issues were encountered.

## Gemini quota exhaustion

Gemini returned:

```text
429 RESOURCE_EXHAUSTED
```

Instead of terminating the interview, the system falls back to Groq.

---

## Groq request-size limitation

Groq returned a `413` error because the request exceeded the account's token-per-minute limit.

The system therefore falls through to the local Ollama provider.

This also highlighted the importance of controlling AI prompt/context size.

---

## Provider fallback implementation

An early provider abstraction contained an incorrectly exported fallback helper and circular dependency.

The provider modules were reorganized so that provider-specific utilities remain inside their respective configuration modules.

---

## Cerebras provider access

A Cerebras fallback initially returned:

```text
402 Payment Required
```

Instead of depending on another cloud billing configuration, the final architecture uses Ollama as the tertiary provider.

---

## Ollama JSON generation

The first Ollama implementation relied primarily on prompt instructions to produce JSON.

The model could occasionally return non-JSON text.

The implementation was changed to use Ollama's structured-output / JSON Schema mechanism.

---

## Browser audio compatibility

Browser-recorded audio can vary between environments.

FFmpeg was introduced to normalize audio before speech-to-text processing.

---

# 📌 Current Scope

The implemented project focuses on a **voice-based AI interview experience**.

The current system includes:

* Resume-aware interviews
* JD-aware interviews
* Role-specific questioning
* Adaptive questioning
* Voice interaction
* Speech-to-text
* Text-to-speech
* AI answer evaluation
* Final interview evaluation
* Interview history
* Authentication
* AI provider fallback
* Local LLM fallback
* Audio normalization
* Error handling
* Security middleware

---

# 🚧 Future Improvements

The following features are potential extensions rather than claims about the current implementation.

## Animated AI Interviewer

Replace the current voice interface with an animated interviewer:

```text
AI Avatar
   ↓
Lip Sync
   ↓
Voice
   ↓
Candidate
```

---

## Better Interview State Modeling

Introduce an explicit topic graph:

```text
DSA
 ├── Arrays
 ├── Trees
 ├── Graphs
 └── DP

OS
 ├── Processes
 ├── Threads
 ├── Synchronization
 └── Memory
```

The interviewer could track topic coverage and dynamically target weak areas.

---

## Prompt / Context Optimization

Reduce repeated context sent to cloud models by maintaining:

* Candidate summaries
* JD summaries
* Interview summaries
* Recent-answer windows
* Topic-level state

This would reduce token usage and improve latency.

---

## Advanced Analytics

Potential metrics:

* Average answer time
* Question difficulty
* Topic-wise performance
* Improvement over multiple interviews
* Confidence trends
* Weak-topic frequency

---

# ⚠️ Limitations

This repository intentionally does not contain every development file or production credential.

Some files/configuration required to run the complete application may therefore be omitted from the public repository.

API keys and secrets are intentionally not committed.

The local Ollama fallback also depends on the machine having sufficient resources to run `gemma3:4b`.

AI-generated evaluations should be treated as **practice feedback**, not as an authoritative hiring assessment.

---

# 🔒 Environment Variables

The application uses environment variables for sensitive configuration.

Example:

```env
MONGODB_URI=your_mongodb_connection_string

GEMINI_API_KEY=your_gemini_api_key

GROQ_API_KEY=your_groq_api_key

JWT_SECRET=your_jwt_secret

PORT=5000

CLIENT_URL=http://localhost:5173

OLLAMA_BASE_URL=http://localhost:11434

OLLAMA_MODEL=gemma3:4b

OLLAMA_TIMEOUT_MS=120000
```

Actual credentials must never be committed to Git.

---

# ▶️ Local Development

## Prerequisites

Install:

* Node.js
* npm
* MongoDB / MongoDB Atlas
* Ollama
* Git

For the local AI fallback, install the `gemma3:4b` model through Ollama.

```bash
ollama pull gemma3:4b
```

Verify:

```bash
ollama list
```

---

## Backend

```bash
cd server
npm install
npm run dev
```

---

## Frontend

```bash
cd client
npm install
npm run dev
```

The exact environment configuration depends on the local development setup.

---

# 🧪 AI Fallback Testing

The fallback architecture can be tested by making the primary provider unavailable.

Expected behavior:

```text
Gemini
   │
   └── unavailable
          ↓
        Groq
          │
          └── unavailable
                 ↓
               Ollama
                 │
                 ▼
             Gemma 3:4b
```

The frontend does not need to know which provider generated the response.

This is an important property of the provider abstraction.

---

# 📚 What This Project Demonstrates

This project demonstrates practical experience with:

```text
Full-Stack Development
        +
React
        +
Node.js / Express
        +
REST APIs
        +
MongoDB
        +
Authentication
        +
AI API Integration
        +
LLM Fallback Architecture
        +
Structured AI Outputs
        +
Speech Processing
        +
FFmpeg
        +
Browser Media APIs
        +
Error Handling
        +
Security
        +
System Design
```

---

# 💡 Key Takeaways

The primary engineering lessons from the project were:

1. **AI applications require provider resilience.**
2. **LLM output should be treated as untrusted and validated.**
3. **Structured outputs are preferable to parsing arbitrary model text.**
4. **Prompt size directly affects API reliability and cost.**
5. **Audio pipelines require format normalization.**
6. **Business logic should be separated from provider-specific implementations.**
7. **Cloud and local models can complement each other.**
8. **A realistic AI interview requires state and context, not just question generation.**

---

# 👨‍💻 Project Highlights

### Personalized AI Interviewing

Resume + JD + role-aware adaptive questioning.

### Real-Time Voice Experience

Browser microphone → audio processing → STT → AI evaluation → next question.

### Multi-Provider AI Resilience

Gemini → Groq → Ollama fallback architecture.

### Local LLM Support

Gemma 3:4b through Ollama as a cloud-independent fallback.

### Structured AI Evaluation

Schema-based responses for reliable application integration.

### Full-Stack Architecture

React frontend + Express backend + MongoDB persistence.

### Secure Application Design

JWT authentication, password hashing, environment-based secrets, CORS, Helmet and rate limiting.

---

# 📄 Project Status

**Status:** Functional resume-project prototype

**Primary focus:** Voice-based AI technical interview preparation

**Current AI text pipeline:**

```text
Gemini → Groq → Ollama / Gemma 3:4b
```

**Current voice pipeline:**

```text
AI Text → TTS → Candidate Voice
Candidate Voice → FFmpeg → STT → AI Evaluation
```

---

# ⭐ Why InterviewGPT?

InterviewGPT is designed around a simple idea:

> **A good mock interview should understand the candidate, not just ask questions.**

By combining resume intelligence, job-description context, adaptive questioning, voice interaction, speech processing, structured AI evaluation and multi-provider resilience, InterviewGPT aims to provide a more realistic and personalized interview-preparation experience.
