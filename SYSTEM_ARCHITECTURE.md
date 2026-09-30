# NEUROWELLCA - System Architecture & Data Relationships

## 🏗️ System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          USER LAYER (Frontend)                               │
│                                                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │  Mobile  │  │ Desktop  │  │  Tablet  │  │ Browser  │  │ PWA App  │    │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘    │
│       │             │             │             │             │           │
└───────┼─────────────┼─────────────┼─────────────┼─────────────┼───────────┘
        │             │             │             │             │
        └─────────────┴─────────────┴─────────────┴─────────────┘
                            │
                   HTTP/HTTPS over TLS 1.3
                            │
┌───────────────────────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER (Next.js)                               │
│                                                                               │
│  ┌─ Port 3000 ──────────────────────────────────────────────────────────┐  │
│  │                                                                       │  │
│  │  Next.js 16.1.4 (React 19 + TypeScript)                             │  │
│  │                                                                       │  │
│  │  Pages:                                                              │  │
│  │  ├─ Login / Register (OTP verification)                             │  │
│  │  ├─ Chat Interface (real-time messaging)                            │  │
│  │  ├─ Assessment Wizard (PHQ-9 + GAD-7)                               │  │
│  │  ├─ Dashboard (analytics & progress)                                │  │
│  │  ├─ Profile Management                                              │  │
│  │  ├─ Admin Panel (user/role management)                              │  │
│  │  ├─ Mini-Games (7+ gamification games)                              │  │
│  │  └─ Settings (preferences)                                           │  │
│  │                                                                       │  │
│  │  Components:                                                         │  │
│  │  ├─ UI Components (Button, Card, Input)                             │  │
│  │  ├─ Chat Interface                                                   │  │
│  │  ├─ Assessment Forms                                                 │  │
│  │  └─ Charts/Analytics                                                 │  │
│  │                                                                       │  │
│  │  State Management:                                                   │  │
│  │  ├─ localStorage (tokens, preferences)                               │  │
│  │  ├─ React hooks (local state)                                        │  │
│  │  └─ API cache (axios)                                                │  │
│  │                                                                       │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                               │
└───────────────────────────────────────────────────────────────────────────────┘
        │
        │  REST API Calls (Axios)
        │  WebSocket for real-time updates (optional)
        │
┌───────┴─────────────────────────────────────────────────────────────────────┐
│                    API GATEWAY / LOAD BALANCER                               │
│                    (Google Cloud Load Balancing)                             │
└───────┬─────────────────────────────────────────────────────────────────────┘
        │
        │
┌───────┴──────────────────────────────────────────────────────────────────────┐
│                   APPLICATION LAYER (FastAPI Backend)                        │
│                                                                              │
│  ┌─ Port 8000 ───────────────────────────────────────────────────────────┐ │
│  │                                                                        │ │
│  │  FastAPI 0.109.0 (Async Python Web Framework)                        │ │
│  │                                                                        │ │
│  │  ┌─ MIDDLEWARE LAYER ─────────────────────────────────────────────┐ │ │
│  │  │ • CORS Middleware                                              │ │ │
│  │  │ • HTTP Request Logging                                         │ │ │
│  │  │ • JWT Authentication (OAuth2)                                  │ │ │
│  │  │ • RBAC Permission Checking                                     │ │ │
│  │  │ • Error Handling & Exception Mapping                           │ │ │
│  │  └────────────────────────────────────────────────────────────────┘ │ │
│  │                                                                        │ │
│  │  ┌─ ROUTE HANDLERS (10 Router Modules) ────────────────────────────┐ │ │
│  │  │                                                                   │ │ │
│  │  │  📍 /api/auth             → Authentication & User Management    │ │ │
│  │  │  📍 /api/chat             → AI Chat & Conversations            │ │ │
│  │  │  📍 /api/assessments      → PHQ-9 & GAD-7 Assessments          │ │ │
│  │  │  📍 /api/dashboard        → User Analytics & Stats             │ │ │
│  │  │  📍 /api/admin            → Admin Functions                    │ │ │
│  │  │  📍 /api/rbac             → Role-Based Access Control          │ │ │
│  │  │  📍 /api/feedback         → Message Feedback & Learning        │ │ │
│  │  │  📍 /api/llm-providers    → LLM Configuration & Routing        │ │ │
│  │  │  📍 /api/settings         → System Settings Management         │ │ │
│  │  │  📍 /api/games            → Gamification Endpoints (implied)   │ │ │
│  │  │                                                                   │ │ │
│  │  └───────────────────────────────────────────────────────────────────┘ │ │
│  │                                                                        │ │
│  │  ┌─ SERVICE LAYER (Business Logic) ───────────────────────────────┐ │ │
│  │  │                                                                 │ │ │
│  │  │  🔧 LLM Service                                               │ │ │
│  │  │     ├─ Multi-provider routing (Ollama, OpenAI, Anthropic)    │ │ │
│  │  │     ├─ Health checks & fallback                              │ │ │
│  │  │     ├─ Token counting                                        │ │ │
│  │  │     └─ Response generation                                   │ │ │
│  │  │                                                                 │ │ │
│  │  │  🔧 Qdrant Service                                            │ │ │
│  │  │     ├─ Vector embeddings (Sentence Transformers)             │ │ │
│  │  │     ├─ Store & search conversations                          │ │ │
│  │  │     ├─ Similarity matching                                   │ │ │
│  │  │     └─ RAG context retrieval                                 │ │ │
│  │  │                                                                 │ │ │
│  │  │  🔧 Crisis Service                                            │ │ │
│  │  │     ├─ Keyword detection                                     │ │ │
│  │  │     ├─ Sentiment analysis (VADER)                            │ │ │
│  │  │     ├─ ML-based crisis scoring                               │ │ │
│  │  │     └─ Risk level calculation                                │ │ │
│  │  │                                                                 │ │ │
│  │  │  🔧 Guardian Alert Service                                    │ │ │
│  │  │     ├─ Cooldown tracking                                     │ │ │
│  │  │     ├─ Multi-channel delivery (SMS/WhatsApp/Email)           │ │ │
│  │  │     ├─ Retry logic                                           │ │ │
│  │  │     └─ Alert logging                                         │ │ │
│  │  │                                                                 │ │ │
│  │  │  🔧 Permission Service                                        │ │ │
│  │  │     ├─ User permission resolution                            │ │ │
│  │  │     ├─ Role-based access evaluation                          │ │ │
│  │  │     ├─ Permission set aggregation                            │ │ │
│  │  │     └─ Audit logging                                         │ │ │
│  │  │                                                                 │ │ │
│  │  │  🔧 Email Service                                             │ │ │
│  │  │     ├─ SMTP configuration                                    │ │ │
│  │  │     ├─ Template rendering                                    │ │ │
│  │  │     ├─ OTP sending                                           │ │ │
│  │  │     └─ Alert notifications                                   │ │ │
│  │  │                                                                 │ │ │
│  │  │  🔧 Settings Service                                          │ │ │
│  │  │     ├─ Version control                                       │ │ │
│  │  │     ├─ Rollback support                                      │ │ │
│  │  │     ├─ Change tracking                                       │ │ │
│  │  │     └─ Audit trails                                          │ │ │
│  │  │                                                                 │ │ │
│  │  └─────────────────────────────────────────────────────────────────┘ │ │
│  │                                                                        │ │
│  │  ┌─ ML MODELS ────────────────────────────────────────────────────┐ │ │
│  │  │ • LSTM Chat Summarizer (conversation titles)                  │ │ │
│  │  │ • Sentence Transformers (embeddings)                          │ │ │
│  │  │ • TensorFlow Models                                           │ │ │
│  │  │ • VADER Sentiment Analysis                                    │ │ │
│  │  └────────────────────────────────────────────────────────────────┘ │ │
│  │                                                                        │ │
│  └────────────────────────────────────────────────────────────────────────┘ │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
        │                   │                           │
        │                   │                           │
