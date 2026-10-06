Markdown
# All-in-One Life Dashboard

A full-stack personal management dashboard built with **Flutter Web**, **Node.js (Express)**, and **PostgreSQL**. The application provides real-time monitoring and management of daily reminders, health metrics, and local weather summaries with automated background polling and persistent storage.

---

## Architecture Overview

┌─────────────────────────┐       HTTP / REST       ┌─────────────────────────┐       SQL Queries       ┌─────────────────────────┐
│                         │   (GET, POST, PATCH,    │                         │   (pg Connection Pool)  │                         │
│   Flutter Web Frontend  │ ─────── DELETE) ────────>   Node.js / Express API   │ ──────────────────────>   PostgreSQL Database   │
│   (Dart / Material 3)   │                         │     (Port 5000 / CORS)  │                         │       (relational)      │
└─────────────────────────┘                         └─────────────────────────┘                         └─────────────────────────┘


---

## Tech Stack

* **Frontend:** Flutter Web (Dart), Material 3 Design, `http` package
* **Backend:** Node.js, Express framework, `cors`, `dotenv`
* **Database:** PostgreSQL (using `pg` Connection Pool)
* **Target Environment:** Windows 11, Microsoft Edge / Modern Browsers

---

## Key Features

1. **Live Dashboard Overview:** Aggregated RESTful view (`GET /api/v1/dashboard`) returning weather, health, and reminder data in a unified payload.
2. **Interactive Reminders (Full CRUD):**
   * **Read (`GET`):** Displays bill and event categories with status indicators.
   * **Create (`POST`):** Modal dialog interface for appending new tasks.
   * **Update (`PATCH`):** Instant status toggling with optimistic UI updates.
   * **Delete (`DELETE`):** One-click task removal from PostgreSQL.
3. **Silent Background Auto-Polling:** Automated 30-second polling (`Timer.periodic`) to refresh client data silently without disrupting user interaction.
4. **Resilient Error Handling:** Fallback mechanisms to retain state during temporary backend network interruptions.

---

## Project Structure

```text
dashboard_mobile_app/
├── server.js                   # Express server entry point & RESTful API routes
├── .env                        # PostgreSQL connection environment variables
├── package.json                # Node.js dependencies
└── flutter_application_1/      # Flutter Web application
    ├── pubspec.yaml            # Flutter packages & configuration
    └── lib/
        └── main.dart           # Dashboard UI components & state management

Database Schema (Postgre SQL)

-- Reminders Table
CREATE TABLE reminders (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    category VARCHAR(50) DEFAULT 'bill',
    due_date VARCHAR(100),
    is_completed BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Health Metrics Table
CREATE TABLE health_metrics (
    id SERIAL PRIMARY KEY,
    steps INT DEFAULT 0,
    bp_systolic INT,
    bp_diastolic INT,
    bmi NUMERIC(4,1),
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

Getting Started & Local Setup

Prerequisites

Node.js (v18+)

Flutter SDK (v3.0+)

PostgreSQL Database running locally

1. Environment Configuration
Create a .env file in the root directory:

Code snippet

PORT=5000
DB_USER=postgres
DB_PASSWORD=your_password
DB_HOST=localhost
DB_PORT=5432
DB_NAME=dashboard_db

2. Backend Startup
From the root project directory:

npm install
node server.js

3. Frontend Startup
In a separate terminal pane:

Bash
cd flutter_application_1
flutter run -d edge

Method,Endpoint,Description
GET,/api/v1/dashboard,Fetch aggregated dashboard payload
POST,/api/v1/reminders,Create a new reminder
PATCH,/api/v1/reminders/:id,Toggle reminder completion status
DELETE,/api/v1/reminders/:id,Delete a reminder by ID

