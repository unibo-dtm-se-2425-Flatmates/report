---
title: Developer guide
has_children: false
nav_order: 11
---

# 1. Developer Guide

This section is intended for developers who wish to contribute to the **Flatmates** project or run it locally for development.

## 1.1 Repository

https://github.com/unibo-dtm-se-2425-Flatmates/artifact

## 1.2 Issue Reporting

- Open issues directly on GitHub
- Use descriptive titles (e.g., "Fix: Calendar event deletion fails" or "Feature: Add expense" categories)
- Include reproduction steps for bugs
- Attach screenshots or error logs when relevant

## 1.3 Team Communication

- **Maintainers**: 
  - Giacomo Lancerin ([@giacomolancerin](https://github.com/giacomolancerin))
  - Matteo Minnucci ([@matteominnucci](https://github.com/matteominnucci))
- **Response time**: Check open pull requests and issues for activity patterns

## 1.4 Branch Naming

- **Features:** feature/add-login-feature
- **Fixes:** bugfix/fix-test-frontend-secrets
- **Structural:** infra/update-.gitignore

## 1.5 Code Style

- Python: Follow PEP 8 guidelines
- Docstrings: Use triple-quoted strings with description and return type
- Type hints: Include where it improves clarity (Pydantic models enforce this)
- Imports: Group standard library, third-party, and local imports separately

## 1.6 File Organization

- **Backend routes:** Place in `backend/routers/` with descriptive names (e.g., expenses.py, calendar.py)
- **Frontend pages:** Place in `frontend/pages/` with numbered prefixes for order (e.g., 1_Calendar.py)
- **Models:** Define all Pydantic schemas in `backend/models.py`
- **Database logics:** Isolate in `backend/db/` `database.py`
- **Ignored Files:** the `.gitignore` excludes:
  - Virtual environments (venv/, .venv/)
  - Database files (*.db, *.sqlite)
  - IDE settings (.vscode/)
  - Environment secrets (.env, .streamlit/secrets.toml)

## 1.7 Pull Request Process

Open a pull request on GitHub from your feature branch to **main**. Write a clear description of changes:

- What problem does it solve?
- What functionality does it add?
- Are there breaking changes?

Link related issues with keywords (e.g., "Closes #5"). While waiting for the automatic reviewer request to be implemented, set both maintainers as **reviewers**. Wait for review and address feedback by pushing additional commits to the same branch.

**Merging:** Merges are performed by maintainers after approval.

## 2. Project Structure

The repository follows a clean separation of concerns between the backend and the frontend.

```
artifact/
├── .gitignore
├── LICENSE
├── MANIFEST.in
├── README.md
├── CHANGELOG.md
├── requirements.txt           # Python dependencies
├── run_tests.py               # Helper to run test suite
├── backend/                   # FastAPI backend
│   ├── main.py                # Backend entry point
│   ├── models.py              # Pydantic models / schemas
│   ├── db/
│   │   ├── __init__.py
│   │   └── database.py        # Database configuration/connection
│   └── routers/               # API routes
│       ├── auth.py
│       ├── calendar.py
│       ├── expenses.py
│       ├── house.py
│       └── shopping.py
├── frontend/                  # Streamlit frontend
│   ├── app.py                 # Frontend entry point
│   ├── utils.py               # Helpers and UI utilities
│   └── pages/                 # Multi-page app screens
│       ├── 0_Settings.py
│       ├── 1_Calendar.py
│       ├── 2_Shopping_List.py
│       └── 3_Expenses.py
└── test/                      # Unit tests
   ├── test_backend.py
   ├── test_database.py
   └── test_frontend.py
```

## 3. Development Setup

### 3.1 Prerequisites

- Python 3.8+
- Git

### 3.2 Installation

#### 3.2.1 Clone the repository:
```bash
git clone https://github.com/unibo-dtm-se-2425-Flatmates/artifact
```

#### 3.2.2 Create a virtual environment:
```bash
python -m venv venv
```

#### 3.2.3 Activate the virtual environment:
```bash
source venv/bin/activate # Linux/Mac
venv\Scripts\activate # Windows
```

#### 3.2.4 Install dependencies:
```bash
pip install -r requirements.txt
```

### 3.3 Running the Application

#### 3.3.1 Run the backend:
Start the backend server
```bash
uvicorn backend.main:app --reload
```
The API will start at http://localhost:8000.
Swagger UI docs available at http://localhost:8000/docs.

#### 3.3.2 Run the frontend:
```bash
streamlit run frontend/app.py
```
The UI will open at http://localhost:8501.

## 4. API Documentation

The backend exposes a RESTful API. There are 19 endpoints divided as:
- Auth (3)
- Expenses (5)
- House (4)
- Calendar (3)
- Shopping List (3) 
- Systems (1)

Most of them are GET/POST, but we’ve also used PUT and DELETE.
All the endpoints can be viewed inside the Swagger UI docs available at http://localhost:8000/docs (after running the backend). The APIs code is inside the `backend/routers/` directory.


## 5. Testing

We use **pytest** for backend testing.

To run the test suite:
```bash
python -m run_tests
```

## 6. Contribution Workflow

1. Fork the repository.
2. Create a feature branch (git checkout -b feature/new-feature).
3. Commit changes following Conventional Commits.
4. Push your branch and open a Pull Request.