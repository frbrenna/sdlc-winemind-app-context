# Application Specification: WineMind

An AI-powered wine pairing assistant that analyzes your meal and recommends the perfect wine.

---

## Executive Summary

WineMind is a web application that helps users discover the ideal wine pairing for any meal. Users upload a photo of their dish, and an AI agent analyzes the image, identifies the food, considers the user's taste preferences and budget, then searches the web to recommend wines that complement the meal perfectly.

---

## Business Requirements

### BR-001: AI-Powered Dish Recognition

**Description:**
The system must accurately identify dishes from user-uploaded images to determine appropriate wine pairings.

**Business Justification:**
Accurate dish identification is fundamental to providing relevant wine recommendations. Users expect the convenience of simply taking a photo rather than describing their meal.

**Success Metrics:**
- 90% accuracy in identifying common dish types
- Dish identification completes within 5 seconds
- User confirmation rate of identified dish exceeds 85%

**Constraints:**
- Must handle various image qualities (smartphone cameras)
- Must work with partial dish visibility

---

### BR-002: Personalized Wine Recommendations

**Description:**
The system must consider user preferences when recommending wines, including taste preferences, budget range, and dietary restrictions.

**Business Justification:**
Generic recommendations fail to account for individual tastes. Personalization increases user satisfaction and trust in recommendations.

**Success Metrics:**
- Users rate recommendations as "helpful" 80% of the time
- Repeat usage rate of 60% within 30 days
- Preference profile completion rate of 70%

**Dependencies:**
- User profile system
- Preference storage

---

### BR-003: Real-Time Wine Search

**Description:**
The system must search the web to find currently available wines that match the pairing criteria and user preferences.

**Business Justification:**
Wine availability changes constantly. Web search ensures recommendations are for wines users can actually purchase.

**Success Metrics:**
- At least 3 wine recommendations per query
- Wines recommended are available for purchase online 90% of the time
- Price accuracy within 10% of actual retail price

---

### BR-004: Pairing Explanation

**Description:**
The system must explain why each wine pairs well with the identified dish in accessible, educational language.

**Business Justification:**
Users want to learn about wine pairing, not just receive recommendations. Education builds trust and engagement.

**Success Metrics:**
- Explanation readability score at 8th grade level or below
- Users report "learning something new" 70% of the time

---

## User Stories

### Epic: User Onboarding

#### US-001: Create Account

As a **new user**,
I want **to create an account with minimal friction**,
So that **I can save my preferences and access my pairing history**.

**Acceptance Criteria:**
- [ ] User can sign up with email/password or social login (Google)
- [ ] Email verification sent within 30 seconds
- [ ] Account creation completes in under 60 seconds
- [ ] User is prompted to set preferences immediately after signup
- [ ] Guest mode available for users who don't want to create an account

**Priority:** High
**Estimated Effort:** M

---

#### US-002: Set Wine Preferences

As a **new user**,
I want **to set my wine preferences during onboarding**,
So that **recommendations match my taste from the start**.

**Acceptance Criteria:**
- [ ] Preference wizard presents 5-7 simple questions
- [ ] Questions cover: red/white/rosé preference, sweetness tolerance, body preference, budget range
- [ ] User can skip questions and set preferences later
- [ ] Visual aids (images, sliders) make selection intuitive
- [ ] Preferences can be updated anytime from profile settings
- [ ] System explains how each preference affects recommendations

**Priority:** High
**Estimated Effort:** M

**Preference Categories:**
- Wine types: Red, White, Rosé, Sparkling, Dessert
- Taste profile: Dry to Sweet scale (1-5)
- Body: Light, Medium, Full
- Budget per bottle: $10-20, $20-40, $40-75, $75+
- Dietary: Organic, Vegan, Low-sulfite preferences

---

### Epic: Dish Analysis

#### US-003: Upload Dish Photo

As a **user ready to find a wine pairing**,
I want **to upload or take a photo of my dish**,
So that **the AI can analyze what I'm eating**.

**Acceptance Criteria:**
- [ ] User can upload image from device gallery
- [ ] User can take photo directly within the app using device camera
- [ ] Supported formats: JPEG, PNG, HEIC
- [ ] Maximum file size: 10MB
- [ ] Image preview shown before submission
- [ ] User can crop or rotate image before submitting
- [ ] Loading indicator shows analysis in progress
- [ ] Error message displayed if image cannot be processed

