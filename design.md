# SheLeads - Design Document

## 1. System Architecture

### 1.1 High-Level Architecture

```
┌───────────────────────┐
│       User Layer      │
│                       │
│ • WhatsApp (Phase 1)  │
│ • Mobile App (Phase 2)│
│ • Web Portal (Admin)  │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│  Integration Layer     │
│                        │
│ • WhatsApp Business API│
│ • Voice Notes Support  │
│ • Govt Scheme Sources  │
│ • Mentor Platform      │
└───────────┬───────────┘
            │
            ▼
┌─────────────────────────────────────────────────────────┐
│                    Application Layer                     │
│                                                         │
│ • User & Business Profiles                               │
│ • Business Tools (pricing, profit, cash flow)            │
│ • Mentor Readiness & Matching                            │
│ • Conversation Orchestration + Safety                    │
└───────────┬───────────────────────────────┬─────────────┘
            │                               │
            ▼                               ▼
┌───────────────────────┐         ┌────────────────────────┐
│       AI Layer         │         │       Data Layer        │
│                        │         │                        │
│ • LLM Agent            │         │ • PostgreSQL            │
│ • Tool Use + Functions │         │ • Redis Cache           │
│ • RAG Knowledge Base   │         │ • S3 Document Storage   │
│ • Language Handling    │         │                        │
└───────────┬───────────┘         └───────────┬────────────┘
            │                                 │
            ▼                                 ▼
┌─────────────────────────────────────────────────────────┐
│                  Monitoring & Analytics                  │
│                                                         │
│ • AWS CloudWatch                                         │
│ • Privacy-safe metrics (no PII in analytics)             │
└─────────────────────────────────────────────────────────┘
```

## 2. Technology Stack (MVP-first, AWS-aligned)

### 2.1 Frontend / Interfaces
- WhatsApp Business API (Phase 1)
- React Native mobile app (Phase 2)
- Admin Web Portal (simple dashboard for mentors/admins)

**Voice Interaction:**
- Phase 1: WhatsApp voice notes + optional transcription (async)
- Phase 2: richer voice UI inside app

### 2.2 Backend
- Python (FastAPI) for API + AI orchestration
- REST APIs (simpler than GraphQL for MVP)
- Redis for caching and rate limiting
- PostgreSQL as the primary database
- AWS S3 for document storage (optional)

### 2.3 AI/ML Stack (Agent-Based)

**Core AI Agent**
- LLM-powered conversational agent built for women micro-entrepreneurs
- Tool-use / function calling for:
  - pricing calculators
  - profit margin estimation
  - cash-flow tracking
  - scheme matching
  - mentor readiness scoring

**Retrieval-Augmented Generation (RAG)**
- Knowledge base includes:
  - Government schemes (MUDRA, Stand-Up India, state schemes)
  - Sector-specific guidance (tailoring, catering, beauty, handicrafts)
  - Pricing templates and business checklists
- Pipeline: ingestion → chunking → embeddings → vector search → grounded response

**Multilingual Support**
- Language detection + localized responses
- Phase 1: Hindi + English
- Phase 2: Tamil, Telugu, Bengali, Marathi (expansion)

**Optional Fine-Tuning (Future Scope)**
- Fine-tuning smaller open models (e.g., Llama/Mistral class) can be explored later
- Training data: anonymized and consented user queries + curated mentor Q&A

## 3. User Experience Design

### 3.1 Design Principles
- **Simplicity First**: minimal steps, no business jargon
- **Progressive Complexity**: beginner → intermediate → scaling
- **Voice Optional (Phase 1)**: voice notes supported, but text-first fallback
- **Cultural Sensitivity**: supportive tone, realistic constraints
- **Empowerment Focus**: confidence-building guidance

### 3.2 User Journey

**Onboarding Flow**
WhatsApp Opt-in → Basic Profile → Business Type → Language Preference → First Guidance

**Daily Usage Flow**
User Question → Intent Detection → Tool / RAG Lookup → Response → Next Step Suggestion

### 3.3 WhatsApp UX (Phase 1)
- Emoji-based menu for low cognitive load
- Quick replies for common actions:
  - "Track Expense"
  - "Set Price"
  - "Find Scheme"
  - "Grow Customers"
- Works with:
  - text messages
  - images (bills, notes)
  - voice notes (optional)

## 4. WhatsApp Constraints & Messaging Design 

### 4.1 WhatsApp Business API Constraints
- **24-hour messaging window**: Saathi can respond freely within 24 hours of user message
- After 24 hours, only approved templates can be used
- Template approvals required for proactive reminders
- Outbound rate limits
- Limited interaction formats compared to a full app

### 4.2 How SheLeads Handles This
- Proactive nudges only through approved templates
- Core experience remains user-initiated (ask → respond)
- Phase 2 app removes most constraints

## 5. Offline & Low-Connectivity Support

This is critical for Bharat users, especially in Tier 2/3 and semi-rural regions.

**Phase 1 (WhatsApp)**
- Works on low bandwidth since WhatsApp is lightweight
- Cached replies for FAQs and common business concepts
- Async voice processing (voice note → "processing" → reply later)

**Phase 2 (Mobile App)**
- Local device storage (SQLite)
- Queue-based sync when online
- Offline access to:
  - saved learning modules
  - business templates
  - previous AI guidance
- Text-only mode when network is weak

