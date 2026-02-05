# SheLeads - Requirements Document

## 1. Project Overview

### 1.1 Product Name
SheLeads

### 1.2 Problem Summary
Women micro-entrepreneurs in India (tailoring, catering, beauty services, handicrafts) often run businesses without access to business education, mentorship, financial literacy, or reliable guidance. They face barriers such as time poverty, isolation, low confidence, limited access to schemes/loans, and lack of networks.

### 1.3 Proposed Solution
SheLeads is a mobile-first AI business mentor that starts as a WhatsApp-based assistant (Phase 1) and evolves into a dedicated app (Phase 2). It provides multilingual guidance for:
- finance and expense tracking
- pricing strategies
- customer and marketing support
- scheme discovery and compliance basics
- growth planning
- mentor connection when ready

## 2. Goals and Objectives

### 2.1 Primary Goals
- Provide accessible, practical business guidance for women micro-entrepreneurs.
- Help users improve profitability, pricing, and cash flow clarity.
- Improve awareness of government schemes and women-focused programs.
- Enable a bridge from AI guidance to human mentorship.

### 2.2 Success Metrics 
Users can successfully:
- track expenses/income
- receive a price suggestion
- get scheme recommendations
- receive a step-by-step growth plan

User satisfaction feedback:
- 👍 helpful / 👎 not helpful

## 3. Target Users

### 3.1 Primary Users
- Women aged 25–45
- Running micro-businesses with annual revenue under ₹10 lakhs
- Tier 2 / Tier 3 cities (Phase 1)
- Sectors:
  - tailoring
  - food preparation / catering
  - beauty services
  - handicrafts

### 3.2 Key Constraints
- Low or unstable internet
- Limited formal business education
- Limited time availability
- Mixed literacy levels
- Preference for WhatsApp and voice notes

## 4. Scope

### 4.1 In Scope (MVP)

**WhatsApp-Based AI Mentor**
- Chat interface through WhatsApp
- Text-first experience with optional voice notes
- Hindi + English support (Phase 1)

**Business Tools**
- daily income/expense tracking
- simple profit estimation
- pricing recommendation tool

**Scheme & Compliance Guidance**
- scheme discovery (MUDRA, Stand-Up India, etc.)
- basic registration and compliance guidance

**Safety & Reliability**
- fallback responses if AI fails
- rate limiting and abuse protection

### 4.2 Out of Scope (for MVP)
- Full GST filing integration
- Automated loan application submission
- Real-time competitor market scraping
- Advanced analytics dashboards
- Full community forum features
- Training large models from scratch

## 5. Functional Requirements

**FR-1: User Onboarding**
- The system shall allow users to opt-in via WhatsApp.
- The system shall collect:
  - name (optional)
  - location (city/state)
  - preferred language
  - business sector
  - business stage (starting/running/scaling)

**FR-2: Multilingual Support**
- The system shall support Hindi and English in Phase 1.
- The system shall detect language automatically when possible.
- The system shall expand to regional languages in Phase 2.

**FR-3: Conversational AI Mentor**
- The system shall answer business queries related to:
  - customer handling
  - marketing basics
  - inventory basics
  - business planning
- The system shall provide actionable next steps.

**FR-4: Financial Tracking**
- The system shall allow users to record:
  - income transactions
  - expense transactions
- The system shall provide summaries:
  - daily/weekly totals
  - basic profit estimation

**FR-5: Pricing Recommendation**
- The system shall generate pricing suggestions based on:
  - material cost
  - time/labor input
  - target profit margin
- The system shall explain the recommendation in simple language.

**FR-6: Scheme Discovery**
- The system shall recommend schemes based on:
  - user location
  - business type
  - eligibility basics
- The system shall present the scheme requirements clearly.

**FR-7: Mentor Readiness (Basic)**
- The system shall assign a simple readiness stage:
  - Beginner
  - Intermediate
  - Ready for mentor
- The system shall suggest when a human mentor is beneficial.

**FR-8: Feedback Collection**
- The system shall allow users to rate responses as:
  - Helpful / Not Helpful
- The system shall store feedback for future improvement.

**FR-9: Safety and Abuse Prevention**
- The system shall implement:
  - per-user rate limits
  - spam protection
  - basic inappropriate message detection
- The system shall refuse unsafe or irrelevant requests politely.

## 6. Non-Functional Requirements

**NFR-1: Accessibility**
- The system shall support low-tech users through:
  - simple language
  - minimal menus
  - WhatsApp-native interactions
- The system shall support voice notes as optional input.

**NFR-2: Performance**
- The system shall respond to most text queries in under 3 seconds.
- Voice note transcription (if enabled) shall be asynchronous.

**NFR-3: Reliability**
- The system shall provide a fallback mode:
  - FAQ-based replies
  - basic tools without AI

**NFR-4: Scalability**
- The system shall support scaling from:
  - 1,000 users → 10,000 users → 100,000 users
- The architecture shall support horizontal scaling.

**NFR-5: Privacy & Compliance**
- The system shall follow minimal data collection.
- The system shall obtain explicit user consent.
- The system shall comply with:
  - Digital Personal Data Protection Act (DPDP) 2023
- Indian user data shall be stored in an India region (AWS India).

**NFR-6: Cost Efficiency**
- The system shall reduce AI inference costs using:
  - caching for common questions
  - RAG for factual answers
  - templates for repetitive responses
- Voice transcription shall be optional in Phase 1.

## 7. Assumptions & Constraints

### 7.1 Assumptions
- Users have access to WhatsApp and basic smartphones.
- Users are willing to share basic business data (optional).
- Government scheme information can be curated and updated regularly.

### 7.2 Constraints
**WhatsApp Business API:**
- 24-hour messaging window
- template approvals for proactive messages
- rate limits

Voice AI is expensive and may be limited in Phase 1.

## 8. Risks & Mitigation

**Risk: Wrong advice from AI**
Mitigation:
- use RAG with verified sources
- include disclaimers for legal/financial advice
- keep responses grounded and practical

**Risk: Cost blow-up due to WhatsApp + AI usage**
Mitigation:
- caching
- RAG-first responses
- phased rollout with controlled pilots

**Risk: Low adoption due to trust issues**
Mitigation:
- empathetic tone
- culturally sensitive onboarding
- explain "why" behind advice
- partner with SHGs/NGOs in later phases

## 9. Future Enhancements (Post-MVP)
- Dedicated mobile app with offline-first storage
- Mentor marketplace + scheduling
- Community learning circles
- Advanced business analytics
- Fine-tuning smaller models for cost reduction
- Partnerships with NGOs, SHGs, and government programs

## 10. Conclusion
SheLeads is designed as a practical and scalable AI business mentor for women micro-entrepreneurs in India. It starts with a WhatsApp-first MVP to maximize adoption, supports multilingual and low-connectivity environments, and provides real impact through financial tools, scheme guidance, and growth support.