**Priority:** High
**Estimated Effort:** M

---

#### US-004: Review Dish Identification

As a **user who uploaded a dish photo**,
I want **to confirm or correct the AI's dish identification**,
So that **wine recommendations are based on accurate information**.

**Acceptance Criteria:**
- [ ] AI displays identified dish name with confidence indicator
- [ ] Key characteristics detected are shown (e.g., "grilled salmon with lemon butter sauce")
- [ ] User can confirm identification with single tap
- [ ] User can edit or provide correct dish name if AI is wrong
- [ ] User can add notes about preparation or ingredients not visible
- [ ] Corrections improve future AI accuracy (feedback loop)

**Priority:** High
**Estimated Effort:** S

---

### Epic: Wine Recommendations

#### US-005: Receive Wine Recommendations

As a **user with an identified dish**,
I want **to receive personalized wine recommendations**,
So that **I can choose the perfect wine for my meal**.

**Acceptance Criteria:**
- [ ] System displays 3-5 wine recommendations
- [ ] Each recommendation shows: wine name, winery, region, vintage (if relevant), price range
- [ ] Recommendations are ranked by pairing strength
- [ ] Match score displayed (e.g., 95% match)
- [ ] User preferences visibly influence results (e.g., "Within your $20-40 budget")
- [ ] Recommendations load within 10 seconds
- [ ] User can request more recommendations

**Priority:** High
**Estimated Effort:** L

---

#### US-006: View Pairing Explanation

As a **user reviewing wine recommendations**,
I want **to understand why each wine pairs well with my dish**,
So that **I can make an informed choice and learn about wine pairing**.

**Acceptance Criteria:**
- [ ] Each recommendation has expandable explanation section
- [ ] Explanation covers: flavor complementarity, texture pairing, regional tradition (if applicable)
- [ ] Language is accessible to wine beginners
- [ ] Sommelier tips included (e.g., "Serve slightly chilled")
- [ ] Tasting notes describe what to expect
- [ ] Food pairing principles explained in educational sidebar

**Priority:** Medium
**Estimated Effort:** M

---

#### US-007: Find Where to Buy

As a **user who selected a wine recommendation**,
I want **to see where I can purchase the wine**,
So that **I can easily buy it for my meal**.

**Acceptance Criteria:**
- [ ] Links to online retailers where wine is available
- [ ] Price comparison across retailers
- [ ] Local availability shown based on user location (if provided)
- [ ] Affiliate links clearly disclosed
- [ ] Alternative vintages suggested if specific vintage unavailable
- [ ] "Save for later" option adds wine to user's wishlist

**Priority:** Medium
**Estimated Effort:** M

---

### Epic: User Profile & History

#### US-008: View Pairing History

As a **returning user**,
I want **to view my past dish and wine pairings**,
So that **I can reference previous recommendations and reorder wines I enjoyed**.

**Acceptance Criteria:**
- [ ] History shows dish photo, identified dish, recommended wines, and date
- [ ] User can mark wines as "Loved it" or "Not for me"
- [ ] Ratings influence future recommendations
- [ ] History searchable by dish name or wine name
- [ ] User can delete individual history entries
- [ ] History retained for 2 years

**Priority:** Medium
**Estimated Effort:** M

---

#### US-009: Update Preferences

As a **user whose tastes have evolved**,
I want **to update my wine preferences**,
So that **future recommendations reflect my current tastes**.

**Acceptance Criteria:**
- [ ] All preference categories editable from profile settings
- [ ] Changes take effect immediately on next recommendation
- [ ] User can reset preferences to retake onboarding quiz
- [ ] Preference history not tracked (only current state matters)

**Priority:** Low
**Estimated Effort:** S

---

#### US-010: Share Pairing

As a **user who found a great pairing**,
I want **to share the dish and wine recommendation with friends**,
So that **I can spread the discovery and help others**.

**Acceptance Criteria:**
- [ ] Share button generates shareable link
- [ ] Shared link shows dish photo, wine recommendation, and pairing explanation
- [ ] Social sharing to major platforms (Facebook, Twitter, WhatsApp)
- [ ] Copy link option for manual sharing
- [ ] Shared page viewable without account

**Priority:** Low
**Estimated Effort:** S

---

## Non-Functional Requirements

### NFR-001: Image Processing Performance