## 6. Core Features (MVP)

### 6.1 AI Business Mentor
Conversational mentor for:
- customer handling
- marketing basics (WhatsApp/Instagram)
- inventory advice
- growth decisions

### 6.2 Financial Tracking (Simple, Daily)
- Daily income/expense tracking
- Profit estimation (weekly/monthly)
- Cash flow warnings

### 6.3 Pricing Intelligence
Price suggestions based on:
- cost of materials
- time investment
- profit margin goals
- local competition (basic)

### 6.4 Legal & Scheme Guidance
Simple explanation of:
- business registration options
- GST basics (only if applicable)
- Government scheme discovery:
  - MUDRA
  - Stand-Up India
  - state-level women entrepreneurship programs

### 6.5 Mentor Connections (Phase 2+)
- Mentor readiness score
- Matching based on:
  - sector
  - region/language
  - business stage

## 7. Data Architecture

### 7.1 Core Data Models

**User Profile**
```json
{
  "userId": "string",
  "name": "string",
  "location": "string",
  "language": "string",
  "businessType": "string",
  "sector": "string",
  "stage": "string",
  "createdAt": "date"
}
```

**Business Transaction**
```json
{
  "transactionId": "string",
  "userId": "string",
  "date": "date",
  "type": "income|expense",
  "amount": "number",
  "category": "string",
  "notes": "string"
}
```

**AI Interaction**
```json
{
  "interactionId": "string",
  "userId": "string",
  "timestamp": "date",
  "intent": "string",
  "language": "string",
  "responseType": "tool|rag|general",
  "feedback": "optional"
}
```

## 8. AI Safety, Quality & Content Strategy

### 8.1 Knowledge Curation
- Sector-specific business guidance is curated from:
  - verified business templates
  - NGO / government resources
  - mentor-created best practices
- Scheme data is refreshed periodically (schemes change frequently)

### 8.2 Content Moderation & Abuse Handling
- Spam prevention and rate limiting per user
- Inappropriate message detection
- Mentor chat moderation rules (Phase 2)
- Clear community guidelines

### 8.3 Feedback Loop
Users can rate responses:
- 👍 Helpful
- 👎 Not helpful
- Low-rated responses are flagged for improvement
- Regular user interviews for refinement

## 9. Security, Privacy & Compliance

### 9.1 Security Measures
- HTTPS everywhere
- Rate limiting
- Input validation
- Secure storage for credentials and API keys
- Encrypted backups

### 9.2 Privacy Principles
- Minimal data collection
- Explicit consent
- Easy data deletion
- No selling of user data

### 9.3 Indian Compliance & Data Sovereignty
Compliance aligned with:
- Digital Personal Data Protection Act (DPDP) 2023
- Data localization: Indian users' data stored in AWS India region
- Right to:
  - access
  - correction
  - deletion
  - portability (future scope)

## 10. Cost & Sustainability Architecture (High-Level)

Saathi is designed to be sustainable at scale.

**Key Cost Drivers**
- WhatsApp Business API conversation charges
- AI inference costs (especially voice transcription)
- Database + storage costs
- Outbound notifications (templates)

**Cost Control Strategies**
- Cache common answers (reduce repeated LLM calls)
- Use RAG for factual queries (reduce token usage)
- Voice transcription is optional + async (Phase 1)
- Move to smaller fine-tuned models in Phase 2/3 for cost efficiency

## 11. Performance & Reliability

**Response Targets (MVP)**
- Text response: < 3 seconds for most queries
- Voice note processing: async (respond when ready)
- Business tool calculations: instant

**Graceful Degradation**
- If AI fails: fallback to FAQs + basic tools
- If scheme lookup fails: show saved scheme list
- If network is weak: text-only mode

## 12. Monitoring & Analytics (Privacy-Safe)

**Monitoring**
- AWS CloudWatch logs and alerts
- Error tracking for:
  - WhatsApp failures
  - AI API failures
  - database issues

**Analytics (no PII)**
- engagement
- retention
- feature usage
- business tool usage
- mentor matching success rates

## 13. Deployment Strategy

**Phase 1: WhatsApp MVP (0–6 months)**
- AI mentor (Hindi + English)
- Basic finance tracking + pricing
- Scheme discovery
- Pilot: 500–1,000 active users (single state)

**Phase 2: Scale WhatsApp + Mentor Layer (6–12 months)**
- Multi-language expansion
- Mentor matching rollout
- 5,000–10,000 users across 2–3 states

**Phase 3: Mobile App Launch (12–18 months)**
- Offline-first app
- richer tools + dashboards
- 25,000–50,000 users

**Phase 4: Pan-India Expansion (18–24 months)**
- Sector expansion
- partnerships with NGOs, SHGs, government programs
- 100,000+ users

## 14. Testing & Quality Assurance

**Testing Approach**
- Unit tests for business tools (pricing, profit calculations)
- Integration tests for WhatsApp flow
- User testing with real women entrepreneurs
- Regional language testing with native speakers
- Accessibility testing for low-tech users
- Load testing before scale phases

## 15. Summary

SheLeads is a voice-friendly, multilingual AI business mentor designed specifically for women micro-entrepreneurs in India. It prioritizes accessibility, trust, and real-world constraints (WhatsApp limitations, low connectivity, and cost). The system is designed to be MVP-friendly today and scalable into a sustainable national platform.
