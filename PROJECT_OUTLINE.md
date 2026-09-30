# NEUROWELLCA-ADV-BOT - Complete Project Outline

## 📋 Project Overview

**NeurowellCA** is a comprehensive **AI-powered mental health support platform** built with cutting-edge technologies. It combines real-time AI chat, mental health assessments, crisis detection, and gamification features to provide personalized mental health support.

### 🎯 Core Purpose
- Provide **24/7 AI-driven mental health support** through conversational chat
- Conduct **standardized mental health assessments** (PHQ-9, GAD-7)
- **Detect and alert** on potential crisis situations
- Track **user progress** and provide **gamified engagement**
- Manage **role-based access control (RBAC)** for multiple user types
- Support **multi-provider LLM routing** for flexible AI backends

---

## 🏗️ Architecture Overview

### Tech Stack

#### **Backend**
- **Framework**: FastAPI 0.109.0 (async, high-performance Python web framework)
- **Language**: Python 3.11+
- **Database**: 
  - PostgreSQL (transactional data, ACID compliance)
  - Qdrant (vector search for AI embeddings)
- **Authentication**: JWT tokens with OAuth2 support
- **AI/ML**:
  - Sentence Transformers (embeddings)
  - LSTM (chat title & summary generation)
  - TensorFlow + scikit-learn (ML models)
  - VADER Sentiment Analysis
- **LLM Integration**:
  - Ollama (local models)
  - Multi-provider support (OpenAI, Anthropic, Azure, etc.)
- **Communication**:
  - Twilio (WhatsApp, SMS alerts)
  - SMTP (email notifications)

#### **Frontend**
- **Framework**: Next.js 16.1.4 (React 19)
- **Language**: TypeScript 5.9
- **Styling**: Tailwind CSS 4.1 + PostCSS
- **API Client**: Axios 1.13
- **Utilities**: marked (Markdown), DOMPurify (HTML sanitization)

#### **Deployment**
- **Containerization**: Docker + Docker Compose
- **Target**: Google Cloud (Cloud Run + Cloud SQL)

---

## 📁 Project Structure