┌───────┴────┐      ┌───────┴────┐          ┌──────────┴────┐
│             │      │             │          │                │
│             │      │             │          │                │
▼             ▼      ▼             ▼          ▼                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                   DATA & EXTERNAL SERVICES LAYER                     │
│                                                                      │
│  ┌─ DATABASES ────────────────────────────────────────────────┐    │
│  │                                                             │    │
│  │  PostgreSQL 14+ (Port 5432)                               │    │
│  │  ├─ Primary: Transactions, users, auth                    │    │
│  │  ├─ Secondary: Assessments, conversations                 │    │
│  │  ├─ Tertiary: RBAC, settings, audit logs                  │    │
│  │  └─ 30+ tables (all with relationships & constraints)     │    │
│  │                                                             │    │
│  │  Qdrant Vector DB (Port 6333)                             │    │
│  │  ├─ Collections: neurowellca_conversations                │    │
│  │  ├─ Vectors: 384-dimensional (Sentence Transformers)     │    │
│  │  ├─ Metadata: user_id, sentiment, timestamp              │    │
│  │  └─ Operations: Store, search, similarity                │    │
│  │                                                             │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  ┌─ LLM PROVIDERS ────────────────────────────────────────────┐    │
│  │                                                             │    │
│  │  🤖 Ollama (Local, Port 11434)                            │    │
│  │     └─ Models: llama3.2:3b, etc.                          │    │
│  │                                                             │    │
│  │  🤖 OpenAI API                                            │    │
│  │     └─ Models: GPT-4, GPT-3.5-turbo                       │    │
│  │                                                             │    │
│  │  🤖 Anthropic Claude API                                  │    │
│  │     └─ Models: Claude 3 (Opus, Sonnet, Haiku)            │    │
│  │                                                             │    │
│  │  🤖 Azure OpenAI                                          │    │
│  │     └─ Enterprise deployment                              │    │
│  │                                                             │    │
│  │  🤖 Custom LLM Providers                                  │    │
│  │     └─ Any provider with REST API                         │    │
│  │                                                             │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  ┌─ EXTERNAL SERVICES ────────────────────────────────────────┐    │
│  │                                                             │    │
│  │  📱 Twilio (SMS & WhatsApp)                               │    │
│  │     ├─ WhatsApp API: Crisis alerts to guardians          │    │
│  │     ├─ SMS API: Fallback notifications                   │    │
│  │     └─ REST API: Webhook support                         │    │
│  │                                                             │    │
│  │  📧 SMTP Mail Server                                      │    │
│  │     ├─ Email alerts (crisis, reminders)                  │    │
│  │     ├─ OTP delivery                                       │    │
│  │     └─ Template-based emails                              │    │
│  │                                                             │    │
│  │  💾 Cloud Storage (Google Cloud Storage)                 │    │
│  │     ├─ Assets & static files                              │    │
│  │     └─ Backup & archival                                  │    │
│  │                                                             │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 🗄️ Database Entity Relationship Diagram (ERD)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         USER MANAGEMENT ENTITIES                              │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────┐                    ┌──────────────┐
    │    USERS     │◄──────many─────────┤ AUTH_IDENTITY│
    ├──────────────┤                    ├──────────────┤
    │ id (PK)      │                    │ id (PK)      │
    │ username *   │                    │ user_id (FK) │
    │ email *      │                    │ provider_id  │
    │ password_hash│                    │ provider_...  │
    │ full_name    │                    │ access_token │
    │ age          │                    │ refresh_token│
    │ guardian_*   │                    └──────────────┘
    │ has_completed│
    │ _assessment  │
    │ email_verified├─────────────────┐
    │ is_active    │                  │
    │ token_version│                  │
    │ created_at   │                  │
    │ updated_at   │                  │
    │ last_login   │                  │
    │ deleted_at   │                  │
    └──────────────┘                  │
           │                          │
           │◄─────many────────────────┘
           │
           │many
           │
    ┌──────▼──────────┐         ┌──────────────────┐
    │ USER_ROLES      │─many──→ │ ROLES            │
    ├─────────────────┤         ├──────────────────┤
    │ id (PK)         │         │ id (PK)          │
    │ user_id (FK)    │         │ code *           │
    │ role_id (FK)    │         │ name             │
    │ assigned_by     │         │ description      │
    │ assigned_at     │         │ is_system        │
    └─────────────────┘         │ is_active        │
                                │ created_at       │
    ┌───────────────┐           │ deleted_at       │
    │ USER_PERMISSION│          └────────┬─────────┘
    ├───────────────┤                    │many
    │ id (PK)       │                    │
    │ user_id (FK)  │           ┌────────▼──────────────┐
    │ permission_id │           │ ROLE_PERMISSION_SETS │
    │ assigned_by   │           ├───────────────────────┤
    │ assigned_at   │           │ id (PK)               │
    └───────────────┘           │ role_id (FK)          │
                                │ permission_set_id (FK)│
    ┌──────────────────┐        │ created_at            │
    │ USER_PERMISSION_ │        └───────────┬───────────┘
    │ SETS             │                    │many
    ├──────────────────┤           ┌────────▼──────────────┐
    │ id (PK)          │           │ PERMISSION_SETS      │
    │ user_id (FK)     │           ├───────────────────────┤
    │ permission_set   │           │ id (PK)               │
    │ _id (FK)         │           │ code *               │
    │ assigned_by      │           │ name                  │
    │ assigned_at      │           │ description           │
    └──────────────────┘           │ is_active            │
                                   │ created_at            │
                    ┌──────────────┤ deleted_at            │
                    │many          └─────┬──────────────────┘
                    │                    │many
           ┌────────▼─────────────────┐  │
           │ PERMISSION_SET_PERMISSIONS
           ├────────────────────────────┤
           │ id (PK)                    │
           │ permission_set_id (FK)─────┘
           │ permission_id (FK)─────┐
           │ created_at              │many
           └────────────────────────┘
                                │
                 ┌──────────────▼──────────┐
                 │ PERMISSIONS            │
                 ├────────────────────────┤
                 │ id (PK)                │
                 │ code * (e.g., chat.v...)
                 │ name                   │
                 │ category               │
                 │ description            │
                 │ is_active              │
                 │ created_at             │
                 │ deleted_at             │
                 └────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                    MENTAL HEALTH DATA ENTITIES                                │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │    USERS     │
    └──────┬───────┘
           │1
           │
    ┌──────▼──────────────┐         ┌──────────────────┐
    │ ASSESSMENTS         │         │ RISK_LEVEL       │
    ├─────────────────────┤         ├──────────────────┤
    │ id (PK)             │         │ LOW              │
    │ user_id (FK)        │         │ MILD             │
    │ phq9_score          │         │ MODERATE         │
    │ gad7_score          │         │ MODERATELY_SEVERE
    │ stress_level        │         │ SEVERE           │
    │ risk_level (ENUM)◄──┴────────◤ (Enum values)   │
    │ phq9_q1...q9        │         └──────────────────┘
    │ gad7_q1...q7        │
    │ notes               │
    │ created_at          │
    └─────────────────────┘

    ┌─────────────────┐
    │ CONVERSATIONS   │
    ├─────────────────┤
    │ id (PK)         │
    │ user_id (FK)───┬────→ USERS
    │ session_id      │
    │ message_text    │
    │ sender          │
    │ crisis_detected │
    │ sentiment_score │
    │ vector_id       │      (Reference to Qdrant)
    │ created_at      │
    └─────────────────┘

    ┌─────────────────┐
    │ CHAT_SESSIONS   │
    ├─────────────────┤
    │ id (PK)         │
    │ user_id (FK)───┬────→ USERS
    │ session_id *    │
    │ title           │      (LSTM-generated)
    │ summary         │      (LSTM-generated)
    │ message_count   │
    │ started_at      │
    │ last_message_at │
    └─────────────────┘

    ┌──────────────┐
    │ CRISIS_LOGS  │
    ├──────────────┤
    │ id (PK)      │
    │ user_id (FK)─┬──→ USERS
    │ message_text │
    │ crisis_score │
    │ keywords_..  │
    │ action_taken │
    │ resolved     │
    │ created_at   │
    └──────┬───────┘
           │1
           │
    ┌──────▼─────────────┐
    │ GUARDIAN_ALERTS    │
    ├────────────────────┤
    │ id (PK)            │
    │ user_id (FK)       │
    │ crisis_log_id (FK) │
    │ guardian_contact   │
    │ alert_sent         │
    │ alert_method       │
    │ message_sent       │
    │ response_received  │
    │ created_at         │
    └────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                    GAMIFICATION & PROGRESS ENTITIES                           │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │    USERS     │
    └──────┬───────┘
           │1
           │
    ┌──────▼──────────────┐
    │ GAMES_PROGRESS      │
    ├─────────────────────┤
    │ id (PK)             │
    │ user_id (FK) *      │(1:1 relationship)
    │ total_points        │
    │ level               │
    │ conversations_count │
    │ assessments_completed
    │ streak_days         │
    │ badges (JSON)       │
    │ last_activity       │
    │ created_at          │
    └─────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                    FEEDBACK & LEARNING ENTITIES                               │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────────┐
    │ CONVERSATIONS    │
    └────┬─────────────┘
         │1
         │
    ┌────▼──────────────┐         ┌──────────────────┐
    │ MESSAGE_FEEDBACK  │         │ FEEDBACK_STATUS  │
    ├───────────────────┤         ├──────────────────┤
    │ id (PK)           │         │ PENDING          │
    │ conversation_id FK├────────→│ APPROVED         │
    │ user_id (FK)      │         │ REJECTED         │
    │ feedback_type     │◄────────┤ DISMISSED        │
    │ reason            │         └──────────────────┘
    │ status (ENUM)     │
    │ reviewed_by (FK)  │
    │ reviewed_at       │
    │ review_notes      │
    │ created_at        │
    │ deleted_at        │
    └────┬──────────────┘
         │1
         │
    ┌────▼──────────────┐
    │ GOLDEN_EXAMPLES   │
    ├───────────────────┤
    │ id (PK)           │
    │ prompt            │
    │ response          │
    │ category          │
    │ tags (JSON)       │
    │ source_feedback_id├───→ MESSAGE_FEEDBACK
    │ qdrant_point_id   │    (Reference to Qdrant)
    │ is_active         │
    │ priority          │
    │ created_by (FK)   │
    │ created_at        │
    │ updated_at        │
    │ deleted_at        │
    └───────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                    CONFIGURATION ENTITIES                                     │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────────┐
    │ LLM_PROVIDERS    │
    ├──────────────────┤
    │ id (PK)          │
    │ name *           │
    │ provider_type    │
    │ base_url         │
    │ api_key_encrypted
    │ config (JSON)    │
    │ models (JSON)    │
    │ is_active        │
    │ is_default       │
    │ last_health_check
    │ health_status    │
    │ health_message   │
    │ created_at       │
    │ updated_at       │
    │ deleted_at       │
    └──────────────────┘

    ┌──────────────────┐
    │ SETTINGS         │
    ├──────────────────┤
    │ id (PK)          │
    │ version *        │
    │ ai_model         │
    │ temperature      │
    │ max_tokens       │
    │ top_p            │
    │ deny_words (JSON)
    │ provider_id (FK) │
    │ auth_enabled     │
    │ oauth_*_enabled  │
    │ feedback_*       │
    │ crisis_threshold │
    │ change_type      │
    │ source_version_id
    │ target_version_id
    │ change_reason    │
    │ is_active        │
    │ created_by (FK)  │
    │ created_at       │
    │ activated_at     │
    └──────────────────┘

    ┌──────────────────────┐
    │ RBAC_AUDIT_LOGS      │
    ├──────────────────────┤
    │ id (PK)              │
    │ entity_type          │
    │ entity_id            │
    │ action               │
    │ old_value (JSON)     │
    │ new_value (JSON)     │
    │ changed_by (FK)      │
    │ changed_at           │
    │ ip_address           │
    │ user_agent           │
    └──────────────────────┘
