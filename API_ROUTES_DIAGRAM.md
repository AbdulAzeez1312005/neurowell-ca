# NEUROWELLCA - API Routes & Terminal Diagrams

## 📊 Complete API Route Hierarchy (Tree View)

```
🌍 NeurowellCA API Server
│
├── 📍 Root Level
│   ├── GET   /                          → Welcome message & links
│   └── GET   /health                    → Health check
│
├── 🔐 /api/auth (Authentication & Profile)
│   ├── POST   /login                    → Authenticate user
│   ├── POST   /register                 → Create new account
│   ├── POST   /start-signup-otp         → Initiate OTP verification
│   ├── POST   /verify-signup-otp        → Complete signup with OTP
│   ├── POST   /refresh-token            → Get new access token
│   ├── POST   /logout                   → Revoke current token
│   ├── POST   /forgot-password          → Request password reset
│   ├── GET    /current-user             → Get authenticated user
│   ├── PUT    /profile                  → Update user profile
│   │   └── Fields: full_name, age, guardian_contact, guardian_email
│   ├── GET    /verify-email/{token}     → Verify email address
│   └── POST   /change-password          → Change password
│
├── 💬 /api/chat (Chat & Conversations)
│   ├── POST   /message                  → Send message (with AI response)
│   │   └── Body: {
│   │       message: string,
│   │       mood?: string,
│   │       mood_note?: string,
│   │       session_id?: string
│   │     }
│   │
│   ├── GET    /conversations            → List all conversations
│   ├── GET    /conversations/{id}       → Get single conversation
│   ├── DELETE /conversations/{id}       → Delete conversation
│   ├── POST   /reset-context            → Clear session context
│   │
│   ├── GET    /sessions                 → List chat sessions
│   ├── GET    /sessions/{id}            → Get session details
│   │   └── Returns: session_id, title (LSTM-generated), summary
│   │
│   ├── GET    /suggested-games          → Get game recommendations
│   │   └── Returns: Based on mood, sentiment, crisis status
│   │
│   └── POST   /stream-message           → Streaming AI response
│       └── Server-Sent Events (SSE)
│
├── 📋 /api/assessments (Mental Health Assessments)
│   ├── POST   /submit                   → Submit PHQ-9 & GAD-7
│   │   └── Body: {
│   │       phq9_answers: [int×9],      (0-3 each)
│   │       gad7_answers: [int×7],      (0-3 each)
│   │       stress_level: int           (0-10)
│   │     }
│   │
│   ├── GET    /latest                   → Get most recent assessment
│   ├── GET    /history                  → Get all assessments
│   ├── GET    /trends                   → Get trend analysis
│   ├── GET    /phq9-questions          → Fetch PHQ-9 questions
│   └── GET    /gad7-questions          → Fetch GAD-7 questions
│
├── 📊 /api/dashboard (User Analytics)
│   ├── GET    /stats                    → Dashboard statistics
│   │   └── Returns: {
│   │       total_conversations: int,
│   │       total_assessments: int,
│   │       current_risk_level: string,
│   │       recent_activity_days: int,
│   │       crisis_alerts: int
│   │     }
│   │
│   ├── GET    /trends                   → Assessment trends
│   ├── GET    /activity                 → Activity timeline
│   ├── GET    /risk-summary             → Current risk level
│   └── GET    /progress                 → Gamification progress
│
├── 👨‍💼 /api/admin (Admin Management)
│   ├── GET    /stats                    → Database statistics
│   │   └── Returns: {
│   │       total_users: int,
│   │       total_conversations: int,
│   │       total_assessments: int,
│   │       total_crisis_logs: int,
│   │       total_sessions: int
│   │     }
│   │
│   ├── GET    /users                    → List all users
│   ├── GET    /users/{id}              → Get user details
│   ├── DELETE /users/{id}              → Delete user
│   │
│   ├── GET    /conversations           → All conversations
│   ├── GET    /assessments              → All assessments
│   ├── GET    /crisis-logs              → All crisis events
│   │
│   └── POST   /purge-old-data          → Delete records older than X days
│       └── Query: days=30
│
├── 🔑 /api/rbac (Role-Based Access Control)
│   │
│   ├─── Permissions Management
│   │   ├── GET    /permissions          → List all permissions
│   │   ├── POST   /permissions          → Create permission
│   │   │   └── Body: {code, name, category, description}
│   │   ├── GET    /permissions/{id}     → Get permission
│   │   ├── PUT    /permissions/{id}     → Update permission
│   │   └── DELETE /permissions/{id}     → Delete permission
│   │
│   ├─── Permission Sets Management
│   │   ├── GET    /permission-sets      → List sets
│   │   ├── POST   /permission-sets      → Create set
│   │   │   └── Body: {code, name, description, permission_ids}
│   │   ├── GET    /permission-sets/{id} → Get set details
│   │   ├── PUT    /permission-sets/{id} → Update set
│   │   └── DELETE /permission-sets/{id} → Delete set
│   │
│   ├─── Roles Management
│   │   ├── GET    /roles                → List roles
│   │   ├── POST   /roles                → Create role
│   │   │   └── Body: {code, name, description, permission_set_ids}
│   │   ├── GET    /roles/{id}           → Get role details
│   │   ├── PUT    /roles/{id}           → Update role
│   │   └── DELETE /roles/{id}           → Delete role
│   │
│   └─── User RBAC Assignment
│       ├── POST   /users/{id}/assign-role       → Assign role
│       ├── POST   /users/{id}/revoke-role       → Remove role
│       ├── POST   /users/{id}/assign-permission → Direct permission
│       ├── POST   /users/{id}/revoke-permission → Remove direct permission
│       ├── GET    /users/{id}/permissions       → Get user permissions
│       └── GET    /users/{id}/roles             → Get user roles
│
├── 💬 /api/feedback (Message Feedback & Learning)
│   ├── POST   /submit                  → Submit feedback
│   │   └── Body: {
│   │       conversation_id: int,
│   │       feedback_type: "positive|negative",
│   │       reason?: string
│   │     }
│   │
│   ├── GET    /list                     → List user feedbacks
│   ├── PUT    /{id}                     → Update feedback status
│   │   └── Body: {
│   │       status: "pending|approved|rejected|dismissed",
│   │       review_notes?: string
│   │     }
│   │
│   ├─── Golden Examples (Admin)
│   │   ├── GET    /golden-examples      → List golden examples
│   │   ├── POST   /golden-examples      → Create example
│   │   │   └── Body: {
│   │   │       prompt: string,
│   │   │       response: string,
│   │   │       category: string,
│   │   │       tags?: string[],
│   │   │       priority?: int
│   │   │     }
│   │   ├── PUT    /golden-examples/{id} → Update example
│   │   └── DELETE /golden-examples/{id} → Delete example
│   │
│   └── GET    /admin/pending           → List pending reviews (admin)
│
├── 🤖 /api/llm-providers (LLM Provider Configuration)
│   ├── GET    /                         → List all providers
│   │   └── Returns: [
│   │       {
│   │           id, name, provider_type, model_name,
│   │           is_active, is_default, last_used_at,
│   │           total_requests, total_tokens
│   │       }
│   │     ]
│   │
│   ├── POST   /                         → Create provider
│   │   └── Body: {
│   │       name: string,
│   │       provider_type: "openai|anthropic|azure|local|custom",
│   │       api_key?: string,
│   │       api_endpoint?: string,
│   │       model_name: string,
│   │       parameters?: object,
│   │       is_default?: bool
│   │     }
│   │
│   ├── GET    /{id}                     → Get provider details
│   ├── PUT    /{id}                     → Update provider
│   ├── DELETE /{id}                     → Delete provider
│   │
│   ├── POST   /test-connection         → Test LLM connection
│   │   └── Query: provider_id=1
│   │
│   ├── GET    /available-models        → List available models
│   │   └── Query: provider_id=1
│   │
│   └── POST   /switch-default          → Set default provider
│       └── Body: {provider_id: int}
│
├── ⚙️ /api/settings (System Settings Management)
│   ├── GET    /                         → Get current active settings
│   │   └── Returns: {
│   │       version, ai_model, temperature, max_tokens,
│   │       crisis_threshold, feedback_auto_approve_*,
│   │       oauth_*_enabled, ...
│   │     }
│   │
│   ├── POST   /                         → Create new settings version
│   │   └── Body: {
│   │       ai_model?, temperature?, max_tokens?,
│   │       crisis_threshold?, [all settings fields],
│   │       change_reason?: string
│   │     }
│   │
│   ├── PUT    /{id}                     → Update settings
│   ├── POST   /{id}/activate            → Activate specific version
│   ├── POST   /{id}/rollback            → Rollback to previous version
│   │
│   ├── GET    /history                  → Get all settings versions
│   ├── GET    /{id}                     → Get specific version
│   │
│   └── GET    /audit-log                → Settings change audit trail
│
└── 🎮 /games (Gamification Routes - Implied)
    ├── GET    /progress                 → User game progress
    ├── POST   /play/{game_id}          → Record game play
    ├── GET    /achievements             → User achievements
    └── GET    /leaderboard             → Global leaderboard
```

