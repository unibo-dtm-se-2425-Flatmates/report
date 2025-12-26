---
title: Validation
has_children: false
nav_order: 6
---

# Validation

## 1. Testing Strategy

Flatmates failed to adopt Test-Driven Development (TDD) for critical business logic, writing tests before implementing features. Each core feature (debt calculation, expense splitting, user authentication) had corresponding tests was not defined first.

The testing approach should have followed this logic by including unit, integration, and system testing. Unit tests should describe their purpose and rationale, reporting success rate and coverage. Integration tests should identify the component pairs tested, explain the testing plan, and report success rate and coverage, specifying any test doubles used and the reasons for their choice. System tests should validate the system as a whole according to the acceptance criteria, including their rationale, plan, success rate, and coverage. If containerization tools were used, this section should explain how they supported testing in isolated or clean environments.

Instead, the validation strategy for **Flatmates** was **designed to ensure that the core logic of the** application, particularly the debt simplification algorithm and house management features, functions correctly before deployment. We adopted a comprehensive testing strategy focused on the **Frontend**, **Backend** and **Database Testing**, leveraging **the** **pytest** framework and **TestClient** from FastAPI to simulate API requests without needing a running server.

### 1.1 Primary Framework: pytest

- Simple, readable test syntax
- Excellent fixture support for test setup/teardown
- Built-in parameterization for testing multiple scenarios
- Industry standard in Python ecosystem

### 1.2 Secondary Frameworks:

- FastAPI TestClient: For simulating HTTP requests without running live server
- Manual Testing: Streamlit UI/UX validation.

## 2. Test Development Process

The project adopts an Automated Testing strategy using the Pytest framework. The testing process is divided into two main layers:

### 2.1 Backend Integration Testing:

**Tools**: pytest, FastAPI TestClient, sqlite3 (in-memory).

**Approach**: These tests verify the correctness of the API endpoints and their interaction with the database. By using an in-memory SQLite database (:memory: or temporary file), tests are isolated, ensuring that data created in one test does not affect others.

**Scope**: Covers all API routes (Calendar, Shopping, Expenses, House, Auth).

### 2.2 Frontend Unit Testing:

**Tools: pytest, monkeypatch (for mocking).**

**Approach:** These tests focus on the frontend. The frontend/utils.py module contains the logic for communicating with the backend. Network requests (requests.get, post, etc.) are mocked to verify that the frontend correctly handles various API responses (success, failure, empty data) without requiring a running backend.

### 2.3 Success Rate

- **Total Tests Executed**: 27

- **Passed**: 27

- **Failed**: 0

- **Success Rate**: 100%

### 2.4 Coverage

Code coverage was measured using pytest-cov.

- **Backend Coverage**: ~90% (High). The core logic in routers and database interactions is well-covered.
- **Frontend Coverage**: ~24%. Covered the "happy-path" of the user.

**Note:** The low coverage is expected for Streamlit applications, as the UI rendering code (app.py and pages) is difficult to test with standard unit testing tools. However, the critical logic in utils.py (data fetching and processing) has higher coverage (~48%).

The testing suite provides strong confidence that the system meets its functional requirements.

**Robustness**: The 100% pass rate on the backend ensures that the core business logic (e.g., debt simplification, event scheduling) is reliable.

**Error Handling**: Frontend tests verify that the application gracefully handles network errors or missing data.

### 2.5 Acceptance Test

The Backend Integration Tests (test/test_backend.py) serve as the primary Acceptance Tests for the system's functionality. They simulate real user scenarios to validate that features work as intended.

**Key Acceptance Scenarios Verified:**

#### a. House Management
- **Tests:** `test_house_settings`, `test_auth_join_existing_house`
- **Outcome:** Users can create a house, generate a join code, and other users can join that house successfully.

#### b. Calendar System
- **Tests:** `test_calendar_flow`
- **Outcome:** Events can be created, listed, and updated. Dates and assignments are persisted correctly.

#### c. Shopping List
- **Tests:** `test_shopping_flow`
- **Outcome:** Items can be added, viewed, and deleted from the shared list.

#### d. Expense Management & Debt Simplification
- **Tests:** `test_expenses_and_debts_flow`
- **Outcome:** Expenses are recorded and split correctly. Debts are automatically calculated based on expenses. Reimbursements correctly reduce the outstanding debt. Invalid actions (e.g., negative reimbursement) are rejected.

---

**Requirement Fulfillment Summary:**
The acceptance tests confirm that the application satisfies the core requirements:
- **Shared Resources**: Validated by Calendar and Shopping tests.
- **Financial Management**: Validated by Expense and Debt tests (including complex splitting logic).
- **Multi-user Support**: Validated by Auth and House tests (multi-user data interaction).

The outcomes demonstrate that the system is functionally complete relative to the project requirements.