```

---

## 📊 Data Flow Diagrams

### Crisis Detection & Alert Flow

```
┌────────────────────────────────────────────────────────────────────────────┐
│ User sends message: "I want to end my life"                                │
└────────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌────────────────────────────────────────────────────────────────────────────┐
│ Crisis Detection Pipeline                                                   │
│                                                                             │
│  Step 1: Text Analysis                                                     │
│  ├─ Unsafe pattern matching: ✓ MATCH ("end my life" = suicide)           │
│  ├─ VADER Sentiment: -0.95 (extremely negative)                          │
│  └─ Decision: UNSAFE_TEXT + VERY_NEGATIVE SENTIMENT = HIGH CRISIS         │
│                                                                             │
│  Step 2: ML Crisis Scoring                                                │
│  ├─ Input: [unsafe_text, sentiment, keywords, user_history]              │
│  ├─ LSTM/ML Model: 92/100 crisis score                                    │
│  └─ Decision: SCORE ≥ 70 → CRISIS DETECTED ✓                            │
│                                                                             │
│  Step 3: Risk Level Calculation                                           │
│  ├─ Latest Assessment: PHQ-9=22, GAD-7=18, Stress=9                      │
│  ├─ Logic: Any ≥ threshold AND crisis_detected = SEVERE RISK             │
│  └─ Result: RISK_LEVEL = SEVERE                                          │
│                                                                             │
│  Step 4: Store Crisis Log                                                 │
│  ├─ DB Insert: crisis_logs table                                         │
│  │   ├─ user_id: 42                                                      │
│  │   ├─ message_text: "I want to end my life"                           │
│  │   ├─ crisis_score: 92                                                │
│  │   ├─ keywords_detected: ["suicide", "end_my_life"]                   │
│  │   └─ action_taken: "guardian_alerted"                                │
│  └─ Result: crisis_log_id = 5678                                         │
│                                                                             │
└────────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌────────────────────────────────────────────────────────────────────────────┐
│ Guardian Alert Pipeline                                                     │
│                                                                             │
│  Step 1: Check Cooldown                                                    │
│  ├─ Query: Last alert for user_id=42 in last 24 hours                    │
│  ├─ Result: Last alert was 3 days ago                                    │
│  └─ Decision: OK to send new alert (cooldown expired)                    │
│                                                                             │
│  Step 2: Fetch Guardian Info                                              │
│  ├─ Query users table:                                                    │
│  │   ├─ guardian_contact: "+1-555-0123"                                 │
│  │   └─ guardian_email: "parent@example.com"                            │
│  └─ Preferred channels: [WhatsApp, Email]                               │
│                                                                             │
│  Step 3: Compose Alert Message                                            │
│  ├─ Template: "Mental Health Alert"                                      │
│  ├─ Content:                                                              │
│  │   "Your child expressed suicidal ideation in NeurowellCA.             │
│  │    User message: 'I want to end my life'                             │
│  │    Time: 2026-04-29 14:32:00 UTC                                    │
│  │    Risk Level: SEVERE                                                │
│  │    Please reach out to them immediately.                             │
│  │    Crisis Resources: [links...]"                                     │
│  └─ Message compiled                                                     │
│                                                                             │
│  Step 4: Send via Twilio WhatsApp                                         │
│  ├─ API Call: POST /Messages                                            │
│  │   ├─ From: whatsapp:+14155238886                                    │
│  │   ├─ To: whatsapp:+1-555-0123                                       │
│  │   └─ Body: [Alert message]                                          │
│  ├─ Response: 201 Created (SID: SM123abc...)                           │
│  └─ Update guardian_alerts:                                             │
│      ├─ alert_sent: true                                               │
│      ├─ alert_method: "whatsapp"                                       │
│      └─ message_sent: "[full message]"                                 │
│                                                                             │
│  Step 5: Fallback Email (if WhatsApp fails)                              │
│  ├─ API Call: SMTP SEND                                                 │
│  │   ├─ To: parent@example.com                                         │
│  │   ├─ Subject: "Mental Health Alert from NeurowellCA"                │
│  │   └─ Body: [Alert message + HTML template]                         │
│  ├─ Response: 250 OK                                                    │
│  └─ Log sent alert                                                       │
│                                                                             │
│  Step 6: Store Guardian Alert                                             │
│  ├─ DB Insert: guardian_alerts table                                    │
│  │   ├─ user_id: 42                                                   │
│  │   ├─ crisis_log_id: 5678                                           │
│  │   ├─ guardian_contact: "+1-555-0123"                               │
│  │   ├─ alert_sent: true                                              │
│  │   ├─ alert_method: "whatsapp"                                      │
│  │   └─ message_sent: "[Alert content]"                              │
│  └─ Result: guardian_alert_id = 9876                                  │
│                                                                             │
└────────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌────────────────────────────────────────────────────────────────────────────┐
│ User Response Pipeline                                                      │
│                                                                             │
│  1. Return to User (Frontend)                                             │
│     ├─ is_crisis: true                                                   │
│     ├─ resources: [Crisis Hotline numbers, therapist contacts, etc.]     │
│     ├─ message: "We've alerted your emergency contact..."               │
│     └─ suggested_game: null (due to crisis status)                      │
│                                                                             │
│  2. Dashboard Alert                                                        │
│     ├─ Flag: HIGH_RISK_ALERT                                            │
│     └─ Recommend: Immediate professional help                           │
│                                                                             │
│  3. Admin Notification                                                     │
│     ├─ System logs: Crisis detected & alert sent                        │
│     ├─ Admin dashboard: Shows pending crisis cases                      │
│     └─ Follow-up: Required within 24 hours                              │
│                                                                             │
└────────────────────────────────────────────────────────────────────────────┘
```

### Vector Search & RAG Flow

```
┌──────────────────────────────────────────────────────────────────────────┐
│ User: "I'm having anxiety about my presentation tomorrow"                │
└──────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ Step 1: Generate Embedding                                               │
│                                                                          │
│  Model: sentence-transformers/all-MiniLM-L6-v2                         │
│  Input text: "I'm having anxiety about my presentation tomorrow"       │
│                                                                          │
│  Processing:
│  ├─ Tokenize: [I'm, having, anxiety, ...]                             │
│  ├─ Embed: 384-dimensional vector                                      │
│  └─ Output: [0.234, -0.156, 0.892, ..., 0.123]  (384 dims)           │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ Step 2: Search Qdrant Vector DB                                         │
│                                                                          │
│  Query: Qdrant search_points()                                         │
│  ├─ collection_name: "neurowellca_conversations"                       │
│  ├─ query_vector: [0.234, -0.156, 0.892, ...]                        │
│  ├─ limit: 5                                                           │
│  ├─ score_threshold: 0.7                                              │
│  └─ filters: {user_id: 42}  (personalized results)                    │
│                                                                          │
│  Similar conversations found:                                          │
│  ├─ ID 1: "I'm nervous about the job interview"    (score: 0.89)     │
│  ├─ ID 2: "Performance anxiety is getting worse"   (score: 0.87)     │
│  ├─ ID 3: "Presentation tips to reduce anxiety"    (score: 0.84)     │
│  ├─ ID 4: "I practice deep breathing before talks" (score: 0.81)     │
│  └─ ID 5: "Social anxiety: how to manage"          (score: 0.78)     │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ Step 3: Retrieve Full Context (Golden Examples)                         │
│                                                                          │
│  For each search result:                                               │
│  ├─ Query conversations table                                          │
│  │   SELECT message_text, sender, sentiment_score                     │
│  │   WHERE vector_id IN (ID1, ID2, ID3, ID4, ID5)                    │
│  │                                                                     │
│  ├─ Also query golden_examples for top matches                        │
│  │   SELECT prompt, response, category                                │
│  │   WHERE category IN ('anxiety', 'presentation_fear')              │
│  │   ORDER BY priority DESC LIMIT 3                                  │
│  │                                                                     │
│  │   Golden examples retrieved:                                       │
│  │   ├─ Prompt: "I'm nervous about public speaking"                 │
│  │   │  Response: "Public speaking anxiety is very common. Here...  │
│  │   │   try grounding techniques like 5-4-3-2-1 sense check..."  │
│  │   │  Category: anxiety | Priority: 10                           │
│  │   │                                                              │
│  │   ├─ Prompt: "Performance anxiety before events"                │
│  │   │  Response: "Performance anxiety comes from caring about     │
│  │   │   the outcome. Reframe: focus on doing your best, not...  │
│  │   │  Category: anxiety | Priority: 9                           │
│  │   │                                                              │
│  │   └─ Prompt: "How to calm nerves before presentations"          │
│  │      Response: "Try these proven techniques: 1) Box breathing  │
│  │       2) Positive self-talk 3) Visualization 4) Practice..."   │
│  │      Category: performance | Priority: 8                       │
│  │                                                                  │
│  └─ All examples compiled into context                              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ Step 4: Build Enhanced Prompt (RAG)                                     │
│                                                                          │
│  SYSTEM PROMPT:                                                        │
│  You are an empathetic mental health AI assistant. Provide support    │
│  based on user context and proven techniques.                        │
│                                                                          │
│  RELEVANT CONTEXT (RAG):                                              │
│  Related conversations from this user's history:                     │
│  - "I'm nervous about the job interview" → Handled with breathing   │
│  - "Performance anxiety is getting worse" → Addressed with reframing│
│                                                                          │
│  Proven responses from experts (Golden Examples):                    │
│  1. For performance anxiety:                                          │
│     "Performance anxiety comes from caring about the outcome...     │
│      Reframe: focus on doing your best, not perfection."           │
│                                                                          │
│  2. For public speaking:                                             │
│     "Try grounding techniques: 5-4-3-2-1 sense check..."           │
│                                                                          │
│  USER MESSAGE:                                                         │
│  "I'm having anxiety about my presentation tomorrow"                │
│                                                                          │
│  INSTRUCTIONS:                                                         │
│  - Acknowledge their feelings                                         │
│  - Draw from relevant context above                                  │
│  - Suggest practical techniques                                       │
│  - Be warm and supportive                                            │
│  - Max 300 tokens                                                     │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ Step 5: Call LLM Provider                                               │
│                                                                          │
│  LLM: Ollama (selected as healthy & default)                          │
│                                                                          │
│  Request:                                                               │
│  POST /api/generate                                                   │
│  {                                                                     │
│    "model": "llama3.2:3b",                                           │
│    "prompt": "[ENHANCED PROMPT FROM STEP 4]",                        │
│    "temperature": 0.7,                                               │
│    "top_p": 0.9,                                                     │
│    "num_predict": 300,                                               │
│    "stream": true                                                    │
│  }                                                                     │
│                                                                          │
│  Response (streamed chunks):                                          │
│  ├─ Chunk 1: "I understand that anxiety about presentations"        │
│  ├─ Chunk 2: " is really common and valid. Many people feel"        │
│  ├─ Chunk 3: " nervous before speaking in front of others."         │
│  └─ ... [more chunks] ...                                           │
│      Final chunk: "[end of response]"                               │
│                                                                          │
│  Full response: "I understand that anxiety about presentations is   │
│  really common and valid. Many people feel nervous before speaking  │
│  in front of others. Here are some techniques that can help:        │
│                                                                          │
│  1. **Breathing**: Try 4-7-8 breathing (inhale 4, hold 7, exhale 8)
│                                                                          │
│  2. **Reframe**: Instead of 'I might fail', think 'I'll do my best'
│                                                                          │
│  3. **Practice**: Rehearse your presentation out loud                │
│                                                                          │
│  4. **Grounding**: Use the 5-4-3-2-1 sense technique beforehand    │
│                                                                          │
│  You've got this! Remember, presentations are about sharing ideas,  │
│  not about being perfect."                                           │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ Step 6: Generate Title & Store Results                                  │
│                                                                          │
│  LSTM Title Generation:                                                │
│  Input conversation: [user_msg, ai_response]                         │
│  Output: "Managing Presentation Anxiety"                             │
│                                                                          │
│  Store in PostgreSQL:                                                 │
│  ├─ conversations: user_msg, ai_response, vector_id                 │
│  ├─ chat_sessions: session_id, title, summary                       │
│  └─ Update metadata                                                   │
│                                                                          │
│  Store in Qdrant:                                                     │
│  ├─ Embedding: [0.456, 0.234, -0.123, ...]                         │
│  ├─ Metadata: {                                                       │
│  │   user_id: 42,                                                    │
│  │   timestamp: 2026-04-29T14:32:00Z,                              │
│  │   sentiment: 0.45,                                               │
│  │   is_crisis: false                                               │
│  │ }                                                                  │
│  └─ Point ID: Qdrant-assigned UUID                                 │
│                                                                          │
│  Update LLM Provider Stats:                                           │
│  ├─ total_requests += 1                                             │
│  ├─ total_tokens += 487  (350 input + 137 output)                  │
│  └─ last_used_at = now()                                            │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ Step 7: Return to User                                                  │
│                                                                          │
│ Response to Frontend:                                                   │
│ {                                                                      │
│   "message_id": 54321,                                               │
│   "response": "I understand that anxiety about presentations...",   │
│   "is_crisis": false,                                               │
│   "suggested_game": {                                               │
│     "title": "Breathe & Balance",                                   │
│     "href": "/games/breathe-balance",                              │
│     "emoji": "🌬️",                                                  │
│     "reason": "A short breathing reset can help settle...         │
│   },                                                                 │
│   "resources": [                                                     │
│     {                                                                │
│       "title": "Anxiety Management Techniques",                      │
│       "url": "https://mentalhealthresources.org/..."               │
│     }                                                                │
│   ]                                                                  │
│ }                                                                    │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 🔄 RBAC Permission Resolution Flow

