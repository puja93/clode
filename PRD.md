# Product Requirements Document (PRD)
## Voice-Enabled Trivia Quiz Platform

---

## 1. Executive Summary

### 1.1 Product Overview
A web-based trivia quiz platform featuring voice-interactive capabilities powered by a LiveKit voice agent. Users engage with trivia questions through natural voice commands, creating an immersive and hands-free quiz experience.

### 1.2 Product Vision
To deliver a futuristic, delightful trivia experience where users can test their knowledge through seamless voice interactions, eliminating traditional point-and-click interfaces in favor of conversational AI.

---

## 2. Product Goals & Objectives

### 2.1 Primary Goals
- Create an accessible, voice-first trivia quiz experience
- Provide an engaging, futuristic user interface with smooth animations
- Enable natural voice interactions for quiz participation
- Deliver immediate feedback and detailed answer reviews

### 2.2 Success Metrics
- User engagement: Average quiz completion rate
- Voice recognition accuracy: >95% correct answer capture rate
- User satisfaction: Post-quiz feedback ratings
- Technical performance: Page load time <2 seconds, voice response latency <1 second

---

## 3. User Stories & Use Cases

### 3.1 Primary User Story
**As a** trivia enthusiast
**I want to** participate in voice-enabled quizzes
**So that** I can test my knowledge in an interactive, hands-free manner

### 3.2 Detailed User Journey

#### Phase 1: Quiz Introduction
1. User lands on the homepage
2. Voice agent reads the quiz guidelines and background information
3. Agent asks if user is ready to begin
4. User confirms readiness via voice

#### Phase 2: Quiz Participation
1. Agent presents multiple-choice questions one at a time
2. User responds by speaking the letter corresponding to their chosen answer (e.g., "A", "B", "C", or "D")
3. Agent acknowledges the answer and moves to the next question
4. User **cannot** ask clarifying questions during this phase
5. Process repeats for all 10 questions

#### Phase 3: Review & Clarification
1. Agent displays all questions with correct/incorrect indicators
2. User can view their score and review answers
3. User **can** interact with the agent to ask for clarifications
4. Agent explains why specific answers are correct or incorrect upon request

---

## 4. Functional Requirements

### 4.1 Quiz System

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-1 | Question Retrieval | P0 | System retrieves 10 trivia questions from OpenTDB API |
| FR-2 | Question Format | P0 | Support for both boolean (True/False) and multiple-choice (4 options) questions |
| FR-3 | Voice Answer Input | P0 | Users answer by speaking the letter of their choice |
| FR-4 | Answer Validation | P0 | System validates and records user responses |
| FR-5 | Progress Tracking | P1 | Display current question number (e.g., "Question 3 of 10") |

### 4.2 Voice Agent Capabilities

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-6 | Introduction Narration | P0 | Agent reads quiz guidelines at the start |
| FR-7 | Question Reading | P0 | Agent reads each question and answer options aloud |
| FR-8 | Response Recognition | P0 | Agent recognizes spoken letters (A, B, C, D, True, False) |
| FR-9 | Phase-Based Interaction | P0 | No clarifications during quiz; clarifications allowed after completion |
| FR-10 | Results Narration | P1 | Agent provides summary of performance |
| FR-11 | Explanation Mode | P1 | Agent explains correct answers when requested |

### 4.3 Review System

| ID | Requirement | Priority | Description |
|----|-------------|----------|-------------|
| FR-12 | Answer Display | P0 | Show all questions with correct/incorrect indicators |
| FR-13 | Score Calculation | P0 | Display total score (e.g., "7 out of 10") |
| FR-14 | Clarification Requests | P1 | Allow users to select specific questions for explanation |
| FR-15 | Interactive Review | P1 | Enable voice-based navigation through review items |

---

## 5. Technical Requirements

### 5.1 Architecture Overview

