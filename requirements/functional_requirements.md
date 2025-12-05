# WineMind – Functional Requirements Document

## 1. Executive Summary

WineMind is a web application that helps users discover ideal wine pairings for their meals using AI. Users can upload a photo of their dish or describe it, configure their taste and budget preferences, and receive personalized, real-time wine recommendations with clear pairing explanations and purchase options.

This Functional Requirements Document translates the business requirements and user stories into implementable system behavior. It focuses on:

- AI-powered dish recognition from images  
- Preference-driven, personalized recommendations  
- Real-time web search for available wines  
- Clear educational pairing explanations  
- Core account, profile, and session flows needed to support these capabilities  

Priorities are assigned as High, Medium, or Low to guide implementation sequencing.

---

## 2. Core Functional Requirements

### FR-001: User Registration & Login

**Description:**  
The system shall allow users to create and access accounts using email/password and Google OAuth, enabling persistent storage of preferences and pairing history.

**Details:**
- Support sign-up with:
  - Email and password
  - Google social login (OAuth 2.0)
- Send email verification after email-based sign-up.
- Allow secure login with:
  - Email/password
  - Google social login
- Maintain authenticated sessions using secure tokens.
- Provide logout functionality that invalidates active session tokens.

**Priority:** High

---

### FR-002: Guest Mode Access

**Description:**  
The system shall allow users to use the application in a guest mode without registration, with limited persistence of data.

**Details:**
- Allow entry into the app without account creation.
- Enable full pairing flow (image upload, preferences, recommendations) during a guest session.
- Store guest session data only for the duration of the browser session.
- Provide a clear upgrade path from guest to registered user, allowing optional migration of current session data to a new account.

**Priority:** High

---

### FR-003: Email Verification

**Description:**  
The system shall send verification emails for new email/password accounts and validate verification tokens.

**Details:**
- Generate a time-limited verification token on sign-up.
- Send a verification link to the user’s email address within 30 seconds of registration.
- When the link is clicked, mark the email as verified if the token is valid and not expired.
- Provide a way for users to request resending of the verification email.
- Prevent unverified users from accessing features that require a fully verified account (configurable).

**Priority:** Medium

---

### FR-004: Password Management

**Description:**  
The system shall allow users with email/password accounts to manage their passwords.

**Details:**
- Allow users to set a strong password during registration.
- Provide a “Forgot Password” flow:
  - Accept email
  - Send reset link with time-limited token
  - Allow user to set a new password upon valid token usage
- Provide an in-app “Change Password” action for logged-in users.

**Priority:** Medium

---

### FR-005: User Preference Onboarding Wizard

**Description:**  
The system shall present a preference wizard during onboarding to capture wine-related preferences.

**Details:**
- After account creation (and optionally for first-time guests), show a wizard with 5–7 simple steps/questions.
- Collect at least:
  - Preferred wine color(s): red/white/rosé/sparkling/“no preference”
  - Sweetness tolerance: e.g., very dry → very sweet (slider or discrete options)
  - Body preference: light/medium/full
  - Budget range per bottle (min–max or tiered options)
- Provide visual aids (images, sliders, short descriptions) to simplify choices.
- Allow users to skip the wizard and use default settings.
- Briefly explain how each preference affects recommendations (e.g., tooltips or short text).

**Priority:** High

---

### FR-006: Preference Storage & Management

**Description:**  
The system shall persist wine preferences and allow users to edit them at any time.

**Details:**
- Store user preferences in a profile associated with their account or temporary guest session.
- Allow users to access and edit preferences from a “Profile” or “Preferences” section.
- Immediately apply updated preferences to subsequent recommendations.
- Support additional preference attributes over time (e.g., preferred regions, varietals, organic/biodynamic preference, alcohol level).

**Priority:** High

---

### FR-007: Dietary & Restriction Preferences

**Description:**  
The system shall allow users to define dietary restrictions and constraints that influence wine recommendations.

**Details:**
- Enable users to indicate restrictions such as:
  - Vegan/vegetarian compatibility
  - Sulfite sensitivity (informational guidance; cannot enforce fully)
  - Organic/natural preference
