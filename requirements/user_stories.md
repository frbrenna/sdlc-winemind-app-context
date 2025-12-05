## Personas

1. **Casual Foodie**
   - Enjoys dining out and cooking occasionally
   - Limited wine knowledge; wants simple, trusted recommendations
   - Often in a hurry (e.g., cooking dinner, already at restaurant)

2. **Enthusiast Home Cook**
   - Cooks frequently, experiments with recipes
   - Curious about *why* pairings work; wants to learn
   - Will invest time in setting preferences and reading explanations

3. **Wine Curious Beginner**
   - Knows basic wine types (red/white/rosé) only
   - Budget-sensitive; afraid of “getting it wrong”
   - Wants guidance in simple, non-intimidating language

4. **Wine Enthusiast**
   - Intermediate knowledge of styles/regions
   - Cares about grape variety, region, vintage
   - Wants more control (filters, detailed notes)

5. **Guest User**
   - Lands from a search or link
   - Wants quick value without account creation
   - May convert to registered user if experience is smooth

6. **Registered User**
   - Has an account and saved preferences/history
   - Expects personalization and continuity across sessions

7. **Admin / Content Curator**
   - Oversees quality of dish recognition and recommendations
   - Reviews user feedback and adjusts rules/content

---

## Epics & Features

1. **Epic: Onboarding & Accounts**
   - Feature: Account Creation & Login
   - Feature: Preference Setup & Management
   - Feature: Guest Mode

2. **Epic: Dish Capture & Recognition**
   - Feature: Image Upload / Capture
   - Feature: AI Dish Identification
   - Feature: Dish Confirmation & Correction

3. **Epic: Personalized Pairing Engine**
   - Feature: Preference-Aware Recommendations
   - Feature: Budget & Availability Filtering
   - Feature: Dietary & Restriction Handling

4. **Epic: Real-Time Wine Search & Details**
   - Feature: Web Wine Search Integration
   - Feature: Wine Detail View
   - Feature: Alternative & Similar Wines

5. **Epic: Pairing Explanation & Education**
   - Feature: Pairing Rationale
   - Feature: Learn More / Educational Content

6. **Epic: History, Feedback & Continuous Improvement**
   - Feature: Pairing History
   - Feature: Rating & Feedback
   - Feature: Admin Quality Review Tools

7. **Epic: Non-Functional UX & Performance**
   - Feature: Performance & Responsiveness
   - Feature: Accessibility & Readability

---

## User Stories

### Epic: Onboarding & Accounts

#### US-001: Simple Account Creation

As a **new WineMind user**,  
I want **to create an account with minimal steps**,  
So that **I can save my preferences and pairing history for future use**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] User can sign up with email/password or Google login.
- [ ] Email verification link is sent within 30 seconds of signup.
- [ ] After successful signup, user is automatically logged in.
- [ ] After first login, user is prompted to start the preference setup flow (with option to skip).
- [ ] Account creation flow can be completed in under 60 seconds on a typical broadband/mobile connection.
- [ ] Error messages are clear for duplicate email, weak password, or failed social login.

---

#### US-002: Guest Mode Usage

As a **guest user**,  
I want **to try WineMind without creating an account**,  
So that **I can quickly get a pairing while deciding if I want to register**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] Landing page offers “Continue as guest” alongside “Sign up / Log in”.
- [ ] Guest users can upload a dish photo and receive full pairing recommendations.
- [ ] Guest users are informed that preferences and history will not be saved.
- [ ] At least one unobtrusive prompt invites guest users to create an account after a successful pairing.
- [ ] Guest state persists across at least one pairing session until browser/tab is closed.

---

#### US-003: Manage My Wine Preferences