```
┌──────────────────────────────────────────────────────────────┐
│                      Your Application                        │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌────────────────────┐         ┌──────────────────────┐    │
│  │  Frontend (Vite)   │  HTTP   │  Backend (FastAPI)   │    │
│  │                    │◄───────►│                      │    │
│  │ - React UI         │  REST   │ - Token Generation   │    │
│  │ - Animations       │  API    │ - Session Manager    │    │
│  │ - User Input       │         │ - API Keys (secure)  │    │
│  │ - Quiz Display     │         │ - OpenTDB API calls  │    │
│  │ - Voice Agent      │         │ - Webhook Receiver   │    │
│  │   Participant      │         │                      │    │
│  └────────────────────┘         │ ┌──────────────────┐ │    │
│           │                     │ │ Voice Agent      │ │    │
│           │                     │ │ (LiveKit Agents) │ │    │
│           │                     │ │                  │ │    │
│           │                     │ │ - STT/TTS Logic  │ │    │
│           │                     │ │ - Quiz Voice I/O │ │    │
│           │                     │ │ - LLM calls      │ │    │
│           │                     │ └──────────────────┘ │    │
│           │                     │ ┌──────────────────┐ │    │
│           │                     │ │ SQLite Database  │ │    │
│           │                     │ │ - Sessions       │ │    │
│           │                     │ │ - Answers        │ │    │
│           │                     │ │ - Scores         │ │    │
│           │                     │ └──────────────────┘ │    │
│           │                     └──────────────────────┘    │
│           │                              │                  │
│           └──────────────────┬───────────┘                  │
│                              │ WebRTC                       │
│                              │ (Voice)                      │
└──────────────────────────────┼──────────────────────────────┘
                               │
                    ┌──────────▼─────────┐
                    │   LiveKit Cloud    │
                    │   (Voice Router)   │
                    └────────────────────┘
                               │
                ┌──────────────┼──────────────┐
                │              │              │
                ▼              ▼              ▼
        ┌──────────────┐ ┌──────────┐ ┌─────────────┐
        │  OpenAI API  │ │ElevenLabs│ │  OpenTDB    │
        │ (STT, LLM)   │ │  (TTS)   │ │  (Questions)│
        └──────────────┘ └──────────┘ └─────────────┘
```

### 5.2 Technology Stack

#### Frontend
- **Framework**: Vite.js with React (or Vue)
- **Styling**: Scandinavian design principles (minimalist, clean, functional)
- **Animations**: Anime.js + additional animation libraries for futuristic effects
- **State Management**: React Context or similar based on framework choice
- **HTTP Client**: Axios or Fetch API for backend communication

#### Backend/API Server
- **Framework**: FastAPI (Python 3.9+)
- **Database**: SQLite for session management and quiz state
- **Key Responsibilities**:
  - Secure API key storage (OpenAI, ElevenLabs, LiveKit)
  - LiveKit token generation for frontend
  - Session management (quiz progress, answers, scores)
  - Webhook receiver for LiveKit events
  - REST API endpoints for quiz operations

#### Voice Agent (Embedded in Backend)
- **Platform**: LiveKit Agents Framework (Python)
- **Deployment**: Runs as part of the FastAPI backend service
- **Components**:
  - **STT (Speech-to-Text)**: OpenAI Whisper API
  - **LLM (Language Model)**: OpenAI GPT-4/GPT-3.5
  - **TTS (Text-to-Speech)**: ElevenLabs API
  - **Voice Communication**: LiveKit WebRTC
- **Responsibilities**:
  - Joins LiveKit room when quiz starts
  - Reads quiz guidelines and questions (TTS)
  - Listens to user answers (STT)
  - Validates answers using LLM
  - Provides explanations in review phase
  - Updates backend with answer data

#### External Data Sources
- **Quiz Data**: OpenTDB API
  - Endpoint: `https://opentdb.com/api.php?amount=10`
  - No authentication required
  - Called by backend, not frontend (security)

### 5.3 Communication Flow

#### Frontend → Backend Communication
```
1. User opens app
2. Frontend requests LiveKit token: GET /api/quiz/token
3. Backend generates token securely using LiveKit API keys
4. Frontend connects to LiveKit room with token
5. Agent joins room and quiz begins
6. Frontend listens to voice from agent (WebRTC via LiveKit)
7. User speaks answer
8. Frontend sends answer to backend: POST /api/quiz/answer
9. Backend updates SQLite session with answer
10. Agent continues (backend updates agent via internal calls)
11. Quiz completes
12. Frontend requests results: GET /api/quiz/results/{session_id}
13. Backend returns score and answers from SQLite
```