- Ensure the recommendation engine can factor these flags into filtering and ranking of wines where such metadata is available.
- Display clear disclaimers where restrictions cannot be guaranteed due to data limitations.

**Priority:** Medium

---

### FR-008: Dish Image Upload

**Description:**  
The system shall allow users to upload a photo of their meal for AI-based dish recognition.

**Details:**
- Support image upload from local file (desktop) and device camera/gallery (mobile-capable UI).
- Accept common formats (e.g., JPEG, PNG, HEIC where supported).
- Enforce maximum file size and provide user feedback if exceeded.
- Show upload progress and error messages for failed uploads.
- Allow users to replace or remove an uploaded image before confirmation.

**Priority:** High

---

### FR-009: Dish Recognition via AI

**Description:**  
The system shall analyze the uploaded dish image using an AI model to identify the dish type and key attributes relevant to wine pairing.

**Details:**
- Invoke an AI image analysis service (via AWS Bedrock agents or similar) upon successful image upload.
- Extract:
  - Dish name or class (e.g., “grilled salmon”, “margherita pizza”)
  - Key ingredients or dominant components (protein, sauce, spice level, cooking method)
  - Cuisine type where identifiable (e.g., Italian, Thai, French)
- Return AI results within a target time window (aiming to support the 5-second business requirement).
- Provide a machine-readable structure (e.g., JSON) of recognized attributes for downstream pairing logic.

**Priority:** High

---

### FR-010: Dish Identification Confirmation & Edit

**Description:**  
The system shall present the identified dish to the user for confirmation and allow corrections.

**Details:**
- Display the AI-detected dish name and key attributes to the user.
- Allow the user to:
  - Confirm the suggested dish as-is.
  - Edit the dish name and description (free-text).
  - Select from alternative AI-suggested dish categories where available.
- Use the final, user-confirmed description as the canonical input for pairing.
- Log user corrections to improve AI model performance over time (feedback loop).

**Priority:** High

---

### FR-011: Text-Based Dish Description (Fallback/Alternative)

**Description:**  
The system shall allow users to manually describe their dish instead of or in addition to uploading an image.

**Details:**
- Provide an input field where users can describe their meal (e.g., “spicy Thai green curry with chicken”).
- Allow starting the pairing flow in text-only mode, without image upload.
- Optionally pre-fill or enhance text description with AI findings if both text and image are provided.
- Route text-only descriptions through the same pairing logic as image-derived dish data.

**Priority:** Medium

---

### FR-012: Pairing Request Submission

**Description:**  
The system shall allow users to submit a pairing request using the dish details and current preferences.

**Details:**
- Gather:
  - Confirmed dish information (from image or text)
  - Current user preferences (taste, budget, restrictions)
  - Optional contextual info (occasion, number of guests, season) if supported by UI
- Validate that minimum required information is present before submission.
- Trigger the recommendation pipeline (AI pairing + real-time search) upon submission.
- Provide a loading state while processing the request.

**Priority:** High

---

### FR-013: Wine Pairing Generation (Conceptual)

**Description:**  
The system shall generate a set of candidate wine pairings that conceptually match the dish and user preferences prior to real-time product lookup.

**Details:**
- Use AI/knowledge-based logic to:
  - Determine suitable wine styles, varietals, and regions for the given dish.
  - Incorporate user preferences such as sweetness and body into the pairing concept.
  - Define target attributes for real wines (e.g., “New Zealand Sauvignon Blanc, dry, $15–$25”).
- Produce a structured list of candidate pairing profiles that can be passed to the web search component.

**Priority:** High

---

### FR-014: Real-Time Wine Product Search

**Description:**  
The system shall perform web-based, real-time search to find actual wines matching the candidate pairing profiles and user constraints.

**Details:**
- Query external data sources/APIs/web search for currently available wines.
- Filter and rank wines using:
  - Wine style/varietal/region targets from FR-013.
  - User budget range.
  - Dietary/restriction flags where metadata is available.
- Aim to return at least 3 viable wines per pairing request; if fewer are found, show best-effort results with an explanation.
- Capture price, merchant name, and availability status for each result.

