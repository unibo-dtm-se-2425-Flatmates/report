---
title: Development
has_children: false
nav_order: 5
---

# Development

## 1. Distributed Version Control System (DVCS)

The development process for **Flatmates** utilized **Git** as the version control system, hosted on **GitHub**. This centralized repository facilitated collaboration, **history tracking, and code integrity** throughout the project lifecycle.

**Branching Strategy:** We adopted a **Feature Branch Workflow**.

- **main Branch:** This branch serves as the stable, production-ready **codebase. Direct** commits to **main** were restricted to ensure stability.
- **Feature Branches:** For every new requirement (e.g., **"Add Expense Logic", "Create** Shopping List UI"), a dedicated branch was created (e.g., **feature/expense-logic**). 3 types of branches were established, each of them was characterized by a brief description in lowercase letters with "-" separating the words.
  - **INFRA/...** when implementing or modifying something that was not a new feature, such as updating requirements.txt, updating .gitignore, or creating README.md/CHANGELOG.md/LICENCE, etc (e.g. INFRA/update-gitignore)
  - **BUGFIX/...** when fixing an error, such as a failed test, a typo, or an incorrect link (e.g. BUGFIX/fix-frontend-auth-tests)
  - **FEATURE/...** when developing or implementing a new feature (e.g. FEATURE/implement-debt-semplification).
- **Merge Process:** Once a feature was complete and tested **locally, a Pull Request (PR)** was opened against **main**. We would have wanted to implement **that merging required at** least one code review approval to ensure quality and prevent regressions.

**Commit Conventions:** The rule for commits was simply **to write, in a short sentence, what that** commit did, always keeping the first letter capitalized and the rest in lowercase.

In order to maintain a clean and readable history, we should have implemented the **Conventional Commits** specification. Following this **specification ensures consistency across** all commit messages, making it easier to understand the purpose and scope of each change. It provides a structured format for writing commit messages, typically including a type (such as **feat, fix, or docs**), an optional scope, and a concise description of the change.

**Lessons Learned:**

One thing that we have learned is that it would have been better to implement commit versioning and greater control over PR approval. In the future, we would like to implement a GitHub action that automatically assigns the PR to one of the members (in this case, since there are two of us, to the other), and that the merge is blocked until the PR is approved. This would have allowed for a security standard to prevent auto-merges and, above all, a double check on the code that would end up in production.

## 1.1 Implementation Details

### 1.1.1 API Communication Flow

### 1.1.2 Data Formats

JSON Request/Response Format: why JSON?

- Human-readable (easy debugging)
- Lightweight (lower bandwidth)
- Native JavaScript/Python support
- Browser DevTools friendly
- Natural for REST APIs

### 1.1.3 Database Design

Query Strategy: SQL (SQLite) Why SQL?

- Structured, relational data (perfect for households)
- SQLite3 is already integrated within Python
- Complex joins (events assigned to multiple users)
- Transactions (atomic operations for expense + involvement + debt)

### 1.1.4 Frontend-Backend Separation

The project enforces a strict separation between the **Streamlit Frontend** and the **FastAPI Backend**.

- **Communication:** The frontend never accesses the database **directly. Instead, it uses** the **requests** library to make HTTP calls to the backend API.
- **Benefit:** This decoupling allows the backend to be reused by other clients (e.g., a future mobile app) without modification.

## 1.2 Technological Details

### 1.2.1 Programming Languages and Frameworks:

- **Python (3.8+):** The primary language for both backend **and frontend, chosen for its** readability and rich ecosystem.
- **FastAPI:** A modern, high-performance web framework **for building APIs with Python. It** was selected for its automatic validation and auto-generated documentation.
- **Streamlit:** An open-source Python framework for building **data apps. It allowed us to** rapidly build a responsive interactive UI without writing HTML/CSS/JS.

**Data Persistence: SQLite:** A C-language library that **implements a small, fast, self-contained,** high-reliability, full-featured, SQL database engine. It was chosen for its simplicity and zero-configuration setup, making the app easy to deploy and test.

**Key Libraries and Dependencies:**

- **`uvicorn`**: An ASGI web server implementation for Python, **used to run the FastAPI** application.
- **`pydantic`**: Data validation and settings management **using Python type annotations.**
- **`pandas`**: Used in the frontend for data manipulation **and preparing datasets for** visualization.
- **`altair`**: A declarative statistical visualization library used to render interactive charts for expense analytics.
- **`requests`**: A simple HTTP library for Python, used by **the frontend to communicate with** the backend API.
- **`pytest`**: A framework for writing small, readable tests, **ensuring code reliability.**

**External Services:**

- **Streamlit Community Cloud:** Used for hosting the frontend **interface.**
- **Railway:** Used for hosting the backend API service.