```
NeuroWellCA-adv-bot/
├── backend/                          # FastAPI Backend
│   ├── src/
│   │   ├── api/                      # API Routes & Logic
│   │   │   ├── main.py              # FastAPI app initialization
│   │   │   ├── dependencies.py      # Shared dependencies
│   │   │   └── routes/              # API Route handlers
│   │   │       ├── auth.py          # Authentication & registration
│   │   │       ├── chat.py          # Chat with AI & crisis detection
│   │   │       ├── assessment.py    # PHQ-9 & GAD-7 assessments
│   │   │       ├── dashboard.py     # User analytics & stats
│   │   │       ├── admin.py         # Admin management
│   │   │       ├── profile.py       # User profile management
│   │   │       ├── permissions.py   # RBAC management
│   │   │       ├── feedback.py      # Message feedback system
│   │   │       ├── llm_providers.py # Multi-LLM provider config
│   │   │       └── settings_routes.py # System settings
│   │   │
│   │   ├── models/
│   │   │   ├── database.py          # SQLAlchemy DB setup
│   │   │   └── models.py            # All ORM models (30+ tables)
│   │   │
│   │   ├── services/                # Business Logic Layer
│   │   │   ├── llm_service.py       # LLM provider routing
│   │   │   ├── qdrant_service.py    # Vector search operations
│   │   │   ├── crisis_service.py    # Crisis detection logic
│   │   │   ├── guardian_alert_service.py  # Alert notifications
│   │   │   ├── email_service.py     # Email handling
│   │   │   ├── permission_service.py # RBAC logic
│   │   │   └── settings_service.py  # Config management
│   │   │
│   │   ├── ml_models/               # Machine Learning
│   │   │   └── lstm_summarizer.py   # Chat title/summary generation
│   │   │
│   │   └── utils/
│   │       └── config.py            # Configuration (TOML + env vars)
│   │
│   ├── scripts/                      # Database & Admin Scripts
│   │   ├── create_admin.py
│   │   ├── seed_rbac.py
│   │   ├── fix_permissions.py
│   │   └── test_ollama.py
│   │
│   ├── data/                         # Configuration Data
│   │   ├── ai_persona_config.json
│   │   ├── assessment_config.json
│   │   ├── crisis_detection.json
│   │   └── mental_health_resources.json
│   │
│   ├── alembic.ini                   # Database migrations
│   ├── Dockerfile                    # Production container
│   ├── config.toml                   # TOML configuration
│   └── requirements.txt              # Python dependencies
│
├── frontend/                         # Next.js Frontend
│   ├── app/
│   │   ├── layout.tsx               # Root layout
│   │   ├── page.tsx                 # Home page
│   │   ├── globals.css              # Global styles
│   │   │
│   │   ├── (auth)/                  # Auth Routes (Dynamic Group)
│   │   │   ├── login/
│   │   │   └── register/
│   │   │
│   │   ├── (dashboard)/             # Dashboard Routes
│   │   │   ├── layout.tsx
│   │   │   ├── admin/               # Admin panel
│   │   │   ├── assessment/          # Assessment wizard
│   │   │   ├── chat/                # Chat interface
│   │   │   ├── dashboard/           # Stats & analytics
│   │   │   ├── profile/             # User profile
│   │   │   └── games/               # Gamification
│   │   │       ├── breathe-balance/
│   │   │       ├── breathing-rhythm/
│   │   │       ├── bubble-pop-bliss/
│   │   │       ├── emoji-catcher/
│   │   │       ├── focus-flow/
│   │   │       ├── gratitude-garden/
│   │   │       └── [other games]/
│   │   │
│   │   └── assessment-wizard/       # Full-page assessment
│   │
│   ├── components/                   # React Components
│   │   ├── ui/                       # Reusable UI components
│   │   ├── icons/
│   │   ├── AppTheme.tsx
│   │   ├── Button.tsx
│   │   ├── Card.tsx
│   │   └── [other components]/
│   │
│   ├── lib/                          # API Client & Utilities
│   │   ├── api.ts                   # Axios API client
│   │   ├── auth.ts                  # Auth helpers
│   │   ├── chat.ts                  # Chat API
│   │   ├── assessment.ts            # Assessment API
│   │   └── feedback.ts              # Feedback API
│   │
│   ├── public/                       # Static assets
│   ├── package.json
│   ├── tsconfig.json
│   ├── tailwind.config.js
│   └── Dockerfile
│
├── data/                             # Shared Configuration Data
│   ├── ai_persona_config.json
│   ├── assessment_config.json
│   ├── crisis_detection.json
│   └── mental_health_resources.json
│
├── config.toml                       # Root configuration (TOML)
├── docker-compose.yml                # Production environment
├── docker-compose.dev.yml            # Development environment
├── requirements.txt                  # Python dependencies
└── README.md                         # Project documentation
```

---

## 🗄️ Database Schema (30+ Tables)

### Core User Management
1. **users** - User accounts with profile info & RBAC support
2. **auth_providers** - OAuth provider configuration
3. **auth_identities** - OAuth identity linking
4. **revoked_tokens** - JWT token blacklist

### RBAC (Role-Based Access Control)
5. **permissions** - Atomic authorization units (e.g., `chat.view`, `assessment.create`)
6. **permission_sets** - Grouped permissions (e.g., `crisis_responder`, `therapist`)
7. **permission_set_permissions** - Junction table for permission grouping
8. **roles** - User roles (e.g., `super_admin`, `therapist`, `patient`)
9. **role_permission_sets** - Junction table for role → permission_sets
10. **user_roles** - User → Role assignments
11. **user_permissions** - Direct user permissions (bypassing roles)
12. **user_permission_sets** - Direct user permission sets
13. **rbac_audit_logs** - Audit trail for RBAC changes