As a **registered user**,  
I want **to view and edit my wine preferences anytime**,  
So that **I can keep recommendations aligned with my changing tastes and budget**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] Preferences page is accessible from profile/settings.
- [ ] User can adjust: preferred wine colors (red/white/rosé/sparkling), sweetness, body, and budget range.
- [ ] User can toggle dietary/restriction tags (e.g., vegan-friendly, low sulfites) if supported.
- [ ] Changes are saved immediately and applied to the next recommendation request.
- [ ] User can reset preferences to default in a single action (with confirmation).
- [ ] UI provides brief descriptions/tooltips explaining each preference dimension.

---

#### US-004: Preference Wizard During Onboarding

As a **wine curious beginner**,  
I want **a simple guided preference wizard during onboarding**,  
So that **I can get personalized recommendations without needing wine knowledge**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] Wizard contains 5–7 steps using simple visuals (e.g., sliders, images) rather than technical terms.
- [ ] Wizard gathers at least: wine color preferences, sweetness tolerance, body preference, budget range.
- [ ] Each step has a “Skip” option and a “Back” option.
- [ ] At the end of the wizard, a brief summary of chosen preferences is shown with the ability to adjust.
- [ ] Completion of the wizard persists to the user profile.
- [ ] If the wizard is skipped, user can access it later from settings as “Set my preferences”.

---

### Epic: Dish Capture & Recognition

#### US-010: Upload or Capture Dish Image

As a **casual foodie**,  
I want **to quickly upload or take a photo of my meal**,  
So that **WineMind can identify the dish without me typing details**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] User can select a photo from device gallery or use camera capture (where supported).
- [ ] Supported image formats include at least JPEG and PNG.
- [ ] System displays upload progress and handles failures (e.g., network error) with retry option.
- [ ] Maximum file size is enforced and communicated with a clear error if exceeded.
- [ ] After successful upload, user is taken to a “Detecting your dish…” loading state.

---

#### US-011: AI Dish Identification & Confirmation

As a **WineMind user**,  
I want **the app to recognize my dish from the photo and let me confirm or correct it**,  
So that **pairings are accurate to what I’m actually eating**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] After image upload, the system proposes a primary dish name within 5 seconds for common dishes.
- [ ] System displays 2–4 alternative dish labels if confidence is low or ambiguous.
- [ ] User can manually edit the dish name or choose from suggested alternatives.
- [ ] If the system cannot recognize the dish, it prompts user to type a brief description instead.
- [ ] User can proceed to pairing only after confirming or providing a dish description.
- [ ] Confirmed dish label is stored with the pairing request for future history and analysis.

---

#### US-012: Handle Poor or Partial Images

As a **user in a real-world setting**,  
I want **WineMind to still work with imperfect photos**,  
So that **I can get recommendations even when the dish is partially visible or lighting is bad**.

**Priority:** Should

**Acceptance Criteria:**
- [ ] If confidence is low due to image quality, the UI indicates “Not sure” and asks for a short text confirmation.
- [ ] User is prompted with clarifying questions (e.g., “Is this chicken, beef, or vegetarian?”) when helpful.
- [ ] System does not block pairing if photo quality is poor; it falls back to user-described dish.
- [ ] All fallback flows still lead into the standard pairing recommendation step.

---

### Epic: Personalized Pairing Engine

#### US-020: Generate Personalized Wine Pairing

As a **registered user**,  
I want **WineMind to recommend wines that reflect my taste preferences**,  
So that **I receive pairings I’m more likely to enjoy**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] Pairing engine uses at least: confirmed dish type, user taste preferences, and budget range.
- [ ] If no preferences exist (guest user), system uses sensible defaults and labels recommendations as “Based on general pairing rules”.
- [ ] For each request, at least 3 wine recommendations are returned when available.
- [ ] Recommendations respect user’s max budget setting and never exceed it.
- [ ] If budget constraints severely limit options, user is informed and can temporarily adjust budget for this session.

---

#### US-021: Budget-Sensitive Recommendations