#### Backend → External APIs
```
- OpenTDB: Backend fetches 10 questions via HTTP GET
- OpenAI: Agent calls for STT (transcribe speech) and LLM (validate/explain)
- ElevenLabs: Agent calls for TTS (convert text to speech)
- LiveKit: Backend calls for token generation; Agent connects via WebRTC
```

### 5.4 API Integration Requirements

| Service | Purpose | Called By | Authentication | Configuration |
|---------|---------|-----------|-----------------|-----------------|
| OpenTDB | Question retrieval | Backend | None | Public API |
| OpenAI | STT & LLM | Backend + Agent | API Key | Environment variable |
| ElevenLabs | TTS | Agent | API Key | Environment variable |
| LiveKit | Voice communication | Backend + Agent | API Key + Secret | Environment variables |

### 5.5 Environment Variables

#### Backend (.env file - NEVER expose these to frontend)
```bash
# OpenAI Configuration
OPENAI_API_KEY=your_openai_api_key_here

# ElevenLabs Configuration
ELEVENLABS_API_KEY=your_elevenlabs_api_key_here

# LiveKit Configuration
LIVEKIT_URL=your_livekit_cloud_url
LIVEKIT_API_KEY=your_livekit_api_key_here
LIVEKIT_API_SECRET=your_livekit_api_secret_here

# Database
DATABASE_URL=sqlite:///./quiz.db

# Application
ENVIRONMENT=production
```

#### Frontend (.env file - NO SENSITIVE KEYS)
```bash
# Backend API URL
VITE_API_URL=http://localhost:8000

# This is OK to expose (public info)
VITE_APP_NAME=Voice Trivia Quiz
```


### 5.6 LiveKit Agent Implementation Details

#### Agent Responsibilities
- **Quiz Introduction Phase**:
  - Reads quiz guidelines and background info (TTS)
  - Asks user if ready to begin
  - Listens for readiness confirmation (STT)

- **Quiz Phase**:
  - Reads each question and options (TTS)
  - Listens for user's answer letter/choice (STT)
  - Uses LLM to validate/confirm answer semantics
  - Moves to next question automatically
  - **Cannot** engage with clarification questions

- **Review Phase**:
  - Agent resets interaction model
  - Listens for user requests for explanations
  - Uses LLM to generate explanations for incorrect answers
  - Reads explanations aloud (TTS)

#### Implementation References
- LiveKit Agents Framework: https://docs.livekit.io/agents/
- LiveKit Voice AI Quickstart: https://docs.livekit.io/agents/start/voice-ai/
- OpenAI Whisper (STT): https://platform.openai.com/docs/guides/speech-to-text
- ElevenLabs TTS: https://elevenlabs.io/docs/api-reference

---

## 6. User Interface & User Experience

### 6.1 Design Principles
- **Scandinavian Design**: Minimalist, functional, clean aesthetics
- **Futuristic Elements**: Smooth animations, modern typography, subtle gradients
- **Accessibility**: High contrast, clear visual hierarchy, voice-first interaction

### 6.2 Key Screens

#### 6.2.1 Homepage / Introduction Screen
- **Elements**:
  - Quiz background/context text (prominent display)
  - Voice agent indicator (animated, showing agent is "speaking")
  - "Listening..." indicator when awaiting user response
  - Visual representation of voice agent (avatar or abstract visualization)
- **Interactions**:
  - Agent reads guidelines automatically on page load
  - User confirms readiness via voice

#### 6.2.2 Quiz Screen
- **Elements**:
  - Question counter (e.g., "Question 3 of 10")
  - Current question text (large, readable font)
  - Answer options (A, B, C, D) with letters clearly labeled
  - Voice activity indicator
  - Visual feedback when answer is selected
- **Interactions**:
  - Questions appear with smooth transitions
  - Answer options animate when user speaks
  - Selected answer highlights before moving to next question

#### 6.2.3 Review Screen
- **Elements**:
  - Score summary (large, celebratory or encouraging)
  - List of all questions with:
    - ✓ Correct answer indicator (green)
    - ✗ Incorrect answer indicator (red)
    - User's selected answer
    - Correct answer
  - Voice agent re-enabled for interactions
  - "Ask for explanation" prompt