---

## 🔄 Key API Call Flows (Sequence Diagrams)

### Flow 1: User Registration & Initial Assessment

```
Client                              Backend                           Database
  │                                   │                                  │
  ├──── POST /api/auth/register ────→ │                                  │
  │         {username, email,          │ Generate OTP                     │
  │          password, age, ...}       │ Send email                       │
  │                                   │                                  │
  │ ← OTP_START_RESPONSE ────────── │                                  │
  │         {otp_expires_minutes}     │                                  │
  │                                   │                                  │
  │ User checks email & enters OTP    │                                  │
  │                                   │                                  │
  ├──── POST /api/auth/verify-signup-otp ──→ │                          │
  │         {email, otp}              │ Verify OTP                       │
  │                                   │ Create User                      │
  │                                   ├──────────────→ INSERT users    │
  │                                   │                  INSERT user_roles
  │                                   │ ← User Created                 │
  │ ← TOKEN_RESPONSE ─────────────── │                                  │
  │   {access_token, refresh_token}   │                                  │
  │   {requires_assessment: true}     │                                  │
  │                                   │                                  │
  │ Redirect to assessment wizard     │                                  │
  │                                   │                                  │
  ├─ POST /api/assessments/submit ──→ │ Calculate risk level             │
  │    {phq9_answers, gad7_answers,   │ Log assessment                   │
  │     stress_level}                 │ Check for crisis                 │
  │                                   ├──────────────→ INSERT assessments
  │                                   │                  UPDATE users
  │                                   │ ← Assessment ID                │
  │ ← ASSESSMENT_RESPONSE ──────────→ │                                  │
  │    {id, phq9_score, gad7_score,   │                                  │
  │     stress_level, risk_level}     │                                  │
  │                                   │                                  │
  └─ Redirect to dashboard           └──────────────────────────────────┘
```

