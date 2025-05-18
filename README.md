# Employee Compensation Forecasting Application

## Overview

A tool to analyze employee compensation, simulate increments, and export insights for HR management.  
**This version includes a backend using SQL for persistent data storage.**

---

## Features

- **Filter and view employee details** by role and location.
- **Group employees by years of experience**.
- **Simulate compensation increments** globally or selectively.
- **Export filtered employee data** to CSV.
- **Persistent employee data storage using SQL database.**

---

## Tools and Technologies

- **Frontend:** React.js, Chart.js
- **Backend:**  SQL database (e.g., SQLite, MySQL, or PostgreSQL)
- **Data Storage:** Employee data stored in a relational SQL database

---

## Setup Instructions

### 1. Clone the repository

### 2. Backend Setup

1. Navigate to the `Backend` directory.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Configure your database connection in `.env` (see `.env.example` for configuration options).
4. Run database migrations or initialize tables:
   ```bash
   npm run migrate
   ```
5. Start the backend server:
   ```bash
   npm start
   ```
   The backend will run on `http://localhost:5000` by default.

### 3. Frontend Setup

1. Navigate to the `Frontend` directory.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the frontend application:
   ```bash
   npm start
   ```
   The frontend will run on `http://localhost:3000` by default.

---

## API Endpoints (Example)

- `GET /api/employees` — Fetch employees (supports filters via query params)
- `POST /api/employees/increment` — Simulate and apply increments
- `GET /api/employees/experience-groups` — Get employees grouped by experience
- `GET /api/employees/export` — Download filtered data as CSV

---

## Code Snippets

### Example: Employee Table Schema (SQL)

```sql
CREATE TABLE employees (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    role TEXT NOT NULL,
    location TEXT NOT NULL,
    experience REAL NOT NULL,
    compensation REAL NOT NULL,
    is_active BOOLEAN NOT NULL DEFAULT 1
);
```

### Example: Fetch Employees (Express Route)

```javascript
// routes/employees.js
const express = require('express');
const router = express.Router();
const db = require('../db');

router.get('/', async (req, res) => {
    const { role, location, includeInactive } = req.query;
    let query = `SELECT * FROM employees WHERE 1=1`;
    let params = [];
    if (role) {
        query += ` AND role = ?`;
        params.push(role);
    }
    if (location) {
        query += ` AND location = ?`;
        params.push(location);
    }
    if (!includeInactive || includeInactive === 'false') {
        query += ` AND is_active = 1`;
    }
    const employees = await db.all(query, params);
    res.json(employees);
});
module.exports = router;
```

---

## User Stories

### 1. Filter and Display Active Employees by Role

- **Feature:** Filter employees by role and location, and toggle active/inactive status.
- **How it Works:**
  - Users fill in the filter form.
  - Data is fetched from the backend and displayed in a table.
  - A bar chart compares average compensation across locations.

### 2. Group Employees by Years of Experience

- **Feature:** View a count of employees in experience ranges.
- **How it Works:**
  - Backend groups data into experience ranges (e.g., 0–1, 1–2 years).
  - Users can optionally group data by location or role.

### 3. Simulate Compensation Increments

- **Feature:** Apply global or custom increments to employee compensation.
- **How it Works:**
  - Users input a global increment percentage.
  - Backend calculates and returns updated compensations.
  - Updated compensations are displayed alongside current compensations.

### 4. Download Filtered Employee Data

- **Feature:** Export filtered employee data to a CSV file.
- **How it Works:**
  - Users filter employees using the filter form.
  - CSV is generated and downloaded via the backend.

---

