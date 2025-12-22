---
title: Requirements
has_children: false
nav_order: 3
---

# Requirements

This section outlines the functional, non-functional, and implementation requirements for  
Flatmates.

![Diagram Overview](/report/pictures/requirements.png)

## 1. User Stories

### 1.1 Persona: Alice
New resident joining an existing shared household; unfamiliar with the platform but tech-savvy;  
wants quick onboarding.

- As a new flatmate, I want to create a user account so that I can access the app features.
- As a new user, I want to create a new house profile so that I can invite my roommates to  
  join.
- As a user joining an existing household, I want to enter a unique "join code" so that I can  
  be added to the correct house group.

### 1.2 Persona: Bob
Detail-oriented resident who takes initiative on household coordination; actively manages  
shared responsibilities; prefers structured workflows.

- As a flatmate, I want to add items to a shared shopping list so that we don't forget  
  essentials.
- As a flatmate, I want to mark items as "purchased" on the shopping list so that we avoid  
  buying duplicates.
- As a flatmate, I want to schedule cleaning shifts on a shared calendar so that everyone  
  knows their responsibilities.

### 1.3 Persona: Charlie
Financially aware resident who carefully tracks shared expenses; wants transparency in  
bill-splitting; motivated by fair settlements.

- As a flatmate, I want to log a shared expense (e.g., internet bill) so that the cost is  
  recorded.
- As a flatmate, I want to see a calculation of "who owes whom" so that we can settle  
  debts easily without manual math.
- As a flatmate, I want to record a reimbursement payment so that the system updates our  
  debt balances.

## 2. Functional Requirements
Functional requirements describe what the system should do to support user activities. They are  
derived directly from user stories.

### 2.1 House Management
**Requirement:** Users are able to login and manage their household association.

**Acceptance Criteria:**
- After registration, users can choose to "Create a House" (generating a unique join code)  
  or "Join a House" (inputting an existing code).
- Users can reset the house data (clearing all events/expenses) or delete the house  
  entirely if needed.

### 2.2 Shared Calendar
**Requirement:** The system must provide a collaborative calendar for scheduling events and  
chores.

**Acceptance Criteria:**
- Users can view a weekly and monthly calendar interface, with the added functionality of  
  being able to display all the events in a list.
- Users can create events with a title, date, start/end time, and description.
- Users can assign specific flatmates to an event (e.g., "Trash Duty" assigned to "Alice").
- Events must be visible to all members of the house immediately after creation.

### 2.3 Shopping List
**Requirement:** The system must allow real-time collaboration on a household shopping list.

**Acceptance Criteria:**
- Users can add new items to the list, specifying the item name and quantity.
- The list displays who added each item.
- Users can toggle an item's status to "Purchased" via a checkbox.

### 2.4 Expense Tracker and Debt Settlement
**Requirement:** The system must track shared expenses and algorithmically calculate debts.

**Acceptance Criteria:**
- Users can log an expense with a title, amount, and the payer's identity.
- Users can specify which flatmates are involved in the split.
- The system must calculate net balances using a debt simplification algorithm (minimizing  
  the number of transactions required to settle up).
- Users can view a "Debts" summary showing exactly who owes whom and how much.
- Users can record "Reimbursements" to pay off debts, which updates the net balance to  
  zero.

## 3. Non-Functional Requirements
Non-functional requirements specify system qualities beyond behavioral features, including  
performance, reliability, security, and usability.

### 3.1 Usability
**Requirement:** The application must be intuitive and accessible for daily household use.

**Acceptance Criteria:**
- The interface must be responsive, adapting to both desktop and mobile screens for  
  on-the-go access.
- Key actions (adding an item, logging an expense) must be achievable within at least 5  
  clicks from the homepage.
- Visual feedback (e.g., success messages) must be provided for all data entry actions.

### 3.2 Performance
**Requirement:** The system must handle data synchronization efficiently.

**Acceptance Criteria:**
- API responses for fetching lists or calendar data should occur within 2 seconds under  
  normal network conditions.
- The debt simplification algorithm must execute fast even with dozens of recorded  
  expenses.

### 3.3 Reliability and Data Integrity
**Requirement:** The system must ensure data consistency across the household.

**Acceptance Criteria:**
- All data (expenses, events, list items) is persistently stored in a relational database  
  (SQLite).
- Concurrent updates (e.g., two users adding items simultaneously) must be handled  
  without data loss.

### 3.4 Security
**Requirement:** User data and house access must be protected.

**Acceptance Criteria:**
- House data is isolated; users cannot access data from a house they do not belong to.
- Passwords are securely hashed by combining a random salt with the password and  
  applying the SHA-256 cryptographic hash function, ensuring each password hash is  
  unique and resistant to common attacks like rainbow table lookups.

## 4. Implementation Requirements
Implementation requirements constrain the system realization process and are justified by  
technical, economic, or organizational factors.

### 4.1 Technology Stack
**Requirement:** The application must be built using the specified modern Python stack.

**Acceptance Criteria:**
- Backend: FastAPI service with Uvicorn.
- Frontend: Streamlit for dynamic UI rendering.
- Database: SQLite for lightweight, file-based persistence.
- Charts: Altair for rendering expense graphs.

### 4.2 Development Standards
**Requirement:** The codebase must follow clean architecture principles.

**Acceptance Criteria:**
- Separation of concerns between Backend (API routers, DB logic) and Frontend  
  (Streamlit pages).
- Dependencies managed via `requirements.txt`.
- Comprehensive unit testing using `pytest` for backend, frontend and database  
  operations.

### 4.3 Deployment
**Requirement:** The application must be deployable to cloud platforms.

**Acceptance Criteria:**
- Backend capable of running on PAAS providers (e.g., Railway).
- Frontend capable of running on Streamlit Community Cloud.

## 5. Glossary

- **Join Code:** A unique alphanumeric number generated when a house is created, used by  
  other users to join that specific household.
- **Reimbursement:** A direct payment recorded between two flatmates to settle a debt,  
  distinct from a shared expense.
- **Debt Simplification:** An algorithm that reduces a complex web of settlements (e.g., A  
  owes B, B owes C) into the simplest set of direct transfers (e.g., A pays C).