As a **budget-conscious user**,  
I want **to set a price range and see wines that fit it**,  
So that **I don’t waste time on recommendations I can’t afford**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] User can specify preferred price range globally in preferences (e.g., per bottle).
- [ ] At pairing time, user can optionally override budget just for that request.
- [ ] All recommended wines display an estimated price in the user’s selected currency, when known.
- [ ] At least 90% of displayed prices are within 10% of the actual online retail price (to be measured).
- [ ] If no wines are found within the specified range, user is shown a clear message and can expand the range with one tap.

---

#### US-022: Dietary & Restriction-Aware Pairing

As a **user with dietary restrictions**,  
I want **wine recommendations that respect my constraints when possible**,  
So that **I can enjoy pairings without compromising my diet or beliefs**.

**Priority:** Should

**Acceptance Criteria:**
- [ ] Preferences allow user to indicate at least one supported restriction (e.g., vegan-friendly) if data is available from sources.
- [ ] When restriction toggles are enabled, recommendations are filtered to matching wines where data is available.
- [ ] If the system cannot guarantee a restriction (e.g., unknown vegan status), it clearly labels wines as “status unknown”.
- [ ] If no wines satisfy the restriction, system explicitly explains this and offers standard recommendations as a separate section (if allowed).

---

### Epic: Real-Time Wine Search & Details

#### US-030: Real-Time Wine Availability Search

As a **WineMind user ready to buy**,  
I want **recommendations to be based on wines currently available online**,  
So that **I can easily purchase the wines I’m shown**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] For each recommendation, system queries at least one online source for current availability.
- [ ] Each recommended wine card indicates “In stock” or “Check availability” with a timestamp of last check.
- [ ] At least 3 available wines are displayed per pairing request when possible.
- [ ] Clicking a recommendation opens a product page link in a new tab (or in-app web view on mobile).
- [ ] If availability checks fail, system falls back to generic recommendations and clearly states that availability could not be verified.

---

#### US-031: View Wine Details

As a **wine enthusiast**,  
I want **to see detailed information about each recommended wine**,  
So that **I can understand style, region, and key characteristics before choosing**.

**Priority:** Should

**Acceptance Criteria:**
- [ ] Tapping or clicking a wine card opens a detailed view/modal.
- [ ] Detailed view shows at least: grape variety, region/country, style (e.g., full-bodied red), approximate alcohol %, and serving temperature range when available.
- [ ] User can see tasting notes in clear, non-technical language.
- [ ] The pairing rationale for that specific wine and dish appears in this view (or links to explanation section).
- [ ] A link or button takes user to an online store page if available.

---

#### US-032: Suggest Alternatives & Similar Wines

As a **user who can’t find a specific bottle**,  
I want **WineMind to suggest similar alternatives**,  
So that **I can still get a good pairing even if the exact wine isn’t available locally**.

**Priority:** Could

**Acceptance Criteria:**
- [ ] For each recommended wine, a “Show similar wines” option is available.
- [ ] Similar wines are selected based on key attributes (grape, style, body, sweetness, region where possible).
- [ ] If no close matches are found, user is informed and given broader suggestions (e.g., “Any Chianti Classico”).
- [ ] Alternatives maintain the user’s price range ± a small configurable tolerance (e.g., 10–20%).

---

### Epic: Pairing Explanation & Education

#### US-040: Understand Why a Wine Pairs With My Dish

As an **enthusiast home cook**,  
I want **a clear explanation of why each wine matches my dish**,  
So that **I can learn and feel confident in the pairing**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] Each recommended wine includes a short pairing explanation (1–3 sentences) in accessible language.
- [ ] Explanations reference key aspects of the dish (e.g., sauce richness, spice level, fattiness).
- [ ] Explanations avoid heavy jargon and target ~8th grade reading level.
- [ ] At least 70% of users in feedback report “I learned something new” for explanations (to be measured via survey).
- [ ] User can expand to see more detail (e.g., “Learn more about this pairing”) without cluttering the default view.

---

#### US-041: Learn General Pairing Principles

As a **wine curious beginner**,  
I want **bite-sized educational content about general pairing rules**,  
So that **I gradually understand how to choose wines myself**.

**Priority:** Should