- **Interactions**:
  - User can scroll through results
  - User can select questions verbally for clarification
  - Agent provides detailed explanations

### 6.3 Animation Requirements
- **Page Transitions**: Smooth fade-in/fade-out effects
- **Question Transitions**: Slide or fade animations (300-500ms)
- **Answer Selection**: Pulse or highlight effect
- **Voice Activity**: Waveform or particle animation during speech
- **Score Reveal**: Animated counter or celebration effect
- **Loading States**: Elegant skeleton screens or loaders

### 6.4 Responsive Design
- Optimized for desktop and tablet (primary focus)
- Mobile support (secondary, may require adapted UX for voice)

---

## 7. Non-Functional Requirements

### 7.1 Performance
- **Page Load Time**: <2 seconds on standard broadband
- **Voice Latency**: <1 second from speech to recognition
- **Animation Frame Rate**: 60 FPS for smooth animations

### 7.2 Reliability
- **Uptime**: 99.5% availability
- **Error Handling**: Graceful degradation if voice agent fails
- **Fallback**: Provide manual click option if voice fails repeatedly

### 7.3 Security
- API keys stored securely in environment variables
- No storage of sensitive user data
- HTTPS required for production deployment

### 7.4 Browser Compatibility
- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)

### 7.5 Accessibility
- Keyboard navigation support
- Screen reader compatibility (where applicable)
- Clear visual feedback for all voice interactions

---

## 8. Dependencies & Integrations

### 8.1 External Dependencies
1. **OpenTDB API**: Free trivia question database
   - No rate limiting specified
   - Public endpoint

2. **OpenAI API**: Paid service
   - Requires API key
   - Usage-based pricing

3. **ElevenLabs API**: Paid service
   - Requires API key
   - Usage-based pricing

4. **LiveKit**: Open-source/cloud service
   - Requires setup and configuration
   - Self-hosted or cloud options

### 8.2 Development & Runtime Dependencies

#### Frontend
- Node.js (v18+ recommended)
- npm/yarn/pnpm
- Vite build tooling
- React (or Vue)
- Anime.js
- LiveKit client SDK (web)
- Axios or Fetch API

#### Backend (Python)
- Python 3.9+
- FastAPI web framework
- Uvicorn (ASGI server)
- python-dotenv (environment variables)
- httpx (async HTTP client)
- SQLAlchemy or similar ORM
- livekit-agents (LiveKit Python SDK)
- livekit (LiveKit server SDK)
- openai (OpenAI SDK)
- requests (HTTP library)

#### System Requirements
- Git
- Docker (optional, for deployment)
- Render account (for hosting)

---

## 9. Constraints & Assumptions

### 9.1 Constraints
- Quiz length fixed at 10 questions per session
- Voice is the primary interaction method (no clicking for answers during quiz)
- User must have microphone access
- Internet connection required

### 9.2 Assumptions
- Users have functional microphones
- Users are in a reasonably quiet environment
- Users speak clearly and in a supported language
- OpenTDB API remains available and free

---

## 10. Out of Scope (v1.0)

The following features are explicitly **not included** in the initial version:

- User authentication/login system
- User profiles or progress tracking across sessions
- Leaderboards or social features
- Custom quiz creation by users
- Multiple language support
- Mobile app (native iOS/Android)
- Offline mode
- Question difficulty selection
- Timed questions/speed rounds
- Multiplayer functionality

---

## 11. Future Enhancements (Post-v1.0)

### 11.1 Potential Features
- User accounts with progress tracking
- Multiple quiz categories/topics
- Difficulty level selection
- Daily challenges
- Social sharing of scores
- Achievement system
- Customizable quiz length (5/10/20 questions)
- Multi-language support
- Voice customization (different agent voices)

---

## 12. Development Phases

### Phase 1: Backend Foundation (Week 1-2)
**Backend (Python + FastAPI)**
- Set up FastAPI project structure
- Configure SQLite database with session schema
- Implement environment variable management
- Create OpenTDB API integration (fetch 10 questions)
- Set up LiveKit token generation endpoint
- Create basic API routes (`/api/quiz/token`, `/api/quiz/start`)

**Frontend (Vite + React)**
- Set up Vite project with React
- Implement basic Scandinavian-designed layout
- Create homepage and quiz introduction screens
- Implement Anime.js animation framework