### Flow 2: Chat with Crisis Detection

```
Client                    Backend                    External Services        Database
  │                          │                             │                   │
  ├─ POST /api/chat/message ─→ │                             │                   │
  │  {message, mood, mood_note}│                             │                   │
  │                           │                             │                   │
  │                           │ 1. Check authentication      │                   │
  │                           │ 2. Validate message          │                   │
  │                           │ 3. Store in conversations    │                   │
  │                           ├──────────────────────────────→ INSERT conversations
  │                           │                             │                   │
  │                           │ 4. Crisis detection:         │                   │
  │                           │    - Keyword matching        │                   │
  │                           │    - Sentiment (VADER)       │                   │
  │                           │    - ML scoring              │                   │
  │                           │                             │                   │
  │                           │ 5. If crisis detected:       │                   │
  │                           ├─ GET /crisis_logs ─────────────────────────────→
  │                           │                             │                   │
  │                           │ 6. Check guardian cooldown   │                   │
  │                           │                             │                   │
  │                           │ 7. Send guardian alert       │                   │
  │                           ├──→ Twilio API ──────────────→ WhatsApp/SMS      │
  │                           │                             │                   │
  │                           │ 8. Store alert log          │                   │
  │                           ├──────────────────────────────→ INSERT guardian_alerts
  │                           │                             │                   │
  │                           │ 9. Search Qdrant            │                   │
  │                           ├──→ Qdrant API ──────────────→ Find similar     │
  │                           │    (Find 5 similar examples) │                   │
  │                           │ ← [Golden examples]         │                   │
  │                           │                             │                   │
  │                           │ 10. Select LLM provider      │                   │
  │                           │ 11. Build prompt with RAG    │                   │
  │                           │                             │                   │
  │                           │ 12. Call LLM                │                   │
  │                           ├──→ Ollama/LLM API ─────────→ Generate response │
  │                           │ ← [AI response]             │                   │
  │                           │                             │                   │
  │                           │ 13. Generate embeddings      │                   │
  │                           │ 14. Store in Qdrant         │                   │
  │                           ├──→ Qdrant API ──────────────→ Store vector    │
  │                           │ ← Vector ID                 │                   │
  │                           │                             │                   │
  │                           │ 15. Store AI response        │                   │
  │                           ├──────────────────────────────→ INSERT conversations
  │                           │                             │                   │
  │                           │ 16. Update game progress     │                   │
  │                           ├──────────────────────────────→ UPDATE games_progress
  │                           │                             │                   │
  │ ← CHAT_RESPONSE ──────────┤ 17. Suggest game            │                   │
  │   {message_id, response,   │     (if appropriate)        │                   │
  │    suggested_game,         │                             │                   │
  │    is_crisis,              │                             │                   │
  │    resources}              │                             │                   │
  │                           │                             │                   │
  └──────────────────────────────────────────────────────────────────────────────┘
```

