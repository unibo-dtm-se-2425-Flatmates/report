---
title: Release
has_children: false
nav_order: 7
---

# Release

The **Flatmates** application is released as a composite package consisting of two primary components.

## 1.1 Backend Service (APIs)

- **Artifacts:** A Python application powered by FastAPI.
- **Contents:**
  - **`backend/`**: Source code for API routers, models, and database logic.
  - **`requirements.txt`**: Python dependencies (e.g., fastapi, uvicorn, pydantic).
  - **`flatmates.db`**: The SQLite database schema (initialized on first run).
- **Target Environment:** A Python 3.8+ environment capable of running Uvicorn.

## 1.2 Frontend Interface (Web App)

- **Artifacts:** A Streamlit application.
- **Contents:**
  - **`frontend/`**: Source code for UI pages (`app.py`, `utils.py`, `pages/`).
  - **`.streamlit/secrets.toml`**: the secrets for link the Railway backend for the deployment on Streaming Community Cloud.
- **Target Environment:** A Streamlit runtime connected to the backend API.

## 2. Release Strategy

### 2.1 Versioning Schema

The project adheres to **Semantic Versioning (SemVer)** using the **MAJOR.MINOR.PATCH** format to communicate the significance of updates:

- **MAJOR (1.x.x):** Reserved for incompatible API changes or significant database schema alterations that require migration.
- **MINOR (x.1.x):** Introduces new features (e.g., adding a "Chore Tracker" module) in a backward-compatible manner.
- **PATCH (x.x.1):** Covers bug fixes, performance improvements, or UI tweaks that do not alter the data model or API contract.

### 2.2 Release Process

The release workflow have been managed through **GitHub**, **serving as both the version control** system and the distribution platform for source code. With this logic:

1. **Tagging:** Stable releases are marked with git tags (e.g., v1.0.0).
2. **Changelog:** Each release is accompanied by a release note detailing:
   - **New Features**
   - **Bug Fixes**
   - **Known Issues**

## 3. Licensing

**Chosen License:** Apache License 2.0

The Apache 2.0 license was selected for its balance of permissiveness and legal clarity. It allows users to freely use, modify, and distribute the software (including for commercial purposes) while providing an express grant of patent rights from contributors to users. This ensures that the project remains open and safe for future developers to build upon, fitting the educational and collaborative nature of the project.

### 3.1 Key Terms

- **Permissive:** Users can do almost anything with the code.
- **Attribution:** Users must preserve the copyright notice and license text.
- **No Warranty:** The software is provided "as is" without liability.

## Where to Access

- **Source Code:** Available publicly on GitHub at https://github.com/unibo-dtm-se-2425-Flatmates/artifact.
- **Live Demo:** The frontend is deployed and accessible via Streamlit Community Cloud (linked in the repository README).
- **API Documentation:** Auto-generated Swagger UI is available at the /docs endpoint of the running backend instance.
