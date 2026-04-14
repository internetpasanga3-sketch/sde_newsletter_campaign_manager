# SDE Project - Spring Boot Newsletter & Email Campaign Manager (MongoDB + JWT)

A secure backend REST API to manage **subscriber lists**, **subscribers**, **email templates**, and **email campaigns**.
Supports **send now** / **scheduled campaigns**, **tracking (open/click)**, **unsubscribe**, and **analytics**.

---

## Tech Stack
- **Java 17**
- **Spring Boot 3**
- **Spring Security**
- **JWT Authentication**
- **BCrypt Password Hashing**
- **MongoDB**
- **Spring Data MongoDB**
- **Spring Scheduler**
- **JavaMailSender (SMTP - Gmail)**
- **Swagger / OpenAPI**
- **Maven**

---

## Roles & Access
### Roles
- `ADMIN`
- `USER` (optional usage)

### Access Rules
- **ADMIN**: Manage lists, subscribers, templates, campaigns, analytics
- **USER**: Create/view campaigns (if enabled)

> Note: In this project, tracking endpoints are public: `/t/**`

---

## Features

### 1) Authentication & Authorization
- JWT-based login
- BCrypt hashed passwords
- Role-based access using Spring Security

### 2) List Management (ADMIN)
- Create, update, delete subscriber lists
- View all lists

### 3) Subscriber Management (ADMIN)
- Add subscribers
- Search subscribers by email
- Update subscriber status

Subscriber Status:
- `ACTIVE`
- `UNSUBSCRIBED`
- `BOUNCED`

### 4) Template Management (ADMIN)
- Create, update, delete templates
- View templates

Template placeholders supported:
- `{{firstName}}`
- `{{email}}`
- `{{unsubscribeUrl}}`
- `{{click}}` (used to track link clicks)

### 5) Campaign Management (ADMIN/USER)
- Create campaign
- Schedule campaign
- Send campaign now
- Pause / cancel campaign
- View campaign history

Campaign Status:
- `DRAFT`
- `SCHEDULED`
- `SENDING`
- `SENT`
- `PAUSED`
- `CANCELLED`

### 6) Sending Engine (System)
- Scheduled processing via Spring Scheduler (every 30 seconds)
- Sends emails in batches (example batch size: 50 per run)
- Creates per-recipient `Message` records for tracking and history

Message Status:
- `QUEUED`
- `SENT`
- `FAILED`
- `BOUNCED`

### 7) Tracking & Unsubscribe (Public)
- Open Tracking Pixel
- Click Tracking Redirect
- Unsubscribe (secure token-based)

### 8) Analytics
- Campaign analytics: total, sent, failed, opened, clicked, openRate, clickRate

---

## API Documentation (Swagger)
After running:
- Swagger UI: `http://localhost:8080/swagger-ui/index.html`

---

## API Endpoints

### Auth (Public)
- `POST /api/auth/register`
- `POST /api/auth/login`

### Lists (ADMIN)
- `POST /api/lists`
- `PUT /api/lists/{id}`
- `DELETE /api/lists/{id}`
- `GET /api/lists`

### Subscribers (ADMIN)
- `POST /api/subscribers`
- `PATCH /api/subscribers/{id}/status?status=ACTIVE`
- `GET /api/subscribers/search?email=gmail&page=0&size=10`

### Templates (ADMIN)
- `POST /api/templates`
- `PUT /api/templates/{id}`
- `DELETE /api/templates/{id}`
- `GET /api/templates`

### Campaigns (ADMIN/USER)
- `POST /api/campaigns`
- `POST /api/campaigns/{id}/schedule?time=2026-04-14T20:30:00`
- `POST /api/campaigns/{id}/send-now`
- `POST /api/campaigns/{id}/pause`
- `POST /api/campaigns/{id}/cancel`
- `GET /api/campaigns`

### Tracking (Public)
- `GET /t/open/{trackingId}.png`
- `GET /t/click/{trackingId}?url=https://example.com`
- `GET /t/unsubscribe/{token}`

### Analytics
- `GET /api/analytics/campaigns/{campaignId}`

---

## Setup & Run (Local)

### Prerequisites
- Java 17
- Maven
- MongoDB running locally
- Gmail App Password (recommended)

### MongoDB
Default database:
- `mongodb://localhost:27017/newsletter_db`

### Configure Gmail SMTP
Update `src/main/resources/application.yml`:
- `spring.mail.username`
- `spring.mail.password` (use **App Password**, not your normal Gmail password)

### Run
```bash
mvn clean install
mvn spring-boot:run