**Priority:** High

---

### FR-015: Wine Result Normalization & Deduplication

**Description:**  
The system shall normalize wine search results into a consistent internal format and remove duplicates.

**Details:**
- Map external attributes (e.g., different API field names) into a unified schema (name, varietal, region, vintage, price, merchant URL).
- Detect and merge duplicate wines returned from multiple sources (e.g., same wine from different merchants).
- Select a primary display record with optional multiple merchant options, if applicable.

**Priority:** Medium

---

### FR-016: Recommendation List Display

**Description:**  
The system shall display a list of recommended wines to the user in a clear, comparable format.

**Details:**
- Show for each wine:
  - Name and producer
  - Varietal and region
  - Vintage (if provided)
  - Price (and currency)
  - Merchant/source and link to purchase
  - Short pairing summary (e.g., “Best for rich cream sauces”)
- Support a minimum of 3 recommended wines per query when available.
- Indicate when data is approximate (e.g., price ranges, limited availability).
- Allow basic sorting (e.g., by price, recommendation rank) and simple filters where practical (e.g., only reds).

**Priority:** High

---

### FR-017: Pairing Explanation per Wine

**Description:**  
The system shall provide a human-readable explanation of why each recommended wine pairs well with the identified dish.

**Details:**
- For each wine, generate a short explanation covering:
  - Key flavor and structure aspects (acidity, tannin, body, sweetness).
  - How these aspects complement or contrast the dish’s main components (fat, spice, acidity, sweetness, umami).
  - Any relevant educational insight (e.g., “Sauvignon Blanc’s high acidity cuts through the richness of goat cheese.”)
- Keep the explanation concise and user-friendly.
- Ensure explanations are specific to the dish and wine, not generic boilerplate.

**Priority:** High

---

### FR-018: Education & “Learn More” Content

**Description:**  
The system shall optionally provide additional educational content about wine pairing principles related to the current recommendation.

**Details:**
- Include an expandable “Why this works” or “Learn more” section.
- Provide general principles illustrated by the current pairing (e.g., “acid vs fat”, “spice and sweetness”, “tannin and protein”).
- Offer simple definitions for technical terms (e.g., “tannin”, “body”, “acidity”).
- Avoid overwhelming the user with overly technical or long content; keep it modular and optional.

**Priority:** Medium

---

### FR-019: Price & Availability Display

**Description:**  
The system shall display current price and availability details for each recommended wine.

**Details:**
- Show:
  - Listed price and currency.
  - Basic availability indicator (e.g., “In stock”, “Low stock”, “Out of stock/Not available” when obtainable).
- Indicate when price or availability cannot be guaranteed and may be approximate.
- Provide a “Check current price” or direct merchant link that opens in a new tab/window.

**Priority:** High

---

### FR-020: Budget Compliance & Alternatives

**Description:**  
The system shall respect the user’s budget preferences and offer alternatives if recommended wines exceed the specified range.

**Details:**
- Prefer wines whose prices fall within the user’s selected budget range.
- If few or no wines meet the budget, provide:
  - At least one lower-cost alternative (if available).
  - A short message explaining limited matches.
- Clearly flag recommendations that exceed the user’s budget, if they are still shown (e.g., “slightly above your budget”).

**Priority:** High

---

### FR-021: Pairing History Storage

**Description:**  
The system shall store historical pairing requests and their recommendations for registered users.

**Details:**
- Save, for each completed pairing:
  - Timestamp
  - Dish description (and image reference if allowed/stored)
  - Active user preferences at the time
  - List of recommended wines (key fields and links)
- Allow users to revisit their past pairings from a “History” section.
- Support re-running a past pairing with updated market data (new search) as an option.

**Priority:** Medium

---

### FR-022: Pairing History Browsing

**Description:**  
The system shall provide a UI for users to browse their pairing history.

**Details:**
- Show a list or card view of previous pairings with:
  - Dish name
  - Date/time
  - Thumbnail of dish image if stored and permitted
- Allow users to:
  - Open a detailed view of a past pairing (including wines and explanations).
  - Re-run the pairing with current preferences and wine availability.
  - Delete individual history items.

