# TaskPulse | Modern Smart To-Do List Application

A full-stack, responsive, and modern To-Do List and productivity web application built with **React.js**, **Tailwind CSS**, **Node.js with Express.js**, and **MySQL**.

![TaskPulse Architecture](https://img.shields.io/badge/Frontend-React%2019%20%2B%20Tailwind%20CSS-61dafb?style=flat-square)
![Backend](https://img.shields.io/badge/Backend-Node.js%20%2B%20Express-339933?style=flat-square)
![Database](https://img.shields.io/badge/Database-MySQL%208.0-4479A1?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

---

## 🌟 Features Overview

### 1. Task Management (CRUD)
- **Add New Tasks**: Title, rich description, due date picker with quick presets (+Today, +Tomorrow, +1 Week), priority selector, and category picker.
- **Edit Tasks**: Instant in-place editing with pre-filled inputs and validation.
- **Delete Tasks**: Safe deletion with custom confirmation modal to prevent accidental data loss.
- **Instant Status Toggle**: Checkbox toggle between `Pending` and `Completed` with smooth UI feedback.
- **Detailed Task View**: Inspect comprehensive metadata including exact creation and update timestamps.

### 2. Rich Task Details
Each task stores and displays:
- **Title**: Required string (up to 255 chars with validation).
- **Description**: Detailed task context or markdown notes.
- **Due Date**: Formatted date with relative urgency indicators (`Due Today`, `Due Tomorrow`, `Overdue by X days`, `In X days`).
- **Priority Levels**: `High` (Rose), `Medium` (Amber), `Low` (Blue).
- **Categories**: Dynamic category tags (`Work`, `Development`, `Personal`, `Finance`, `Health`, `Study`, or Custom).
- **Status**: `Pending` or `Completed`.

### 3. Interactive Dashboard & Analytics
- **KPI Metrics Cards**:
  - Total Tasks counter
  - Pending Tasks counter
  - Completed Tasks counter with completion percentage
  - Overdue Tasks counter with attention pulse badge
- **Productivity Progress Bar**: Visual progress indicator calculating real-time completion velocity.
- **Analytics & Insights View**:
  - Priority distribution breakdown with percentage indicators.
  - Category distribution with workload counters.
  - Overall productivity and health summary.

### 4. Advanced Filtering, Sorting & Search
- **Instant Search**: Search tasks in real-time across titles and descriptions (Press `/` anywhere to focus search).
- **Priority Filtering**: Filter by `All`, `High`, `Medium`, or `Low`.
- **Status Filtering**: Filter by `All`, `Pending`, `Completed`, or `Overdue`.
- **Category Filtering**: Filter tasks by clicking categories in the sidebar.
- **Sorting Options**:
  - Due Date (Soonest first)
  - Due Date (Latest first)
  - Priority (High to Low)
  - Title (A → Z)
  - Created Date (Newest first)
- **View Switcher**: Toggle seamlessly between **Grid Cards View** and **Compact List View**.

### 5. UI/UX Design & Theme
- **Modern & Minimal Interface**: Clean typography, glassmorphism headers, subtle borders, and smooth micro-interactions.
- **Dark & Light Mode**: Persisted theme preferences with smooth transition and icon rotation.
- **Responsive Layout**: Collapsible sidebar navigation with mobile drawer backdrop and responsive grid layouts.
- **Toast Notifications**: Interactive toast system for actions (created, updated, deleted, status toggled).
- **Database Status Indicator**: Live status indicator displaying active MySQL connection status and schema info.

---

## 🏗️ Project Architecture

```
smart-todo-app/
├── package.json               # Root scripts (run client & server together)
├── start-app.js               # Cross-platform concurrent runner script
├── server/                    # Node.js + Express Backend
│   ├── package.json
│   ├── .env                   # Database and port settings
│   ├── .env.example
│   ├── scripts/
│   │   └── init_db.sql        # Standalone MySQL schema and seed data
│   └── src/
│       ├── index.js           # Server entry point & CORS
│       ├── config/
│       │   └── db.js          # MySQL connection pool, auto-table creation & seeder
│       ├── controllers/
│       │   └── taskController.js # CRUD & analytics controller
│       ├── routes/
│       │   └── taskRoutes.js  # API route definitions
│       └── middleware/
│           ├── validator.js   # Request validation middleware
│           └── errorHandler.js# Error handling middleware
└── client/                    # React + Vite + Tailwind CSS Frontend
    ├── package.json
    ├── vite.config.js         # Vite configuration with /api proxy
    ├── tailwind.config.js     # Tailwind design tokens and dark mode
    ├── index.html
    └── src/
        ├── main.jsx           # React DOM root with Context Providers
        ├── App.jsx            # Main application layout
        ├── index.css          # Tailwind base & custom scrollbar
        ├── context/
        │   ├── ThemeContext.jsx # Dark/Light mode provider
        │   └── TaskContext.jsx  # Task state, filters & modals
        ├── services/
        │   └── api.js         # API fetch client
        └── components/
            ├── Sidebar.jsx    # Collapsible sidebar with navigation & categories
            ├── Header.jsx     # Search bar, shortcuts, theme toggle, +New Task
            ├── DashboardStats.jsx # KPI cards & progress bar
            ├── AnalyticsView.jsx  # Priority & category breakdown charts
            ├── TaskList.jsx   # Filter toolbar, sort selector & card grid
            ├── TaskCard.jsx   # Interactive task card (Grid/List mode)
            ├── TaskModal.jsx  # Create & Edit task dialog
            ├── TaskDetailModal.jsx # Full task details inspection
            ├── DeleteConfirmModal.jsx # Delete confirmation prompt
            ├── DatabaseStatusModal.jsx# MySQL connection details modal
            └── Toast.jsx      # Animated toast notifications
```

---

## 🗄️ MySQL Database Setup

The backend automatically creates the `todo_app` database and the `tasks` table on startup, and automatically seeds sample tasks if the table is empty.

### Table Schema (`tasks`):
```sql
CREATE DATABASE IF NOT EXISTS `todo_app`
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;

USE `todo_app`;

CREATE TABLE IF NOT EXISTS `tasks` (
  `id` INT AUTO_INCREMENT PRIMARY KEY,
  `title` VARCHAR(255) NOT NULL,
  `description` TEXT NULL,
  `due_date` DATE NULL,
  `priority` ENUM('Low', 'Medium', 'High') NOT NULL DEFAULT 'Medium',
  `category` VARCHAR(100) NOT NULL DEFAULT 'General',
  `status` ENUM('Pending', 'Completed') NOT NULL DEFAULT 'Pending',
  `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  INDEX `idx_status` (`status`),
  INDEX `idx_priority` (`priority`),
  INDEX `idx_category` (`category`),
  INDEX `idx_due_date` (`due_date`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

---

## 🚀 Getting Started

### Prerequisites
- **Node.js** (v18 or higher) & **npm**
- **MySQL Server** (8.0 or compatible, e.g. XAMPP, MySQL Workbench, or Windows MySQL80 service)

### Step 1: Clone or Navigate to Directory
```bash
cd "d:\Antigravity Projects"
```

### Step 2: Configure Backend Environment
Open `server/.env` and update credentials to match your MySQL setup:
```env
PORT=5000
NODE_ENV=development

# MySQL Database Configuration
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=root123
DB_NAME=todo_app
```

### Step 3: Run the Complete Application
You can start both the backend server and frontend with one single command:
```bash
npm run dev
```

Or run each service individually:
- **Start Backend**:
  ```bash
  cd server
  npm run dev
  ```
  Runs at: `http://localhost:5000`

- **Start Frontend**:
  ```bash
  cd client
  npm run dev
  ```
  Runs at: `http://localhost:5173`

---

## 📡 REST API Reference

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/api/health` | Health check & MySQL status |
| `GET` | `/api/tasks` | Get all tasks (supports query filters: `search`, `priority`, `status`, `category`, `sort`) |
| `GET` | `/api/tasks/stats` | Dashboard statistics (total, completed, pending, overdue, priorities, categories) |
| `GET` | `/api/tasks/db-status` | Database driver, host, port, database status |
| `GET` | `/api/tasks/:id` | Get details of a single task |
| `POST` | `/api/tasks` | Create a new task (body: `title`, `description`, `due_date`, `priority`, `category`, `status`) |
| `PUT` | `/api/tasks/:id` | Update an existing task |
| `PATCH` | `/api/tasks/:id/toggle` | Quick toggle task status between Pending and Completed |
| `DELETE` | `/api/tasks/:id` | Delete a task |
| `POST` | `/api/tasks/seed` | Reset and re-seed sample database records |

---

## 🛡️ Resilience & Fault Tolerance
- Automatic fallback mechanism: If MySQL is stopped or unreachable during development, the backend activates a smooth local storage engine so your application continues working without crashing.
- Live database status pill badge in the sidebar navigation informs you of current MySQL connectivity at a glance.
- Built-in input validation prevents empty titles and enforces character length limits.
- Fully typed responsive Tailwind layouts optimized for mobile, tablet, and desktop viewports.
#   t o d o  
 