# User Stories: Mobile Banking Application

This document contains example user stories for a mobile banking application. Use these as a reference for format, structure, and level of detail expected in user story artifacts.

---

## Epic: Account Access & Authentication

### US-001: Biometric Login

As a **mobile banking customer**,
I want **to log in using my fingerprint or face recognition**,
So that **I can access my accounts quickly and securely without typing a password**.

**Acceptance Criteria:**
- [ ] User can enable biometric authentication in security settings after initial password login
- [ ] System supports both fingerprint and facial recognition based on device capabilities
- [ ] Biometric login completes within 2 seconds
- [ ] Failed biometric attempt (3 consecutive) falls back to password entry
- [ ] User can disable biometric login at any time from settings
- [ ] Biometric data is stored only on device, never transmitted to servers

**Priority:** High
**Estimated Effort:** M

**Technical Notes:**
- Integrate with device native biometric APIs (iOS Face ID/Touch ID, Android BiometricPrompt)
- Requires secure token storage using device keychain/keystore

---

### US-002: Multi-Factor Authentication Setup

As a **security-conscious customer**,
I want **to set up multi-factor authentication for my account**,
So that **my account has an additional layer of protection against unauthorized access**.

**Acceptance Criteria:**
- [ ] User can choose between SMS, email, or authenticator app as second factor
- [ ] Setup wizard guides user through MFA configuration step by step
- [ ] User can register multiple MFA methods and set a primary
- [ ] Recovery codes are generated and can be downloaded/saved during setup
- [ ] MFA is required for high-risk actions (transfers over $1000, adding new payees)
- [ ] User receives confirmation notification when MFA settings are changed

**Priority:** High
**Estimated Effort:** L

**Dependencies:**
- SMS gateway integration
- TOTP library implementation

---

## Epic: Account Management

### US-003: View Account Dashboard

As a **banking customer**,
I want **to see all my accounts and balances on a single dashboard**,
So that **I can quickly understand my overall financial position**.

**Acceptance Criteria:**
- [ ] Dashboard displays all linked accounts (checking, savings, credit cards, loans)
- [ ] Each account shows current balance and account nickname
- [ ] Total net worth calculation displayed at top of dashboard
- [ ] Balances refresh automatically when dashboard is opened
- [ ] Pull-to-refresh gesture updates all balances
- [ ] Last updated timestamp is visible
- [ ] Accounts can be reordered by drag and drop
- [ ] User can hide accounts from dashboard without closing them

**Priority:** High
**Estimated Effort:** M

**UI/UX Notes:**
- Use card-based layout for each account
- Color-code by account type
- Show positive balances in green, negative in red

---

### US-004: View Transaction History

As a **banking customer**,
I want **to view my transaction history with search and filter options**,
So that **I can find specific transactions and track my spending**.

**Acceptance Criteria:**
- [ ] Transaction list shows last 90 days by default
- [ ] Each transaction displays date, description, amount, and running balance
- [ ] User can filter by date range, transaction type, and amount range
- [ ] Search functionality matches against description and merchant name
- [ ] Pending transactions are visually distinguished from posted transactions
- [ ] User can export transactions to CSV or PDF
- [ ] Infinite scroll loads older transactions on demand
- [ ] Tapping a transaction opens detailed view with category, location, and reference number

**Priority:** High
**Estimated Effort:** L

**Performance Requirements:**
- Initial load of 50 transactions in under 1 second
- Search results return within 500ms

---

## Epic: Money Transfers

### US-005: Transfer Between Own Accounts

As a **banking customer**,
I want **to transfer money between my own accounts instantly**,
So that **I can manage my funds across accounts without waiting**.

**Acceptance Criteria:**
- [ ] User can select source and destination from their linked accounts
- [ ] Available balance is displayed and transfer cannot exceed it
- [ ] Transfers between own accounts are processed immediately
- [ ] Confirmation screen shows all details before final submission
- [ ] Success screen displays confirmation number and updated balances
- [ ] Transfer appears immediately in transaction history for both accounts
- [ ] User can set up recurring transfers with frequency options (weekly, biweekly, monthly)
- [ ] User can cancel scheduled transfers before execution date

**Priority:** High
**Estimated Effort:** M

**Business Rules:**
- No fees for internal transfers
- Daily transfer limit: $50,000

---

### US-006: Send Money to External Accounts

As a **banking customer**,
I want **to send money to accounts at other banks**,
So that **I can pay rent, send money to family, or move funds to external accounts**.

**Acceptance Criteria:**
- [ ] User can add external accounts using routing and account numbers
- [ ] External account verification via micro-deposits (two small deposits to verify)
- [ ] User can save external accounts as payees with nicknames
- [ ] Transfer options include standard (2-3 business days) and expedited (same day)
- [ ] Fee disclosure shown before confirmation for expedited transfers
- [ ] User can schedule future-dated transfers
- [ ] Email/push notification sent when transfer is initiated and completed
- [ ] User can cancel pending transfers before processing cutoff time

**Priority:** High
**Estimated Effort:** L

**Compliance Notes:**
- Requires OFAC screening before processing
- Transaction limits subject to regulatory requirements

---