### Flow 3: RBAC Permission Checking

```
Client                    Backend                         Database
  │                          │                               │
  ├─ GET /api/admin/stats ──→ │                               │
  │   (with JWT token)        │ 1. Extract user_id from JWT  │
  │                           │ 2. Load user                 │
  │                           ├──────────────────→ SELECT users WHERE id=?
  │                           │ ← User object                │
  │                           │                             │
  │                           │ 3. Get user roles            │
  │                           ├──────────────────→ SELECT user_roles
  │                           │                    WHERE user_id=?
  │                           │ ← [role_1, role_2]          │
  │                           │                             │
  │                           │ 4. Get role permission_sets  │
  │                           ├──────────────────→ SELECT role_permission_sets
  │                           │                    WHERE role_id IN (...)
  │                           │ ← [perm_set_1, perm_set_2] │
  │                           │                             │
  │                           │ 5. Get permissions           │
  │                           ├──────────────────→ SELECT permission_set_permissions
  │                           │                    WHERE permission_set_id IN (...)
  │                           │ ← [perm_1, perm_2, ...]    │
  │                           │                             │
  │                           │ 6. Check direct permissions  │
  │                           ├──────────────────→ SELECT user_permissions
  │                           │                    WHERE user_id=?
  │                           │ ← [direct_perm_1, ...]     │
  │                           │                             │
  │                           │ 7. Check for "admin.view_stats" │
  │                           │    Has: YES ✓               │
  │                           │                             │
  │                           │ 8. Execute endpoint         │
  │                           ├──────────────────→ SELECT COUNT(*) from users, ...
  │                           │ ← Stats                     │
  │                           │                             │
  │ ← ADMIN_STATS_RESPONSE ──→ │                            │
  │   {total_users, ...}      │                            │
  │                           │                            │
  │ 9. Log RBAC access        │                            │
  │    in audit_logs          │                            │
  │ ├──────────────────→ INSERT rbac_audit_logs
  │                           │                            │
  └───────────────────────────────────────────────────────┘
```

### Flow 4: Multi-LLM Provider Routing

```
User Message                Backend LLM Service            External LLMs      Database
    │                           │                              │                 │
    │ Send chat message         │                              │                 │
    ├──────────────────────────→ │                              │                 │
    │                           │ 1. Get all active providers  │                 │
    │                           ├──────────────────────────────────→ SELECT llm_providers
    │                           │    WHERE is_active=true      │                 │
    │                           │ ← [Ollama, OpenAI, ...]      │                 │
    │                           │                              │                 │
    │                           │ 2. Check health status       │                 │
    │                           │    (from cache/DB)           │                 │
    │                           │                              │                 │
    │                           │ 3. Select provider:          │                 │
    │                           │    Default: Ollama (healthy) │                 │
    │                           │                              │                 │
    │                           │ 4. Generate embeddings       │                 │
    │                           │    (Sentence Transformers)   │                 │
    │                           │                              │                 │
    │                           │ 5. Search Qdrant            │                 │
    │                           ├──────────────────────────────→ Find similar    │
    │                           │    {embedding, top_k: 5}     │   examples     │
    │                           │ ← [Top 5 examples]           │                 │
    │                           │                              │                 │
    │                           │ 6. Build prompt with RAG     │                 │
    │                           │    (examples + message)      │                 │
    │                           │                              │                 │
    │                           │ 7. Call Ollama               │                 │
    │                           ├──────────────────────────────→ POST /api/generate
    │                           │ {prompt, temperature: 0.7,   │                 │
    │                           │  max_tokens: 2000}           │                 │
    │                           │                              │                 │
    │                           │ 8. Stream response (SSE)     │                 │
    │                           │ ← [AI response chunks]       │                 │
    │                           │                              │                 │
    │ ← Stream response (SSE) ──│                              │                 │
    │   (chunked)               │                              │                 │
    │                           │                              │                 │
    │                           │ 9. Generate title (LSTM)     │                 │
    │                           │ 10. Store full conversation  │                 │
    │                           ├──────────────────────────────────→ INSERT conversations
    │                           │                              │                 │
    │                           │ 11. Update provider stats    │                 │
    │                           ├──────────────────────────────────→ UPDATE llm_providers
    │                           │    total_requests += 1       │                 │
    │                           │    total_tokens += 154       │                 │
    │                           │    last_used_at = now()      │                 │
    │                           │                              │                 │
    └───────────────────────────────────────────────────────────────────────────┘
```

