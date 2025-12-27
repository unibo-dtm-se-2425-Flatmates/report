---
title: Deployment
has_children: false
nav_order: 8
---

# Deployment

## 1. Deployment Strategy

The **Flatmates** application is designed for a **Dual Deployment** strategy, separating the Frontend (User Interface) from the Backend (API and Data) to ensure scalability and flexibility. This decoupled approach allows each component to be hosted on platforms optimized for their specific runtime requirements.

### 1.1 Backend Deployment (Railway)

For the backend we have selected **Railway** for its seamless GitHub integration and automatic builds.

- **Repository Connection:** The GitHub repository is linked to a Railway project.
- **Build Command:** Railway automatically detects the requirements.txt and installs dependencies.
- **Start Command:** uvicorn backend.main:app --host 0.0.0.0 --port $PORT

### 1.2 Frontend Deployment (Streamlit Community Cloud)

The frontend is hosted on **Streamlit Community Cloud**, which is purpose-built for deploying Streamlit apps directly from a GitHub repository.

- **Repository Connection:** The `frontend/app.py` file is selected as the entry point.
- **Configuration:**
  - **Secrets:** The `API_URL` pointing to the deployed Railway backend (e.g., https://flatmate-api-production.up.railway.app) is stored in the Streamlit secrets management system.
  - **Dependencies:** Streamlit Cloud installs packages from `requirements.txt` automatically.

## 2. System Requirements

To run the application locally (e.g., for development or self-hosting), the following requirements must be met:

- **Operating System:** Windows, macOS, or Linux.
- **Python:** Version 3.8 or higher.
- **Git:** To clone the repository.

## 3. Local Installation and Execution Guide

Follow these steps to deploy the application on a local machine:

### Step 1: Clone the Repository
```bash
git clone https://github.com/unibo-dtm-se-2425-Flatmates/artifact.
```

### Step 2: Setup Virtual Environment
Create the environment and activate it:
```bash
python -m venv venv
```
Activate the environment:
```bash
source venv/bin/activate # on MacOS or Linux
venv\Scripts\activate # on Windows
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Run the Backend
Open a terminal and execute:
```bash
uvicorn backend.main:app --host 0.0.0.0 --port 8000
```
**Verify:** Open http://localhost:8000/docs in your browser. You should see the interactive Swagger API documentation. Note: The SQLite database (`flatmates.db`) will be automatically created in `backend/db/` upon the first run.

### Step 5: Run the Frontend
Open a new terminal and execute:
```bash
streamlit run frontend/app.py
```
**Access:** The application will launch automatically in your default browser at http://localhost:8501.

## 4. Server-Side Installation

### 4.1 Backend Deployment (Railway):

The backend requires a Python-compatible Platform-as-a-Service. Railway was selected for automatic GitHub integration and build triggers. The backend is automatically redeployed with each PR merge.

**Installation Steps:**

1. Connect the GitHub repository to a Railway project
2. Railway detects requirements.txt and installs dependencies automatically
3. Configure start command: `uvicorn backend.main:app --host 0.0.0.0 --port $PORT`

**Dependencies:**

The backend uses SQLite as an embedded database. No external database server installation is required. The database file (`flatmates.db`) persists in the `backend/db/` directory within the Railway container filesystem.

### 4.2 Frontend Deployment (Streamlit Community Cloud):

The frontend deploys directly from the GitHub repository without server installation.

**Installation Steps:**

1. Connect repository to Streamlit Community Cloud
2. Select `frontend/app.py` as the entry point
3. Streamlit Cloud installs dependencies from requirements.txt automatically

**Configuration:**

Create a folder .streamlit with a file secrets.toml with `API_URL` = Full Railway backend URL (e.g. https://flatmate-api-production.up.railway.app)

Add the same URL in Streamlit secrets management (Streamlit Community Cloud): `API_URL` = Full Railway backend URL.

**Dependencies:** No additional software is required. Streamlit Community Cloud provides the complete runtime environment.