### Phase 2: Backend Voice Agent Setup (Week 3)
**Backend**
- Set up LiveKit Agents Framework (Python)
- Configure OpenAI API integration (for STT and LLM)
- Configure ElevenLabs integration (for TTS)
- Implement agent entry point
- Test basic voice connection to LiveKit

### Phase 3: Core Quiz Agent Logic (Week 4-5)
**Backend Agent**
- Implement quiz introduction (read guidelines, ask if ready)
- Implement question reading loop (TTS)
- Implement answer listening (STT)
- Implement answer validation (LLM)
- Manage quiz phase state (no clarifications during quiz)

**Backend API**
- Create `/api/quiz/answer` endpoint to receive answers from frontend
- Create `/api/quiz/session/{id}` endpoints for session management
- Implement session update with answers in SQLite

### Phase 4: Frontend Quiz Interface (Week 5-6)
- Connect frontend to backend API for LiveKit tokens
- Implement voice visualizations and loading states
- Create question display with answer options
- Implement answer submission flow
- Add smooth transitions between questions
- Test microphone access and permissions

### Phase 5: Review & Explanation System (Week 7)
**Backend Agent**
- Implement review phase (different interaction mode)
- Implement explanation generation (LLM)
- Implement explanation reading (TTS)

**Backend API**
- Create `/api/quiz/results/{id}` endpoint
- Implement score calculation

**Frontend**
- Build results/review screen
- Display correct/incorrect answers
- Implement interaction for requesting explanations
- Display explanations from agent

### Phase 6: Polish & Animations (Week 8)
- Add Anime.js animations for:
  - Page transitions
  - Question reveals
  - Answer selections
  - Score displays
  - Voice activity indicators
- Optimize performance
- Cross-browser testing
- Accessibility review

### Phase 7: Testing & Deployment (Week 9)
- End-to-end testing
- Load testing for voice agent
- Deployment to Render (both frontend and backend)
- Production environment configuration
- Monitoring and logging setup
- Bug fixes and final adjustments

---

## 13. Success Criteria

### 13.1 MVP Launch Criteria
- [ ] User can complete a full 10-question quiz using only voice
- [ ] Voice agent successfully reads all questions and options
- [ ] System accurately captures voice responses (>90% accuracy)
- [ ] Review screen displays correct/incorrect answers
- [ ] User can request explanations after quiz completion
- [ ] UI follows Scandinavian design principles
- [ ] Animations are smooth and enhance user experience

### 13.2 Quality Gates
- [ ] All functional requirements met
- [ ] Voice latency <1 second
- [ ] Page load time <2 seconds
- [ ] No critical bugs
- [ ] Tested across major browsers
- [ ] API keys securely configured

---

## 14. Risks & Mitigations

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Voice recognition accuracy low | High | Medium | Use high-quality OpenAI Whisper; provide visual feedback |
| OpenTDB API downtime | High | Low | Implement fallback question set or caching |
| High API costs (OpenAI/ElevenLabs) | Medium | Medium | Monitor usage; implement rate limiting |
| Poor microphone quality | Medium | High | Provide clear audio requirements; noise cancellation |
| LiveKit configuration complexity | Medium | Medium | Follow official docs; allocate time for learning |
| Animation performance issues | Low | Medium | Optimize animations; test on lower-end devices |

---

## 15. Appendix

### 15.1 Glossary
- **STT**: Speech-to-Text
- **TTS**: Text-to-Speech
- **LLM**: Large Language Model
- **VAD**: Voice Activity Detection
- **OpenTDB**: Open Trivia Database

### 15.2 References
- LiveKit Documentation: https://docs.livekit.io/home
- OpenTDB API: https://opentdb.com/api_config.php
- Anime.js: https://animejs.com/
- Scandinavian Design Principles: (to be researched)

### 15.3 Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-06 | Generated from Storyline | Initial PRD creation |
| 2.0 | 2025-11-06 | Architecture Refinement | Finalized tech stack (Python + FastAPI + SQLite), embedded agent in backend, detailed communication flows, comprehensive development phases |

---

**Document Status**: Ready for Development
**Last Updated**: 2025-11-06
**Next Review**: After Phase 1 completion
