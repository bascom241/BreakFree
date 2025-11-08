# Quitting App 🚪 — Backend Requirements & Implementation Plan

**Purpose:**  
This document defines the backend requirements, APIs, and data models for the **Quitting App 🚪**.  
The app helps Muslims avoid explicit content by **detecting visits to such sites** and **opening the app interface** with **trusted scholar reminders**, rather than blocking access.

---

## Table of Contents
1. Summary / Goals  
2. Core Features (Backend)  
3. System Architecture Overview  
4. Data Models  
5. API Endpoints  
6. Browser-extension / Detection Flow  
7. Push Notification System  
8. Admin Panel  
9. Authentication & Security  
10. Metrics & Privacy  
11. DevOps  
12. Testing & QA  
13. Roadmap  
14. Example Flow  

---

## 1. Summary / Goals
- Detect explicit websites and trigger reminders.  
- Provide APIs for:
  - Authentication (guest or registered users)
  - Reminder content (text/audio/video)
  - Explicit site list synchronization  
  - User streaks, progress, and preferences  
  - Admin content upload & moderation  
- Ensure strong privacy and security.  

---

## 2. Core Features (Backend)
- **Auth:** JWT-based login, signup, and guest sessions.  
- **Profile:** Store language, timezone, and notification settings.  
- **Reminders:** Scholar content (verified), with type and category.  
- **Detection:** List of explicit domains for extensions.  
- **Event Logging:** Track detection and reminder display events.  

---

## 3. System Architecture Overview
- **Frontend:** Mobile + Web app.  
- **Extension:** Detects explicit URLs → calls backend → opens app.  
- **Backend:** Node.js + Express + MongoDB.  
- **CDN:** For audio/video content.  
- **Auth Service:** JWT/Refresh tokens.  

---

## 4. Data Models

### User
\`\`\`js
{
  _id,
  email,
  password,
  role: "user" | "admin",
  preferences: { language, notifications },
  streak: Number
}
\`\`\`

### Reminder
\`\`\`js
{
  _id,
  title,
  bodyText,
  type: "text" ,
  scholar: { name, verified },

  tags: [String]
}
\`\`\`

### ExplicitSite
\`\`\`js
{
  domain,
  category,
  severity,
  updatedAt
}
\`\`\`

### Event
\`\`\`js
{
  userId,
  eventType,
  site,
  timestamp
}
\`\`\`

---

## 5. API Endpoints
| Method | Endpoint | Description |
|--------|-----------|-------------|
| GET | /api/v1/reminders/random | Fetch a random scholar reminder |
| GET | /api/v1/sites/list | Return explicit site list |
| POST | /api/v1/auth/signup | Register a new user |
| POST | /api/v1/auth/login | User login |
| POST | /api/v1/events | Log detection or reminder event |

---

## 6. Browser-extension / Detection Flow
1. User visits a website.  
2. Extension checks against the explicit site list.  
3. If a match is found:  
   - Sends event → `/api/v1/events`  
   - Fetches reminder → `/api/v1/reminders/random`  
   - Opens Quitting App with the reminder.  

---

## 7. Push Notification System
- Firebase Cloud Messaging (FCM) integration.  
- Triggers motivational notifications after repeated visits or streak loss.  

---

## 8. Admin Panel
- Upload/manage reminders.  
- Review flagged content.  
- Manage explicit site lists.  

---

## 9. Authentication & Security
- JWT tokens (access + refresh).  
- HTTPS only.  
- bcrypt password hashing.  
- Role-based access control (RBAC).  

---

## 10. Metrics & Privacy
- Anonymous tracking for insights (no personal data stored).  
- Optional user stats: total triggers, reminders viewed, streaks.  

---

## 11. DevOps
- Deploy on Render / Vercel / Railway.  
- MongoDB Atlas for database.  
- CI/CD with GitHub Actions.  

---

## 12. Testing & QA
- Unit tests: Mocha/Jest.  
- Integration tests for API responses.  
- Load testing with Artillery.  

---

## 13. Roadmap
- AI-based trigger classification.  
- Daily motivational reminders.  
- Offline mode.  

---

## 14. Example Flow
1. User opens an explicit site.  
2. Browser extension detects URL → triggers backend.  
3. Backend responds with reminder.  
4. App interface opens → displays content.  
5. User optionally reflects or marks reminder as helpful.  

---