**Acceptance Criteria:**
- [ ] An optional “Learn wine pairing” section or tooltip is accessible from the main pairing results screen.
- [ ] Content includes short, scannable explanations (e.g., “Why acidity matters”, “Pairing with spicy food”).
- [ ] Educational content is contextual to the user’s dish type when possible (e.g., fish, steak, spicy Asian).
- [ ] Users can close or hide educational overlays and not be forced to read them.
- [ ] Engagement metrics (e.g., clicks on “Learn more”) are trackable for future optimization.

---

### Epic: History, Feedback & Continuous Improvement

#### US-050: View My Pairing History

As a **registered user**,  
I want **to see a history of my past pairings**,  
So that **I can revisit wines I liked and learn from previous choices**.

**Priority:** Should

**Acceptance Criteria:**
- [ ] History view lists past pairing sessions with date, dish name, and number of wines suggested.
- [ ] Selecting a past session shows the recommended wines, pairing explanations, and any user ratings.
- [ ] User can mark specific wines as “Favorites” from history.
- [ ] History is sortable by date, dish type, or rating.
- [ ] Guest users are informed that history is not saved unless they create an account.

---

#### US-051: Rate Recommendations

As a **WineMind user**,  
I want **to rate how helpful each recommendation was**,  
So that **the system can improve future suggestions for me and others**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] Each wine recommendation can be rated with at least a simple scale (e.g., thumbs up/down or 1–5 stars).
- [ ] A “Rate this pairing” prompt appears after viewing or closing the pairing details, but is skippable.
- [ ] Ratings are stored with context: wine, dish, timestamp, and user (if logged in).
- [ ] User can change their rating later from the history view.
- [ ] Aggregated rating scores are available for admin review.

---

#### US-052: Admin Review of Low-Performing Recommendations

As an **admin/content curator**,  
I want **to review patterns of poor ratings and recognition issues**,  
So that **I can tune the system and improve overall quality**.

**Priority:** Should

**Acceptance Criteria:**
- [ ] Admin view lists pairings with low average ratings and high volume of usage.
- [ ] Admin can see examples of dish photos/descriptions associated with poor ratings (with privacy safeguards).
- [ ] Admin can export data (e.g., CSV) of ratings and their attributes for offline analysis.
- [ ] Admin can flag specific rule sets or model outputs for investigation (e.g., “spicy curry + Chardonnay”).
- [ ] Changes made by admins (e.g., rule tweaks) are logged with timestamps for auditability.

---

### Epic: Non-Functional UX & Performance

#### US-060: Fast Pairing Experience

As a **busy user cooking or eating**,  
I want **WineMind to respond quickly to my photo upload**,  
So that **I don’t have to wait long to get my pairing**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] For common dishes and standard image sizes, dish recognition completes within 5 seconds in at least 90% of cases (excluding network latency outside our control).
- [ ] While processing, a clear loading state with progress or “Working on your pairing…” text is shown.
- [ ] User can cancel a pairing request if it takes too long and retry.
- [ ] If processing exceeds a threshold (e.g., 10 seconds), system notifies user and offers retry or manual input.

---

#### US-061: Accessible & Readable UI

As a **user of varying ages and abilities**,  
I want **the pairing explanations and UI to be easy to read and navigate**,  
So that **I can comfortably use WineMind on any device**.

**Priority:** Must

**Acceptance Criteria:**
- [ ] Text for critical content (dish name, wine names, key explanations) meets WCAG AA contrast ratios.
- [ ] Font sizes are responsive and readable on mobile and desktop without zoom.
- [ ] All key actions (upload, confirm dish, view recommendations) are accessible via keyboard navigation.
- [ ] Images and controls include descriptive alt text or ARIA labels where appropriate.
- [ ] Explanations are written to an 8th grade reading level or simpler, excluding unavoidable proper nouns and wine region names.

---

These user stories align with the provided business requirements (dish recognition, personalized recommendations, real-time search, and pairing explanations) and the SDLC/user story format specified in the context documents.