```
┌─────────────────────────────────────────────────────────────┐
│ User (ID: 42) Makes Request                                 │
│ GET /api/admin/stats                                        │
│ Authorization: Bearer eyJhbGc...                           │
└─────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────┐
│ Middleware: Extract & Verify JWT Token                      │
│ ├─ Decode token                                            │
│ ├─ Verify signature (HS256 + JWT_SECRET_KEY)              │
│ ├─ Check expiration (valid until: 2026-04-29T15:32:00Z)   │
│ ├─ Check blacklist (revoked_tokens table)                 │
│ └─ Result: user_id = 42 ✓                                  │
└─────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────┐
│ Permission Service: Resolve Permissions                     │
│                                                             │
│ Required Permission: "admin.view_stats"                    │
│                                                             │
│ 1. Load User (user_id=42)                                  │
│    Query: SELECT * FROM users WHERE id=42                 │
│    Result: User loaded ✓                                  │
│                                                             │
│ 2. Get User Roles                                          │
│    Query: SELECT role_id FROM user_roles                  │
│            WHERE user_id=42                               │
│    Result: [role_id: 3, role_id: 5]                      │
│           └─ Role 3: Admin                                │
│           └─ Role 5: Crisis Responder                     │
│                                                             │
│ 3. Get Role Permission Sets                                │
│    Query: SELECT permission_set_id                        │
│            FROM role_permission_sets                      │
│            WHERE role_id IN (3, 5)                        │
│    Result: [perm_set_id: 1, perm_set_id: 2, ...]        │
│           └─ PermSet 1: AdminAll                          │
│           └─ PermSet 2: CrisisResponderRead               │
│                                                             │
│ 4. Get Permissions from Permission Sets                    │
│    Query: SELECT p.code                                   │
│            FROM permissions p                             │
│            JOIN permission_set_permissions psp ON...      │
│            WHERE psp.permission_set_id IN (1, 2, ...)    │
│    Result: [                                              │
│      'admin.create_user',                                 │
│      'admin.delete_user',                                 │
│      'admin.view_stats',  ← MATCH! ✓                      │
│      'admin.view_conversations',                          │
│      'crisis.view_logs',                                  │
│      ...                                                  │
│    ]                                                      │
│                                                             │
│ 5. Check Direct User Permissions                           │
│    Query: SELECT p.code FROM permissions p                │
│            JOIN user_permissions up ON...                 │
│            WHERE up.user_id=42                            │
│    Result: (none, user has role-based permissions only)  │
│                                                             │
│ 6. Check Direct User Permission Sets                       │
│    Query: SELECT p.code FROM permissions p                │
│            JOIN permission_set_permissions psp ON...      │
│            JOIN user_permission_sets ups ON...            │
│            WHERE ups.user_id=42                           │
│    Result: (none)                                         │
│                                                             │
│ 7. Final Decision                                          │
│    ├─ Required: "admin.view_stats"                        │
│    ├─ Available: "admin.view_stats" ∈ resolved_permissions
│    ├─ Status: AUTHORIZED ✓                               │
│    └─ Reason: Role 3 (Admin) → AdminAll → contains      │
│              'admin.view_stats'                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────┐
│ Audit Logging                                               │
│                                                             │
│ Insert into rbac_audit_logs:                              │
│ ├─ entity_type: 'endpoint_access'                         │
│ ├─ entity_id: 42                                          │
│ ├─ action: 'accessed'                                     │
│ ├─ old_value: null                                        │
│ ├─ new_value: {                                           │
│ │   endpoint: '/api/admin/stats',                        │
│ │   permission: 'admin.view_stats',                      │
│ │   authorized: true                                     │
│ │ }                                                       │
│ ├─ changed_by: 42                                         │
│ ├─ changed_at: 2026-04-29T14:32:00Z                      │
│ ├─ ip_address: '192.168.1.100'                          │
│ └─ user_agent: 'Mozilla/5.0...'                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────┐
│ Execute Endpoint                                            │
│                                                             │
│ GET /api/admin/stats                                      │
│                                                             │
│ Queries:                                                   │
│ ├─ SELECT COUNT(*) FROM users                            │
│ ├─ SELECT COUNT(*) FROM conversations                    │
│ ├─ SELECT COUNT(*) FROM assessments                      │
│ ├─ SELECT COUNT(*) FROM crisis_logs                      │
│ └─ SELECT COUNT(*) FROM chat_sessions                    │
│                                                             │
│ Response: {                                                │
│   "total_users": 1247,                                    │
│   "total_conversations": 45382,                           │
│   "total_assessments": 3891,                              │
│   "total_crisis_logs": 284,                               │
│   "total_sessions": 12471                                 │
│ }                                                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────┐
│ Response Returned (200 OK)                                  │
│                                                             │
│ Headers:                                                    │
│ ├─ Content-Type: application/json                         │
│ ├─ X-RateLimit-Remaining: 4999                            │
│ └─ X-Request-ID: req-12345abc                            │
│                                                             │
│ Body: [stats response above]                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 📊 System Interaction Matrix

```
┌──────────────────────┬──────────────────────────────────────────────────────┐
│ Component            │ Interactions                                          │
├──────────────────────┼──────────────────────────────────────────────────────┤
│                      │                                                      │
│ Frontend (Next.js)   │ → REST API calls to Backend                          │
│                      │ → Stores JWT tokens in localStorage                 │
│                      │ → Receives SSE streams for real-time chat          │
│                      │                                                      │
│ Backend (FastAPI)    │ ← HTTP requests from Frontend                       │
│                      │ → Database queries to PostgreSQL                   │
│                      │ → Vector operations to Qdrant                       │
│                      │ → API calls to LLM providers (Ollama, OpenAI, etc.)│
│                      │ → SMS/WhatsApp via Twilio                          │
│                      │ → Email via SMTP                                   │
│                      │                                                      │
│ PostgreSQL           │ ← Queries from Backend                             │
│                      │ → Transactions with ACID guarantees                │
│                      │ → Stores all relational data (users, messages)    │
│                      │                                                      │
│ Qdrant Vector DB     │ ← Vector operations from Backend                   │
│                      │ → Stores embeddings & metadata                     │
│                      │ → Returns similarity search results                │
│                      │                                                      │
│ LLM Providers        │ ← Chat requests from Backend                       │
│ (Ollama, OpenAI)     │ → AI-generated responses                           │
│                      │ → Token usage data                                 │
│                      │                                                      │
│ Sentence Transformers│ ← Text from Backend for embedding                 │
│ (ML Model)           │ → 384-dimensional vectors                          │
│                      │                                                      │
│ LSTM Summarizer      │ ← Chat messages from Backend                       │
│ (ML Model)           │ → Conversation titles & summaries                  │
│                      │                                                      │
│ Twilio API           │ ← Alert messages from Backend                      │
│                      │ → SMS/WhatsApp delivery confirmations              │
│                      │ → Webhook callbacks (optional)                     │
│                      │                                                      │
│ SMTP Server          │ ← Email from Backend                               │
│                      │ → Email delivery confirmations                     │
│                      │                                                      │
│ Google Cloud         │ ← Container deployments                            │
│ (Deployment)         │ → Managed services (Cloud Run, SQL)               │
│                      │                                                      │
└──────────────────────┴──────────────────────────────────────────────────────┘
```

---

## 🔄 Deployment Pipeline

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. Development                                                   │
│ ├─ Docker Compose (docker-compose.dev.yml)                     │
│ ├─ Services: postgres, qdrant, ollama, backend, frontend      │
│ ├─ Hot reload enabled                                          │
│ └─ Debug logging on                                            │
└─────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────────┐
│ 2. Testing                                                       │
│ ├─ Unit tests (API endpoints, services)                        │
│ ├─ Integration tests (with test DB)                            │
│ ├─ Linting (pylint, black, mypy)                              │
│ └─ TypeScript checks (frontend)                                │
└─────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────────┐
│ 3. Build                                                         │
│ ├─ Backend: Docker build (backend/Dockerfile)                  │
│ │   └─ Base: python:3.11-slim                                  │
│ │   └─ Installs: requirements.txt                              │
│ │   └─ Output: neurowellca-backend:latest                      │
│ │                                                               │
│ ├─ Frontend: Docker build (frontend/Dockerfile)                │
│ │   └─ Base: node:20-alpine                                   │
│ │   └─ Build: npm run build                                    │
│ │   └─ Output: neurowellca-frontend:latest                     │
│ │                                                               │
│ └─ Push to Container Registry (Google Artifact Registry)       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────────┐
│ 4. Deploy to Google Cloud (Production)                          │
│                                                                 │
│ Backend Deployment:                                            │
│ ├─ Service: Cloud Run (serverless)                             │
│ ├─ CPU: 2x                                                     │
│ ├─ Memory: 4GB                                                 │
│ ├─ Port: 8000                                                  │
│ ├─ Env vars: DATABASE_URL, JWT_SECRET, QDRANT_URL, etc.      │
│ └─ Auto-scaling: 0-100 instances                              │
│                                                                 │
│ Frontend Deployment:                                           │
│ ├─ Service: Cloud Run (or Cloud Storage + CDN)                │
│ ├─ CPU: 1x                                                     │
│ ├─ Memory: 1GB                                                 │
│ ├─ Port: 3000                                                  │
│ └─ Auto-scaling: 0-50 instances                               │
│                                                                 │
│ Database:                                                       │
│ ├─ Service: Cloud SQL for PostgreSQL                          │
│ ├─ Instance: 4 vCPU, 16 GB RAM                                │
│ ├─ High Availability: Regional                                │
│ ├─ Backups: Daily automatic                                   │
│ └─ Replication: Read replicas (optional)                      │
│                                                                 │
│ Vector DB:                                                      │
│ ├─ Service: Cloud Run or Compute Engine                       │
│ ├─ Qdrant: Standalone deployment                              │
│ └─ Storage: Persistent volume or GCS                          │
│                                                                 │
│ Load Balancing:                                                │
│ ├─ Service: Cloud Load Balancing                              │
│ ├─ Protocol: HTTPS (TLS 1.3)                                  │
│ ├─ Health checks: /health endpoint                            │
│ └─ Geographic distribution (optional)                         │
│                                                                 │
│ Monitoring:                                                     │
│ ├─ Cloud Monitoring: Metrics & logs                           │
│ ├─ Cloud Logging: Centralized logs                            │
│ ├─ Error Reporting: Stack traces                              │
│ └─ Uptime checks: External monitoring                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

**Last Updated**: April 29, 2026
**Version**: 2.0.0
**Architecture**: Microservices-friendly (can be extended to true microservices with event-driven messaging)