**Category:** Performance

**Requirement:**
Dish image analysis must complete within 5 seconds for 95% of requests.

**Measurement:**
P95 latency tracked via application monitoring.

**Priority:** Critical

---

### NFR-002: Recommendation Response Time

**Category:** Performance

**Requirement:**
Wine recommendations (including web search) must return within 15 seconds.

**Measurement:**
End-to-end timing from dish confirmation to recommendation display.

**Priority:** Critical

---

### NFR-003: Image Data Security

**Category:** Security

**Requirement:**
User-uploaded images must be encrypted in transit and at rest, and deleted within 30 days unless user opts to save.

**Measurement:**
Security audit verification; automated deletion job logs.

**Priority:** High

---

### NFR-004: Authentication Security

**Category:** Security

**Requirement:**
User authentication must use industry-standard protocols with secure session management.

**Measurement:**
JWT tokens with appropriate expiration; OAuth 2.0 compliance for social login.

**Priority:** High

---

### NFR-005: Mobile Responsiveness

**Category:** Usability

**Requirement:**
Application must be fully functional on mobile devices with screens 375px and wider.

**Measurement:**
Cross-device testing on iOS Safari, Android Chrome; Lighthouse mobile score above 90.

**Priority:** High

---

### NFR-006: Accessibility Compliance

**Category:** Usability

**Requirement:**
Application must meet WCAG 2.1 Level AA accessibility standards.

**Measurement:**
Automated accessibility testing; manual screen reader testing.

**Priority:** Medium

---

### NFR-007: Availability

**Category:** Reliability

**Requirement:**
Application must maintain 99.5% uptime excluding scheduled maintenance.

**Measurement:**
Uptime monitoring with alerting.

**Priority:** High

---

### NFR-008: Scalability

**Category:** Scalability

**Requirement:**
System must handle 1,000 concurrent users without performance degradation.

**Measurement:**
Load testing with simulated traffic.

**Priority:** Medium

---

## Architecture Decisions

### ADR-001: AI Agent Framework Selection

**Status:** Accepted

**Context:**
The application requires AI capabilities for image analysis (dish recognition) and intelligent web search (wine discovery). We need a framework that supports multi-modal inputs and tool use (web search).

**Decision:**
Use AWS Bedrock with Claude as the foundation model for all AI agent capabilities.

**Consequences:**

*Positive:*
- Native multi-modal support (image + text)
- Tool use capability for web search integration
- Managed infrastructure reduces operational burden
- Strong reasoning for pairing explanations

*Negative:*
- AWS lock-in for AI capabilities
- Usage-based pricing requires cost monitoring

*Risks:*
- Model availability during high demand — mitigate with retry logic and queue

**Alternatives Considered:**
1. OpenAI GPT-4V — Strong multi-modal but less integrated with AWS ecosystem
2. Self-hosted open source — Operational complexity too high for initial launch

---

### ADR-002: Frontend Framework Selection

**Status:** Accepted

**Context:**
Need a modern frontend framework that supports responsive design, fast development, and excellent mobile experience.

**Decision:**
Use React with shadcn/ui component library and Tailwind CSS for styling.

**Consequences:**

*Positive:*
- shadcn/ui provides accessible, customizable components
- Tailwind enables rapid UI development
- Large ecosystem and community support
- Consistent with coding guidelines

*Negative:*
- Bundle size requires optimization attention

**Alternatives Considered:**
1. Vue.js + Vuetify — Viable but team has more React experience
2. Next.js — SSR benefits not critical for this application

---

### ADR-003: Backend Framework Selection

**Status:** Accepted

**Context:**
Backend must handle image uploads, integrate with AWS Bedrock, and provide REST APIs for the frontend.

**Decision:**
Use Python with FastAPI framework.

**Consequences:**

*Positive:*
- Native async support for external API calls
- Excellent AWS SDK (boto3) support
- Built-in OpenAPI documentation
- Consistent with coding guidelines

*Negative:*
- Python performance for CPU-intensive tasks (mitigated by offloading to AI services)

**Alternatives Considered:**
1. Node.js/Express — Good but Python has better AI/ML ecosystem
2. Go — Performance benefits not needed; slower development velocity

---

### ADR-004: Web Search Strategy

**Status:** Accepted

**Context:**
Wine recommendations must include currently available wines with purchase links, requiring real-time web search.