### US-007: Pay Bills

As a **banking customer**,
I want **to pay my bills directly from the mobile app**,
So that **I can manage all my payments in one place and never miss a due date**.

**Acceptance Criteria:**
- [ ] User can add billers by searching a directory or entering biller details manually
- [ ] Saved billers display in a list with next due date and amount due (if available)
- [ ] User can make one-time or recurring payments
- [ ] Payment scheduling allows selection of specific date or "pay on due date"
- [ ] User can set up autopay with rules (pay minimum, pay full balance, pay fixed amount)
- [ ] Push notification reminders sent 3 days before due date
- [ ] User can view payment history per biller
- [ ] eBills can be received and displayed within the app for supported billers

**Priority:** Medium
**Estimated Effort:** XL

**Integration Requirements:**
- Bill pay network integration
- eBill provider connections

---

## Epic: Card Management

### US-008: Lock and Unlock Debit Card

As a **banking customer**,
I want **to instantly lock my debit card from the app**,
So that **I can prevent unauthorized use if my card is lost or I notice suspicious activity**.

**Acceptance Criteria:**
- [ ] Toggle switch on card details screen to lock/unlock card
- [ ] Lock takes effect within 30 seconds across all payment networks
- [ ] Locked card declines all transactions (POS, ATM, online)
- [ ] User receives push notification confirming lock/unlock action
- [ ] Card can be unlocked instantly when found
- [ ] Recurring payments and subscriptions are not affected by temporary lock
- [ ] Lock status is clearly displayed on dashboard and card details
- [ ] User can add a reason when locking (optional) for personal reference

**Priority:** High
**Estimated Effort:** S

**Technical Notes:**
- Real-time integration with card processor required
- Must handle network latency gracefully

---

## Epic: Notifications & Alerts

### US-009: Customize Transaction Alerts

As a **banking customer**,
I want **to set up custom alerts for account activity**,
So that **I am immediately informed of transactions and can detect fraud quickly**.

**Acceptance Criteria:**
- [ ] User can enable/disable alerts for each account individually
- [ ] Alert types include: all transactions, transactions over threshold, international transactions, card-not-present transactions
- [ ] User can set custom threshold amounts per account
- [ ] Delivery channel options: push notification, SMS, email (multiple can be selected)
- [ ] Alerts are delivered within 60 seconds of transaction posting
- [ ] User can configure quiet hours to suppress non-urgent alerts
- [ ] Low balance alert with user-defined threshold
- [ ] Large deposit alert option for incoming funds

**Priority:** Medium
**Estimated Effort:** M

**Notification Content:**
- Must include: amount, merchant/description, account last 4 digits, timestamp
- Must not include: full account number, available balance (security)

---

## Epic: Customer Support

### US-010: In-App Chat Support

As a **banking customer**,
I want **to chat with customer support directly in the app**,
So that **I can get help with issues without leaving the app or waiting on hold**.

**Acceptance Criteria:**
- [ ] Chat icon accessible from all main screens
- [ ] Initial interaction with AI assistant for common questions
- [ ] Seamless handoff to human agent when AI cannot resolve
- [ ] User is shown estimated wait time for human agent
- [ ] Chat history is preserved and viewable for 90 days
- [ ] User can share screenshots securely within chat
- [ ] Agent can send secure forms and links within chat
- [ ] User can request a transcript via email after chat ends
- [ ] Typing indicators show when agent is responding
- [ ] User receives push notification when agent responds if app is backgrounded

**Priority:** Medium
**Estimated Effort:** XL

**Integration Requirements:**
- AWS Bedrock for AI assistant
- Customer service platform integration
- Secure file upload service

---

## Appendix: Story Template

Use this template when creating new user stories:

```
### US-[ID]: [Title]

As a **[type of user]**,
I want **[goal/desire]**,
So that **[benefit/value]**.

**Acceptance Criteria:**
- [ ] Criterion 1 (specific, testable condition)
- [ ] Criterion 2
- [ ] Criterion 3

**Priority:** [High | Medium | Low]
**Estimated Effort:** [S | M | L | XL]

**Dependencies:** (optional)
- Dependency 1

**Technical Notes:** (optional)
- Note 1

**Business Rules:** (optional)
- Rule 1

**UI/UX Notes:** (optional)
- Note 1
```

---

## Story Sizing Guide

| Size | Description | Typical Duration |
|------|-------------|------------------|
| S | Simple change, single component, minimal testing | 1-2 days |
| M | Moderate complexity, few components, standard testing | 3-5 days |
| L | Complex feature, multiple components, extensive testing | 1-2 weeks |
| XL | Very complex, multiple integrations, significant testing | 2-4 weeks |

---

## Acceptance Criteria Best Practices

When writing acceptance criteria:

1. **Be specific and measurable** — "User can log in" is vague; "Login completes within 2 seconds" is measurable
2. **Cover happy path and edge cases** — Include what happens when things go wrong
3. **Include performance expectations** — Response times, limits, thresholds
4. **Consider security requirements** — Authentication, authorization, data handling
5. **Think about accessibility** — Screen readers, keyboard navigation
6. **Define boundaries** — What's in scope and what's explicitly out of scope