**Priority:** Medium

---

### FR-023: Feedback on Recommendations

**Description:**  
The system shall allow users to rate the usefulness of recommendations and optionally provide feedback.

**Details:**
- Enable a simple rating mechanism (e.g., “Helpful / Not helpful” or 1–5 stars) per pairing session or per wine.
- Optionally collect short free-text comments.
- Associate feedback with:
  - User (if logged in)
  - Pairing session and dish
- Store feedback for analytics and to refine algorithms over time.

**Priority:** Medium

---

### FR-024: AI Confidence & Fallback Handling

**Description:**  
The system shall respond gracefully when AI confidence in dish recognition or pairing is low.

**Details:**
- Capture a confidence score from the AI dish recognition.
- When confidence is below a threshold:
  - Explicitly ask the user to confirm or correct the dish.
  - Suggest the user provide additional context via text.
- When pairing confidence is low or data is sparse:
  - Provide “best effort” suggestions with a transparent note.
  - Offer generic but reasonable style-level guidance (e.g., “A crisp white such as…”).

**Priority:** Medium

---

### FR-025: Core Navigation & Layout

**Description:**  
The system shall provide clear navigation between core sections: Home/Pairing, History, Preferences/Profile, and Help.

**Details:**
- Implement a top or side navigation consistent with shadcn/ui guidelines.
- Provide quick access to:
  - New pairing request
  - History
  - Profile/Preferences
  - Help/FAQ
- Ensure users can always start a new pairing from any major page.

**Priority:** Medium

---

### FR-026: Help & Onboarding Hints

**Description:**  
The system shall offer lightweight guidance to first-time users and contextual help during key steps.

**Details:**
- Show brief tooltips or inline hints for:
  - How to take a good dish photo.
  - What preferences mean (e.g., sweetness, body).
  - How budget affects results.
- Provide a dedicated Help/FAQ section with:
  - How WineMind works (at a high level).
  - Limitations and disclaimers about AI and availability.
- Allow users to dismiss onboarding hints and not show them again.

**Priority:** Low

---

### FR-027: Session Management

**Description:**  
The system shall manage user sessions for both authenticated users and guests.

**Details:**
- Maintain a session identifier for each visitor (guest or logged-in).
- Associate in-progress pairing requests with the current session.
- Expire inactive guest sessions after a configurable timeout.
- For logged-in users, restore relevant state (e.g., last used preferences) upon returning, where possible.

**Priority:** Medium

---

### FR-028: Error Handling & User Feedback

**Description:**  
The system shall provide clear, user-friendly error messages and fallback options for common failure scenarios.

**Details:**
- Handle:
  - Image upload failures
  - AI analysis failures or timeouts
  - External wine search errors
- Show:
  - What went wrong in plain language
  - Suggested next steps (e.g., try again, use text description, adjust filters)
- Provide a generic “Something went wrong” message with a retry option when root cause is unknown.

**Priority:** High

---

### FR-029: Logging & Analytics Events (Functional Perspective)

**Description:**  
The system shall emit functional events needed to measure key business metrics.

**Details:**
- Log events for:
  - Account creation and verification
  - Preference wizard completion and updates
  - Image upload and dish confirmation
  - Pairing request submission and completion
  - Recommendation views and click-throughs to merchants
  - Feedback submissions
- Include anonymous identifiers for guest users and user IDs for authenticated users.

**Priority:** Medium

---

### FR-030: Basic Admin/Configuration Panel (Initial Scope)

**Description:**  
The system shall provide a minimal internal admin/configuration interface for tuning certain functional behaviors.

**Details:**
- Restrict access to admin users only.
- Allow configuration of:
  - AI confidence thresholds for dish recognition.
  - Default budget range used when a user skips setup.
  - Feature flags for experimental flows (e.g., extended education content).
- Provide a simple log view or export for pairing sessions and feedback for analysis.

**Priority:** Low

---

These functional requirements collectively describe how WineMind must behave to meet its core business objectives: accurate, personalized, real-time wine pairing with educational explanations, supported by a usable, preference-aware web experience.