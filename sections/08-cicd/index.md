---
title: CI/CD
has_children: false
nav_order: 9
---

# CI/CD

The **Flatmates** project should have employed a **Continuous Integration (CI)** strategy to ensure code stability and prevent regressions. Instead, full **Continuous Deployment (CD)** it’s been automated with the automatic deployment to cloud platforms.

## 2. CI - Continuous Improvements

Currently, no continuous improvement pipeline has been implemented in the project. However, we plan to add one shortly, with the aim of:

- **Automated Testing:** On every push to the main branch or creation of a Pull Request, an automated workflow triggers the execution of the full test suite.
- **Environment Setup:** The workflow automatically sets up a clean Python environment and installs dependencies to ensure the code builds correctly.
- **Quality Gates:** The pipeline blocks merging if any tests fail, ensuring that broken code never reaches the production branch.

### 2.1 Workflow (**`.github/workflows/ci.yml`**):

The pipeline we want to implement:

```yaml
name: CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up Python 3.10
        uses: actions/setup-python@v5
        with:
          python-version: "3.10"
      
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
      
      - name: Run tests
        run: |
          python run_tests.py

```

This workflow is configured to:

- **Trigger:** Run on every push and pull request to the main branch.
- **Environment:** Use the latest Ubuntu runner.
- **Steps:**
  - **Checkout:** Clones your repository.
  - **Setup Python:** Installs Python 3.10.
  - **Install Dependencies:** Installs packages from requirements.txt
  - **Test:** Runs your test suite using python run_tests.py.

### 2.2 Why automate CI?

- **Reliability:** Manual testing is prone to oversight. Automated CI ensures that critical logic (like the debt simplification algorithm) is verified against edge cases every time code changes.
- **Collaboration:** With multiple developers potentially working on features (e.g., shopping list vs. expenses), CI provides immediate feedback if a change in one module breaks another.

## 3. Deployment Workflow (CD)

- **Backend (Railway):** Railway is configured to watch the GitHub repository. It automatically detects new commits to **main** and rebuilds the service. This serves as a "Continuous Delivery" mechanism where deployment is automated but relies on the external platform's integration rather than a repository-defined script.
- **Frontend (Streamlit):** Similar to the backend, Streamlit Community Cloud automatically pulls the latest code from **main** when it sees that something has changed in the `frontend/` repo and updates the running app without manual intervention. If, on the other hand, something changes in the `backend/` folder, it does not relaunch, as it is always linked to the secrets `API_URL` in the `.stremlit/secrets.toml` folder, so unless the link is changed, it always points to the same backend hosting (Railway), which took care of redeploying the new backend.

## 4. Future Improvements

To achieve a mature CI/CD pipeline, the following additions are planned:

1. **Formatting:** Add a step to run `blake`, `isort` and `flake8` with the command `pre-commit` before every commit.
2. **Semantic Release:** Automate version tagging and changelog generation based on commit messages.
3. **CI Workflow:** As said before, a new workflow for the CI will be implemented for launch tests on every push and pull request to the main branch.