**Decision:**
Implement web search as a tool within the Bedrock agent using a web search API (e.g., Tavily, Brave Search API, or SerpAPI).

**Consequences:**

*Positive:*
- Agent can intelligently construct search queries based on dish and preferences
- Results can be filtered and ranked by the agent
- Flexible to search multiple sources (retailers, wine databases, reviews)

*Negative:*
- Search API costs per query
- Result quality depends on search API

*Risks:*
- Rate limiting on search APIs — mitigate with caching common queries

**Alternatives Considered:**
1. Curated wine database — Limited selection, stale data
2. Direct retailer API integrations — Complex, limited coverage initially

---

### ADR-005: Image Storage Strategy

**Status:** Accepted

**Context:**
User-uploaded dish images need temporary storage for processing and optional long-term storage for history.

**Decision:**
Use AWS S3 for image storage with lifecycle policies for automatic deletion.

**Consequences:**

*Positive:*
- Scalable, durable storage
- Easy integration with Bedrock for image analysis
- Lifecycle policies automate retention compliance

*Negative:*
- Additional AWS service dependency

---

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                           Frontend                                   │
│                  React + shadcn/ui + Tailwind                       │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────┐
│                         API Gateway                                  │
│                        AWS API Gateway                               │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────┐
│                          Backend                                     │
│                    Python + FastAPI                                  │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐                  │
│  │   Auth      │  │   Upload    │  │  Pairing    │                  │
│  │   Service   │  │   Service   │  │  Service    │                  │
│  └─────────────┘  └─────────────┘  └─────────────┘                  │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                    ┌─────────────┼─────────────┐
                    ▼             ▼             ▼
            ┌───────────┐ ┌───────────┐ ┌───────────┐
            │    S3     │ │  Bedrock  │ │  DynamoDB │
            │  Images   │ │   Agent   │ │   Users   │
            └───────────┘ └───────────┘ └───────────┘
                                │
                                ▼
                    ┌───────────────────┐
                    │   Web Search API  │
                    │  (Agent Tool)     │
                    └───────────────────┘
```

---

## Agent Design

### Wine Pairing Agent

**Purpose:** Analyze dish images and recommend wine pairings based on user preferences.

**System Prompt Outline:**
```
You are a sommelier AI assistant. Your role is to:
1. Analyze dish images to identify the food and its characteristics
2. Consider the user's wine preferences and budget
3. Search for wines that pair well with the identified dish
4. Explain your pairing recommendations in accessible language

When analyzing a dish, identify:
- Main proteins or vegetables
- Cooking method (grilled, fried, braised, raw)
- Sauce or seasoning profiles
- Cuisine origin

When recommending wines, consider:
- Flavor complementarity and contrast
- Weight/body matching
- Regional pairing traditions
- User's stated preferences
```

**Tools:**
1. `web_search` — Search for wine recommendations and availability
2. `get_user_preferences` — Retrieve user's saved preferences
3. `get_wine_details` — Fetch detailed information about specific wines

---

## Data Models

### User
```
{
  id: string
  email: string
  created_at: datetime
  preferences: {
    wine_types: string[]
    sweetness: int (1-5)
    body: string
    budget_min: int
    budget_max: int
    dietary: string[]
  }
}
```

### Pairing History
```
{
  id: string
  user_id: string
  created_at: datetime
  dish_image_url: string
  identified_dish: string
  dish_characteristics: string[]
  recommendations: [
    {
      wine_name: string
      winery: string
      region: string
      price: float
      match_score: int
      explanation: string
      purchase_links: string[]
    }
  ]
  user_rating: string (loved | disliked | null)
}
```

---

## MVP Scope

**In Scope for MVP:**
- User registration and authentication
- Basic preference setting (wine type, budget)
- Dish photo upload and analysis
- AI-powered wine recommendations (3 per query)
- Pairing explanations
- Purchase links via web search

**Out of Scope for MVP:**
- Social sharing
- Detailed pairing history
- Local store availability
- Wine wishlist
- Advanced dietary filters

---

## Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Dish recognition accuracy | 90% | User confirmation rate |
| Recommendation satisfaction | 80% positive | User feedback ratings |
| Time to recommendation | < 15 seconds | Application monitoring |
| User retention (30-day) | 40% | Analytics |
| Preference completion | 70% | Funnel analytics |