### Mental Health Data
14. **assessments** - PHQ-9 & GAD-7 scores + risk levels
15. **conversations** - Chat messages (user ↔ AI)
16. **chat_sessions** - Chat session metadata & LSTM summaries
17. **crisis_logs** - Crisis detection events
18. **guardian_alerts** - Guardian notifications for crisis situations

### Feedback & Curation
19. **message_feedback** - User feedback on AI responses
20. **golden_examples** - Admin-curated ideal responses for RAG

### System Configuration
21. **settings** - Application settings with version control
22. **llm_providers** - Multi-provider LLM configuration
23. **pending_signup_otps** - OTP verification during signup
24. **games_progress** - Gamification tracking

### Additional Tables
25. **game_achievements** (implied)
26. **user_segments** (implied)
27. Various junction/audit tables

---

## 🔌 API Routes & Endpoints

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         Frontend (Next.js)                       │
│                                                                   │
│  Login → Chat → Assessment → Dashboard → Games → Profile         │
└────────────────────────┬────────────────────────────────────────┘
                         │ Axios HTTP Requests
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    FastAPI Backend (Python)                      │
│                      :8000/api/*                                │
│                                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ Middleware Layer                                           │ │
│  │ • CORS Middleware                                          │ │
│  │ • Request Logging                                          │ │
│  │ • JWT Authentication (OAuth2)                              │ │
│  └────────────────────────────────────────────────────────────┘ │
│                         ▼                                         │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ Route Handlers (10 router modules)                         │ │
│  │                                                             │ │
│  │ • /auth → Authentication & Authorization                   │ │
│  │ • /chat → AI chat & crisis detection                       │ │
│  │ • /assessments → PHQ-9 & GAD-7                             │ │
│  │ • /dashboard → User analytics                              │ │
│  │ • /admin → Admin management                                │ │
│  │ • /rbac → RBAC management                                  │ │
│  │ • /feedback → Message feedback                             │ │
│  │ • /llm-providers → LLM config                              │ │
│  │ • /settings → System settings                              │ │
│  └────────────────────────────────────────────────────────────┘ │
│                         ▼                                         │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ Service Layer (Business Logic)                             │ │
│  │                                                             │ │
│  │ • LLM Service (Multi-provider routing)                     │ │
│  │ • Qdrant Service (Vector search)                           │ │
│  │ • Crisis Service (Detection & scoring)                     │ │
│  │ • Guardian Alert Service (Notifications)                   │ │
│  │ • Permission Service (RBAC)                                │ │
│  │ • Email Service (SMTP)                                     │ │
│  │ • Settings Service (Config)                                │ │
│  └────────────────────────────────────────────────────────────┘ │
│                         ▼                                         │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ Data Layer                                                 │ │
│  │                                                             │ │
│  │ • PostgreSQL (Transactions, Auth, Config)                  │ │
│  │ • Qdrant (Vector embeddings, RAG)                          │ │
│  │ • External LLMs (Ollama, OpenAI, Anthropic, etc.)          │ │
│  │ • Twilio (SMS/WhatsApp)                                    │ │
│  │ • SMTP (Email)                                             │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Complete API Endpoint Tree

#### **Authentication Routes** (`/api/auth`)
```
POST   /login                      → Login & get tokens
POST   /register                   → Register new user
POST   /start-signup-otp          → Initiate signup OTP flow
POST   /verify-signup-otp         → Verify OTP and complete registration
POST   /refresh-token             → Refresh expired access token
POST   /logout                    → Logout (revoke token)
POST   /forgot-password           → Request password reset
POST   /change-password           → Change password
GET    /verify-email/{token}      → Verify email address
GET    /current-user              → Get current user info
PUT    /profile                   → Update user profile
POST   /change-password           → Change password
```

#### **Chat Routes** (`/api/chat`)
```
POST   /message                    → Send message to AI
GET    /conversations             → Get user's conversations
GET    /conversations/{id}        → Get specific conversation
GET    /sessions                  → Get chat sessions
GET    /sessions/{id}             → Get session details
DELETE /conversations/{id}        → Delete conversation
POST   /reset-context             → Clear chat context
GET    /suggested-games           → Get game recommendations
POST   /stream-message            → Streaming AI response
```

#### **Assessment Routes** (`/api/assessments`)
```
POST   /submit                     → Submit PHQ-9 & GAD-7
GET    /latest                     → Get latest assessment
GET    /history                    → Get assessment history
GET    /trends                     → Get assessment trends
GET    /phq9-questions             → Get PHQ-9 questions
GET    /gad7-questions             → Get GAD-7 questions
```

#### **Dashboard Routes** (`/api/dashboard`)
```
GET    /stats                      → Get user dashboard stats
GET    /trends                     → Get assessment trends
GET    /activity                   → Get activity timeline
GET    /risk-summary               → Get current risk level
GET    /progress                   → Get gamification progress
```

#### **Admin Routes** (`/api/admin`)
```
GET    /stats                      → Database statistics
GET    /users                      → List all users
GET    /users/{id}                 → Get user details
GET    /conversations             → All conversations
GET    /assessments                → All assessments
GET    /crisis-logs                → All crisis logs
POST   /purge-old-data             → Delete old records
```

#### **Profile Routes** (included in `/api/auth`)
```
PUT    /profile                    → Update profile
POST   /change-password            → Change password
GET    /preferences                → Get user preferences
PUT    /preferences                → Update preferences
```

#### **RBAC Routes** (`/api/rbac`)
```
GET    /permissions                → List all permissions
POST   /permissions                → Create permission
GET    /permission-sets            → List permission sets
POST   /permission-sets            → Create permission set
GET    /roles                      → List roles
POST   /roles                      → Create role
GET    /roles/{id}                 → Get role details
POST   /roles/{id}                 → Update role
POST   /users/{id}/assign-role     → Assign role to user
POST   /users/{id}/revoke-role     → Revoke role from user
POST   /users/{id}/assign-permission → Assign permission
```

#### **Feedback Routes** (`/api/feedback`)
```
POST   /submit                     → Submit feedback on AI response
GET    /list                       → List user's feedback
PUT    /{id}                       → Update feedback status
GET    /golden-examples            → Get golden examples
POST   /golden-examples            → Create golden example (admin)
```

#### **LLM Provider Routes** (`/api/llm-providers`)
```
GET    /                           → List all providers
POST   /                           → Create provider
GET    /{id}                       → Get provider details
PUT    /{id}                       → Update provider
DELETE /{id}                       → Delete provider
POST   /test-connection            → Test LLM connection
GET    /available-models           → List available models
POST   /switch-default             → Set default provider
```

#### **Settings Routes** (`/api/settings`)
```
GET    /                           → Get current settings
POST   /                           → Create new settings version
PUT    /{id}                       → Update settings
POST   /{id}/rollback              → Rollback to previous version
GET    /history                    → Get settings history
```

#### **Health & System**
```
GET    /health                     → Health check
GET    /                           → Root endpoint
```

---

## 🔄 Key User Flows

### 1️⃣ **User Registration & Onboarding**
```
User enters username/password
        ↓
OTP sent to email
        ↓
User verifies OTP
        ↓
Account created in PostgreSQL
        ↓
Redirect to assessment wizard (PHQ-9 + GAD-7)
        ↓
Initial risk level calculated
        ↓
Dashboard shown
```

### 2️⃣ **Chat with AI (with Crisis Detection)**
```
User sends message
        ↓
Store in PostgreSQL conversations table
        ↓
Crisis detection service analyzes
        ├─ Keyword matching (suicide, self-harm, etc.)
        ├─ Sentiment analysis (VADER)
        └─ ML-based crisis scoring
        ↓
If crisis detected:
    ├─ Alert guardian (SMS/WhatsApp/Email via Twilio)
    ├─ Log to crisis_logs table
    └─ Provide crisis resources
        ↓
Generate AI response via LLM:
    ├─ Search Qdrant for relevant examples
    ├─ Route to configured LLM (Ollama, OpenAI, etc.)
    ├─ Generate empathetic response
    └─ Suggest gamification (optional)
        ↓
Store response in PostgreSQL
        ↓
Create vector embedding & store in Qdrant
        ↓
Return to frontend (with streaming if enabled)
```

### 3️⃣ **Assessment Submission**
```
User completes PHQ-9 + GAD-7 + stress level
        ↓
Calculate scores:
    ├─ PHQ-9: 0-27 (depression)
    ├─ GAD-7: 0-21 (anxiety)
    └─ Stress: 0-10
        ↓
Calculate risk level:
    LOW → MILD → MODERATE → MODERATELY_SEVERE → SEVERE
        ↓
Store assessment in PostgreSQL
        ↓
If severe crisis detected:
    ├─ Alert guardian
    ├─ Show crisis resources
    └─ Suggest mental health support
        ↓
Update user dashboard
        ↓
Add gamification points
```

### 4️⃣ **Crisis Detection & Guardian Alert**
```
Crisis keywords detected in chat
        ↓
Crisis service calculates crisis score
        ↓
If score > threshold:
    ├─ Check cooldown (prevent spam)
    ├─ Guardian alert service triggered
    ├─ Send via preferred channel:
    │   ├─ Twilio WhatsApp
    │   ├─ Twilio SMS
    │   └─ SMTP Email
    ├─ Log alert in guardian_alerts table
    └─ Mark user as high-risk
        ↓
Provide crisis resources to user
        ↓
Suggest immediate mental health support
```

### 5️⃣ **RBAC - Multi-Role Permission System**
```
User has multiple roles:
    ├─ Patient (base role)
    ├─ Crisis Responder (optional)
    └─ Admin (optional)
        ↓
Each role has permission sets:
    ├─ Patient: chat.view, assessment.submit, feedback.submit
    ├─ Crisis Responder: view_crisis_logs, send_alerts
    └─ Admin: manage_users, view_all_conversations
        ↓
Permission service evaluates:
    ├─ Direct user permissions
    ├─ Role-based permissions
    └─ Permission set permissions
        ↓
Endpoint access granted/denied
        ↓
All changes logged to rbac_audit_logs
```

### 6️⃣ **LLM Provider Routing**
```
Multiple LLM providers configured:
    ├─ Ollama (local, free)
    ├─ OpenAI (powerful, paid)
    ├─ Anthropic Claude (advanced)
    └─ Azure (enterprise)
        ↓
LLM service selects provider based on:
    ├─ Default configuration
    ├─ User preference (if enabled)
    ├─ Load balancing
    └─ Provider health status
        ↓
Generate embeddings via Sentence Transformers
        ↓
Search Qdrant for relevant examples
        ↓
Route request to selected LLM
        ↓
Stream or return response
        ↓
Update provider statistics
```

---

## 🎮 Gamification Features

**Games Available:**
- 🌬️ **Breathe Balance** - Breathing exercises
- 🎯 **Breathing Rhythm** - Rhythm-based breathing
- 🫧 **Bubble Pop Bliss** - Stress relief game
- 😊 **Emoji Catcher** - Mood tracking
- 🎮 **Focus Flow** - Concentration training
- 🌻 **Gratitude Garden** - Gratitude practice
- And more...

**Progress Tracking:**
- Total points
- Level progression
- Streaks
- Badges/achievements
- Conversation count
- Assessments completed

---

## 🔐 Security Architecture

### Authentication & Authorization
- **JWT Tokens**: HS256 algorithm
- **Access Token Lifetime**: 60 minutes (configurable)
- **Refresh Token Lifetime**: 7 days
- **Token Versioning**: Invalidate all tokens on critical events
- **Token Blacklist**: Revoked tokens tracked in PostgreSQL

### Password Security
- **Algorithm**: Bcrypt with salt
- **Rounds**: 12 (configurable)
- **Verification**: Secure comparison

### RBAC (Role-Based Access Control)
- **3-Level Hierarchy**:
  1. Permissions (atomic: `chat.view`)
  2. Permission Sets (grouped: `crisis_responder`)
  3. Roles (collection: `therapist`)
- **Direct Assignment**: Users can have direct permissions/sets (bypass roles)
- **Audit Trail**: All RBAC changes logged
- **Soft Deletes**: Historical tracking

### Data Protection
- **Email Verification**: OTP-based signup
- **CORS Configuration**: Whitelist of allowed origins
- **HTTPS in Production**: Enforced in Google Cloud
- **Database**: PostgreSQL with encryption at rest (GCP)
- **Sensitive Fields**: API keys encrypted in transit

---

## 📊 Mental Health Metrics

### PHQ-9 (Patient Health Questionnaire)
- Measures depression severity
- 9 questions, 0-3 points each
- Scores:
  - 0-4: Minimal
  - 5-9: Mild
  - 10-14: Moderate
  - 15-19: Moderately Severe
  - 20+: Severe

### GAD-7 (Generalized Anxiety Disorder)
- Measures anxiety severity
- 7 questions, 0-3 points each
- Scores:
  - 0-4: Minimal
  - 5-9: Mild
  - 10-14: Moderate
  - 15+: Severe

### Stress Level (User-Reported)
- 0-10 scale
- Influences risk calculation

### Risk Level (Combined)
- **LOW**: PHQ-9 < 5, GAD-7 < 5, Stress < 3
- **MILD**: PHQ-9 5-9, GAD-7 5-9, Stress 3-5
- **MODERATE**: PHQ-9 10-14, GAD-7 10-14, Stress 5-7
- **MODERATELY_SEVERE**: PHQ-9 15-19, GAD-7 15-19, Stress 7-9
- **SEVERE**: PHQ-9 ≥ 20, GAD-7 ≥ 15, Stress ≥ 9

---

## 🚀 Deployment Architecture

### Local Development
```bash
# Start infrastructure
docker-compose up -d postgres qdrant ollama

# Run backend
cd backend
pip install -r requirements.txt
python -m uvicorn src.api.main:app --reload

# Run frontend
cd frontend
npm install
npm run dev
```

### Production (Google Cloud)
- **Compute**: Cloud Run (serverless)
- **Database**: Cloud SQL (PostgreSQL)
- **Vector DB**: Qdrant (Cloud Run or VM)
- **Storage**: Cloud Storage (assets)
- **DNS**: Cloud DNS
- **Load Balancer**: Cloud Load Balancing

### Docker Compose Services
```yaml
Services:
- postgres         # Main database
- qdrant          # Vector search
- ollama          # Local LLM
- backend         # FastAPI app
- frontend        # Next.js app
```

---

## 📝 Configuration System

### TOML Configuration (`config.toml`)
- Non-sensitive settings (safe to commit)
- Structured format for complex config

### Environment Variables (`.env`)
- Sensitive values (API keys, passwords)
- Git-ignored
- Production overrides

### Hierarchy (Priority Order)
1. **Environment Variables** (highest priority)
2. **TOML Config**
3. **Default Values** (lowest)

### Key Configuration Sections
```toml
[app]          # Application metadata
[api]          # API host/port
[security]     # JWT, tokens
[database]     # PostgreSQL URL
[qdrant]       # Vector DB config
[ollama]       # Local LLM
[lstm]         # ML model paths
[cors]         # CORS origins
[whatsapp]     # Twilio config
[crisis_detection]  # Thresholds
[data_retention]    # Cleanup settings
[logging]      # Log configuration
```

---

## 📦 Dependencies Summary

### Backend (40+ packages)
**Core Framework**: FastAPI, Uvicorn
**Database**: SQLAlchemy, asyncpg, psycopg2, Alembic
**Auth**: python-jose, passlib, bcrypt
**AI/ML**: torch, tensorflow, scikit-learn, sentence-transformers
**Vector DB**: qdrant-client
**HTTP**: httpx, requests
**NLP**: vaderSentiment, marked (server-side)
**Communication**: twilio, email (SMTP)
**Utils**: pydantic, python-dotenv, python-dateutil, loguru

### Frontend (12+ packages)
**Framework**: Next.js, React, React-DOM
**Language**: TypeScript
**Styling**: Tailwind CSS, PostCSS
**API**: Axios
**Utils**: marked, DOMPurify
**Development**: ESLint, various type definitions

---

## 🔄 Data Flow Examples

### Example 1: Crisis Detection Flow
```
User Message: "I can't take this anymore"
        ↓
Unsafe text patterns matched: YES
VADER sentiment: -0.85 (very negative)
ML crisis score: 85/100
        ↓
Crisis detected! ✓
        ↓
Guardian alert cooldown checked: OK
        ↓
Compose alert:
"Your child just expressed crisis thoughts in the chat.
 Crisis message: 'I can't take this anymore'
 Timestamp: 2026-04-29 14:32:00
 Please reach out to them. [Resource links...]"
        ↓
Send via Twilio WhatsApp
        ↓
Log to crisis_logs & guardian_alerts
        ↓
Return empathetic AI response + crisis resources
```

### Example 2: Multi-LLM Routing
```
User sends chat message
        ↓
Check LLM configuration:
- Ollama (local): Healthy ✓
- OpenAI: Quota exceeded ✗
- Anthropic: Not configured
        ↓
Route to Ollama (default, healthy)
        ↓
Generate embeddings (Sentence Transformers)
        ↓
Search Qdrant:
[Find 5 most similar past conversations]
        ↓
Build context with examples
        ↓
Call Ollama with prompt + examples
        ↓
Stream response back to frontend
        ↓
Update Ollama stats: +1 request, +154 tokens
```

### Example 3: Assessment Risk Calculation
```
User submits assessment:
PHQ-9: [2, 1, 3, 2, 2, 1, 0, 1, 1] = 13 (Moderate)
GAD-7: [2, 2, 1, 3, 1, 2, 1] = 12 (Moderate)
Stress: 7 (Self-reported)
        ↓
Calculate risk level:
- PHQ-9=13 ≥ 10? YES → at least MODERATE
- GAD-7=12 ≥ 10? YES → at least MODERATE
- Stress=7 ≥ 7? YES → triggers MODERATE_SEVERE
        ↓
Final risk: MODERATELY_SEVERE
        ↓
Store all individual answers + overall score
        ↓
Check if triggered alerts:
- PHQ-9 ≥ 20? NO
- GAD-7 ≥ 15? NO
- Stress ≥ 9? NO
        ↓
Risk level: MODERATELY_SEVERE (no immediate alert)
        ↓
Update dashboard
        ↓
Add gamification points: +50
        ↓
Suggest resources for moderate-severe anxiety
```

---

## 📡 API Integration Points

### External Services
1. **Twilio** (WhatsApp, SMS)
   - Endpoint: `api.twilio.com`
   - Auth: Account SID + Token
   - Methods: SMS, WhatsApp, Voice

2. **LLM Providers**
   - Local: Ollama (`http://localhost:11434`)
   - Cloud: OpenAI, Anthropic, Azure, Google
   - Custom: Any provider with API

3. **SMTP Mail Service**
   - Any SMTP provider (Gmail, SendGrid, etc.)
   - Used for email alerts & notifications

4. **Vector Database (Qdrant)**
   - Local or cloud deployment
   - REST API for operations
   - Embedding dimension: 384

### Internal Service Integration
```
Frontend (Next.js)
        ↓
Axios HTTP Client
        ↓
FastAPI Backend
        ↓
Service Layer
        ├─→ PostgreSQL (transactions)
        ├─→ Qdrant (vectors)
        ├─→ Ollama/LLM (AI)
        ├─→ Twilio (alerts)
        └─→ SMTP (email)
```

---

## 🎯 Key Features Breakdown

### 1. **AI Chat System**
- Real-time conversation
- Crisis detection
- Sentiment analysis
- Context awareness (RAG)
- Streaming responses
- Session management

### 2. **Mental Health Assessments**
- PHQ-9 & GAD-7 questionnaires
- Risk level calculation
- Trend tracking
- Assessment history
- Automated alerts

### 3. **Crisis Management**
- Pattern recognition
- Guardian notifications
- Multi-channel alerts (SMS, WhatsApp, Email)
- Crisis resource provision
- Audit logging

### 4. **Gamification**
- Mini-games (7+ games)
- Point system
- Level progression
- Streak tracking
- Achievements/badges

### 5. **RBAC System**
- Fine-grained permissions
- Role assignments
- Permission sets
- Audit logging
- Dynamic access control

### 6. **Multi-LLM Support**
- Provider routing
- Health checks
- Statistics tracking
- User model selection (optional)
- Fallback handling

### 7. **Feedback & Learning**
- Message feedback system
- Golden examples curation
- Admin review workflow
- Data quality tracking
- Continuous improvement

### 8. **Settings Management**
- Version-controlled configuration
- Rollback capability
- Change tracking
- Audit trail

---

## 🔧 Development Tools & Commands

### Backend Setup
```bash
# Install dependencies
pip install -r requirements.txt

# Run migrations
alembic upgrade head

# Create admin user
python scripts/create_admin.py

# Setup RBAC
python scripts/seed_rbac.py

# Run server
python -m uvicorn src.api.main:app --reload
```

### Frontend Setup
```bash
# Install dependencies
npm install

# Development server
npm run dev

# Build
npm build

# Production run
npm start
```

### Docker
```bash
# Development environment
docker-compose -f docker-compose.dev.yml up

# Production environment
docker-compose up -d
```

---

## 📈 Scalability Considerations

### Horizontal Scaling
- **Stateless FastAPI**: Deploy multiple instances
- **PostgreSQL**: Connection pooling via PgBouncer
- **Qdrant**: Cluster mode available
- **Ollama**: Multiple instances for load distribution

### Performance Optimization
- **Caching**: JWT token caching
- **Vector Search**: Qdrant indexing
- **Database**: PostgreSQL indexes on common queries
- **Async Operations**: Non-blocking I/O
- **Streaming**: Reduce latency for large responses

### Monitoring & Logging
- **Application Logging**: Structured logs with loguru
- **Request Logging**: All HTTP requests logged
- **Error Tracking**: Centralized error handling
- **Health Checks**: `/health` endpoint for load balancers

---

## 🚧 Project Status

### Completed
✅ Core FastAPI backend
✅ PostgreSQL database schema
✅ Qdrant vector search integration
✅ Multi-LLM provider support
✅ RBAC system (3-tier)
✅ Crisis detection
✅ Guardian alerts (Twilio + SMTP)
✅ PHQ-9 & GAD-7 assessments
✅ Next.js frontend
✅ Docker containerization
✅ Gamification framework

### In Progress / Planned
🚀 Advanced analytics dashboard
🚀 Mobile app
🚀 OAuth integration (Google, GitHub)
🚀 Advanced ML models
🚀 Multi-language support
🚀 Therapist collaboration features

---

## 📚 Additional Resources

- **Main Backend**: `backend/src/api/main.py`
- **Database Models**: `backend/src/models/models.py`
- **Configuration**: `backend/config.toml` & `backend/src/utils/config.py`
- **Services**: `backend/src/services/`
- **Routes**: `backend/src/api/routes/`
- **Frontend**: `frontend/app/`

---

**Last Updated**: April 29, 2026
**Version**: 2.0.0
**Tech Stack**: FastAPI + Next.js + PostgreSQL + Qdrant + Ollama