---

## 🎯 HTTP Status Codes & Error Handling

```
Status Codes Used:

✅ Success Responses
├── 200 OK                    → GET/POST successful
├── 201 Created              → Resource created
└── 204 No Content           → Successful delete/no response

❌ Client Errors (4XX)
├── 400 Bad Request          → Invalid input
├── 401 Unauthorized         → Missing/invalid JWT token
├── 403 Forbidden            → Insufficient permissions (RBAC)
├── 404 Not Found            → Resource doesn't exist
├── 409 Conflict             → Resource already exists (e.g., username)
├── 422 Unprocessable Entity → Validation failed
└── 429 Too Many Requests    → Rate limit exceeded (optional)

❌ Server Errors (5XX)
├── 500 Internal Server Error      → General server error
├── 502 Bad Gateway                → LLM service unavailable
├── 503 Service Unavailable        → Database/Qdrant down
└── 504 Gateway Timeout            → LLM timeout
```

---

## 🔐 Request/Response Pattern

### All Requests Include
```
Headers:
├── Authorization: Bearer {access_token}  [for protected routes]
├── Content-Type: application/json
└── User-Agent: {client_info}

Body (POST/PUT):
└── application/json with required/optional fields
```

### All Responses Include
```json
{
  "data": {
    // Response-specific fields
  },
  "message": "Success/Error message",
  "timestamp": "2026-04-29T14:32:00Z",
  "status": 200
}
```

### Error Response Pattern
```json
{
  "detail": "Error message",
  "status": 400,
  "timestamp": "2026-04-29T14:32:00Z"
}
```

---

## 🔄 Middleware Pipeline

```
HTTP Request
    ↓
┌─────────────────────────────────────┐
│ CORS Middleware                     │
│ • Validate origin                   │
│ • Add CORS headers                  │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ Request Logging Middleware          │
│ • Log: method, path, client IP      │
│ • Start timer                       │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ JWT Authentication                  │
│ (if required)                       │
│ • Extract token                     │
│ • Verify signature                  │
│ • Check expiration                  │
│ • Check token blacklist             │
│ • Inject user_id into request       │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ RBAC Permission Check               │
│ (if required)                       │
│ • Load user roles                   │
│ • Load permissions                  │
│ • Check endpoint access             │
│ • Audit log the access              │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ Route Handler / Endpoint            │
│ • Execute business logic            │
│ • Access database/services          │
│ • Return response                   │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│ Response Logging Middleware         │
│ • Log: status code, duration        │
│ • Record timing metrics             │
└─────────────────────────────────────┘
    ↓
HTTP Response
```

---

## 🗄️ Database Query Patterns

### Authentication Flow
```sql
-- 1. User login
SELECT id, password_hash, email_verified, is_active 
FROM users 
WHERE username = $1 AND deleted_at IS NULL;

-- 2. Update last_login
UPDATE users SET last_login = NOW() WHERE id = $1;

-- 3. Verify token (check blacklist)
SELECT id FROM revoked_tokens 
WHERE jti = $1 AND expires_at > NOW();
```

### RBAC Query Pattern
```sql
-- Get all user permissions (combined)
SELECT DISTINCT p.* FROM permissions p
LEFT JOIN permission_set_permissions psp ON p.id = psp.permission_id
LEFT JOIN permission_sets ps ON ps.id = psp.permission_set_id
LEFT JOIN role_permission_sets rps ON ps.id = rps.permission_set_id
LEFT JOIN roles r ON r.id = rps.role_id
LEFT JOIN user_roles ur ON ur.role_id = r.id
LEFT JOIN user_permissions up ON up.permission_id = p.id
LEFT JOIN user_permission_sets ups ON ups.permission_set_id = ps.id
WHERE 
  ur.user_id = $1              -- User's roles
  OR up.user_id = $1           -- Direct permissions
  OR ups.user_id = $1          -- Direct permission sets
  AND p.is_active = true;
```

### Crisis Detection Query
```sql
-- Find recent crisis logs
SELECT COUNT(*) as crisis_count, 
       MAX(created_at) as last_crisis
FROM crisis_logs
WHERE user_id = $1
  AND created_at > NOW() - INTERVAL '24 hours'
  AND resolved = false;

-- Check guardian alert cooldown
SELECT created_at FROM guardian_alerts
WHERE user_id = $1
  AND alert_sent = true
ORDER BY created_at DESC
LIMIT 1;
```

### Qdrant Vector Search
```python
# Search for similar conversations
results = qdrant_client.search(
    collection_name="neurowellca_conversations",
    query_vector=embedding,              # 384-dim vector
    limit=5,                             # Top 5 results
    score_threshold=0.7
)
```

---

## 📱 Frontend API Calls (from Next.js)

### Example: Chat API Call
```typescript
// lib/chat.ts
async function sendMessage(message: string, sessionId?: string) {
  const response = await axios.post('/api/chat/message', {
    message,
    mood: getCurrentMood(),
    mood_note: getMoodNote(),
    session_id: sessionId
  }, {
    headers: {
      Authorization: `Bearer ${getAccessToken()}`
    }
  });
  
  return response.data;
}

// Usage in component
const response = await sendMessage("I'm feeling anxious");
// Returns: {
//   message_id: 123,
//   response: "I understand...",
//   is_crisis: false,
//   suggested_game: {...},
//   resources: [...]
// }
```

### Example: Streaming Response
```typescript
// Server-Sent Events for streaming
async function streamChatMessage(message: string) {
  const eventSource = new EventSource(
    `/api/chat/stream-message?message=${encodeURIComponent(message)}`,
    {
      headers: { Authorization: `Bearer ${token}` }
    }
  );
  
  eventSource.onmessage = (event) => {
    const chunk = JSON.parse(event.data);
    updateChatUI(chunk.content);
  };
  
  eventSource.onerror = () => eventSource.close();
}
```

---

## 🔌 Service Integration Points

### LLM Integration
```python
# Service call
response = await llm_service.generate_response(
    message=user_message,
    context=rag_context,        # From Qdrant
    persona=ai_persona,         # From config
    provider_id=selected_provider
)
```

### Qdrant Integration
```python
# Store embedding
await qdrant_service.store_conversation(
    conversation_id=conv_id,
    text=message,
    embedding=embedding_vector,
    metadata={
        'user_id': user_id,
        'sentiment': sentiment_score,
        'is_crisis': is_crisis
    }
)

# Search
results = await qdrant_service.search_similar(
    query_vector=embedding,
    top_k=5,
    filters={'user_id': user_id}
)
```

### Guardian Alert Integration
```python
# Send alert
await guardian_alert_service.send_alert(
    user_id=user_id,
    crisis_message=message,
    channels=['whatsapp', 'email']  # Preferred channels
)
```

---

## 📊 API Performance Metrics

```
Expected Response Times:
├── Authentication (/auth/login)        : 100-200 ms
├── Chat message (/chat/message)        : 2,000-5,000 ms (LLM-dependent)
├── Streaming chat (/chat/stream-message): 30+ seconds (streaming)
├── Assessment submit (/assessments)    : 200-300 ms
├── Dashboard stats (/dashboard/stats)  : 150-250 ms
├── RBAC check (per request)           : 50-100 ms
└── Database queries (avg)              : 10-50 ms

Load Capacity (per server instance):
├── Concurrent connections             : 1,000+
├── Requests per second                 : 500+ RPS
├── Database connections (pool)         : 20-50
└── Qdrant connections                  : 10-20
```

---

## 🔐 Authentication Token Flow

```
1. User logs in
   POST /api/auth/login
   ← {access_token, refresh_token, expires_in: 3600}

2. Store tokens locally
   localStorage.setItem('access_token', token)
   localStorage.setItem('refresh_token', token)

3. Use access_token in subsequent requests
   Authorization: Bearer {access_token}

4. On expiration (after 60 min)
   POST /api/auth/refresh-token
   {refresh_token: ...}
   ← {access_token: new_token, refresh_token: new_refresh}

5. On logout
   POST /api/auth/logout
   → Token added to blacklist (revoked_tokens)
   → Clear localStorage
   → Redirect to login
```

---

**Last Updated**: April 29, 2026
**API Version**: 2.0.0
**Base URL**: `http://localhost:8000/api` (dev) or `https://api.neurowellca.com/api` (prod)
