---
title: Design
has_children: false
nav_order: 4
---

# Design

This chapter explains the strategies and design decisions used to meet the requirements  
identified in the analysis.

## 1. Architecture

The Flatmates application follows a client-server architecture, specifically adopting a Layered  
Architecture pattern to separate different concerns. This design ensures modularity, scalability,  
and ease of maintenance.

**Rationale:**

- Layered Architecture provides clear separation of concerns, enabling independent  
  development and testing of presentation, business logic, and data layers.
- Supports the natural hierarchical structure of household management features (UI →  
  business logic → data storage).
- Facilitates future extensions (e.g., mobile app clients, alternative frontends) via shared  
  backend API.

![layers](/report/pictures/layers.png)

The system is composed of three main layers:

### 1.1 Presentation Layer (Frontend)

- **Technology:** Streamlit
- **Responsibility:** Renders the user interface and handles user interactions. It  
  communicates with the backend via API calls. It is responsible for displaying the  
  dashboard, calendar, shopping lists, and expense reports.

- **Key Components:**
```
├── frontend/                  # Streamlit frontend
│   ├── app.py                 # Frontend entry point
│   ├── utils.py               # Helpers and UI utilities
│   └── pages/                 # Multi-page app screens
│       ├── 0_Settings.py
│       ├── 1_Calendar.py
│       ├── 2_Shopping_List.py
│       └── 3_Expenses.py
```


### 1.2 Application Logic Layer (Backend)

- **Technology:** FastAPI with Uvicorn server
- **Responsibility:** Exposes API endpoints. It processes incoming requests, enforces  
business rules (e.g., debt calculation algorithms), validates data using Pydantic models,  
and orchestrates interactions with the database.

- **Key Components**
```
├── backend/ # FastAPI backend
│ ├── main.py # Backend entry point
│ ├── models.py # Pydantic models / schemas
│ ├── db/
│ │ ├── init.py
│ │ └── database.py # Database configuration/connection
│ └── routers/ # API routes
│ ├── calendar.py
│ ├── expenses.py
│ ├── house.py
│ └── shopping.py
```

- **Interactions:**
- Receives HTTP requests from Streamlit frontend
- Queries SQLite database via SQL
- Returns JSON responses to frontend

### 1.3 Data Persistence Layer

- **Technology:** SQLite (via Python sqlite3)
- **Responsibility:** Stores all persistent data including user profiles, house configurations,  
events, expenses, shopping items, and reimbursements. Ensures data integrity and  
isolates data by house.

**Key Entities:**
- `houses` - Household profiles with name and unique join codes
- `users` - User accounts with salted SHA-256 password hashes and house associations
- `events` - Calendar events and chores with assignee information
- `shopping_items` - Shopping list items with quantity and purchase status
- `expenses` - Shared expense records with payer and involved people (stored as JSON)
- `reimbursements` - Direct payment records between users
- `sessions` - Authentication tokens with timestamps

**Interactions:**
- Receives method calls from FastAPI backend through the Database class
- Enforces schema constraints (foreign keys, unique constraints) and data consistency
- Returns typed domain objects (User, Event, Expense, etc.) to backend

## 2. Infrastructure

For the current scope, Flatmates operates as a monolithic application with all components  
co-located:

| Component | Technology | Deployment | Purpose |
|---------|-----------|------------|--------|
| Frontend Server | Streamlit | Streamlit Community Cloud | User-facing interface, accessible via browser |
| API Server | FastAPI + Uvicorn | Railway | Backend business logic and API endpoints |
| Database | SQLite | Railway | Persistent data storage |

![infrastructure](/report/pictures/infrastructure.png)

## 3. Modelling

The Flatmates application follows a layered architecture with a pragmatic approach to domain  
modeling. The system is organized around core domain entities with direct database  
persistence.

The application is structured around the following core entities (defined in  
`backend/models.py`):

### 3.1 Household Management

**House (HouseSettings)**

- `id` - Unique identifier
- `name` - Household name
- `flatmates` - List of usernames (derived from users table)
- `join_code` - Unique numeric code for invitations

**User**

- `id` - Unique identifier
- `username` - Unique username (globally unique, not per-house)
- `house_id` - Reference to associated house
- Password stored as salted SHA-256 hash
- No role system (Admin/Member) - all users have equal permissions

### 3.2 Calendar and Events

**Event**

- `id` - Unique identifier
- `title` - Event name
- `date` - Scheduled date
- `start_time` - Optional start time
- `end_time` - Optional end time
- `description` - Optional details
- `assigned_to` - List of usernames (stored as JSON)

### 3.3 Shopping Management

**ShoppingItem**

- `id` - Unique identifier
- `name` - Item name
- `quantity` - Item quantity (default: 1)

- `added_by` - Username who added the item
- `purchased` - Boolean purchase status

### 3.4 Expense Management

**Expense**

- `id` - Unique identifier
- `title` - Expense description
- `amount` - Total cost (float)
- `payer` - Username who paid
- `involved_people` - List of usernames splitting the cost (stored as JSON)
- No category or date fields
- Equal split only (no custom split logic, possible future implementation)

**Debt**

- `debtor` - Username owing money
- `creditor` - Username receiving money
- `amount` - Amount owed
- Calculated dynamically, not persisted

**Reimbursement**

- `id` - Unique identifier
- `from_person` - Username making payment
- `to_person` - Username receiving payment
- `amount` - Payment amount
- `note` - Optional note
- No date field

## 4. Data Access Layer

The system uses a single Database class (`backend/db/database.py`) rather than separate  
repositories:

- Direct SQL operations through Database class methods
- No repository pattern implementation

## 5. API Layer

REST endpoints are organized in routers (`backend/routers/`):

- `house.py` - House settings and member management
- `calendar.py` - Events operations
- `shopping.py` - Shopping list management
- `expenses.py` - Expense tracking and debt calculation

All business logic is contained within router handlers, with no separate service layer.

## 6. Component Diagram

The following structure illustrates the high-level components and their interactions:

![Component Diagram](/report/pictures/component-diagram.png)

### 6.1 Client (Streamlit App)

`app.py`: Main entry point handling authentication (login/register) and navigation. Displays  
welcome page with links to all app modules (Settings, Calendar, Shopping List, Expenses).  
Manages session state and authentication.

`pages/`: Individual feature modules implementing specific functionality:

- `0_Settings.py` - House configuration and user profile management
- `1_Calendar.py` - Event scheduling and calendar view for cleaning, tasks, and activities
- `2_Shopping_List.py` - Shared shopping list management
- `3_Expenses.py` - Bill splitting and expense tracking with debt calculation

`utils.py`: Helper functions for API communication with the FastAPI backend. Contains  
methods for authentication (login_user, register_user), profile fetching and sidebar  
rendering.

### 6.2 Server (FastAPI)

`main.py`: API entry point that creates the FastAPI application instance and aggregates all  
domain-specific routers (auth, calendar, shopping, expenses, house). Provides root endpoint  
with welcome message.

`routers/`: Domain-specific API handlers containing endpoint logic:

- `calendar.py` - Events operations
- `shopping.py` - Shopping list item management
- `expenses.py` - Expense tracking, debt calculation, and reimbursement handling
- `house.py` - House settings and configuration management

`models.py`: Pydantic model definitions for request/response validation and serialization.  
Defines data structures for Event, ShoppingItem, Expense, Debt, Reimbursement, HouseSettings,  
User, and authentication payloads.

### 6.3 Database

`flatmates.db`: SQLite relational database file storing all application data in tables: houses,  
users, sessions, events, shopping_items, expenses, and reimbursements. Located in  
`backend/db/` directory.

`database.py`: Database abstraction layer providing the Database class. Manages SQLite  
connections, schema creation/migration, and implements all CRUD operations. Handles  
password hashing, session management, multi-house data isolation, and JSON serialization for  
list fields.

## 7. Data Modelling

The domain model is designed to support the collaborative nature of household management.  
The core entities and their relationships are defined below using Entity-Relationship (ER)  
concepts.

![ER Concepts](/report/pictures/er-entity-relationship-concepts.png)

### 7.1 Core Entities

`House`: The central aggregation point.  
- Attributes: id (PK), name, join_code (unique)
- Relationships: One House has many Users, Events, Expenses, ShoppingItems, and  
Reimbursements

`User`: Represents a registered flatmate.  
- Attributes: id (PK), username (unique), password_hash, password_salt, house_id (FK)
- Relationships: Belongs to one House

`Event`: Represents a calendar entry.  
- Attributes: id (PK), title, date, start_time, end_time, description,  
assigned_to (JSON list of usernames), house_id (FK)
- Relationships: Belongs to one House

`ShoppingItem`: Represents an item to buy.  
- Attributes: id (PK), name, quantity, added_by (username string), purchased  
(Boolean/Integer 0 or 1), house_id (FK)
- Relationships: Belongs to one House

`Expense`: Represents a shared cost.  
- Attributes: id (PK), title, amount, payer (username string), involved_people  
(JSON list of usernames), house_id (FK)
- Relationships: Belongs to one House

`Reimbursement`: Represents direct payment between users.  
- Attributes: id (PK), from_person (username), to_person (username), amount, note,  
house_id (FK)
- Relationships: Belongs to one House

### 7.2 Database Schema (SQLite)

The SQLite database uses the following tables:

- `houses`: Stores house metadata (id, name, join_code)
- `users`: Stores user credentials (id, username, password_hash, password_salt, house_id)
- `sessions`: Manages authentication tokens (token, user_id, created_at)
- `events`: Stores calendar events with assigned_to as JSON-serialized list
- `shopping_items`: Stores shopping list items with purchase status
- `expenses`: Stores financial records with involved_people as JSON-serialized list
- `reimbursements`: Stores direct payments between users

## 8. Interaction Design

**Sequence Diagram: Adding an Expense**

![Sequence Diagram Adding An Expense](/report/pictures/sequence-diagram-adding-an-expense.png)

This sequence illustrates the flow when a user adds a new shared expense:

1. User fills the "Add Expense" form on the Streamlit Frontend.
2. Frontend sends a POST /expenses/ request with the expense payload (title, amount,  
 payer, split details) to the Backend.
3. Backend (Router) validates the current user's session token.
4. Backend (DB Handler) executes an `INSERT` statement into the expenses table.
5. Database confirms the insertion and returns the new ID.
6. Backend returns the created Expense object to the Frontend.
7. Frontend updates the UI to show the new expense and calls GET /expenses/debts to  
 refresh the debt summary.

## 9. Behavior Design

**State Machine: Debt Settlement**

![Debt Settlements State Machine](/report/pictures/debt-settlement-state-machine.png)

The debt calculation logic follows a specific state transformation flow:

1. **Initial State**: Raw lists of Expenses and Reimbursements from database
2. **Balance Calculation**:
    - For each expense, the payer's balance increases by the full amount
    - Each person in involved_people (potentially including the payer) has their  
    balance decreased by amount / len(involved_people)
    - For each reimbursement, from_person's balance increases and to_person's  
    balance decreases by the reimbursement amount
3. **Simplification Process**:
    - Users are separated into debtors (negative balance < -0.01) and creditors  
    (positive balance > 0.01)
    - Debtors are sorted in ascending order (most negative first)
    - Creditors are sorted in descending order (most positive first)
    - A greedy algorithm iteratively matches the largest debtor with the largest creditor
    - Each iteration settles the minimum of the debtor's debt and creditor's credit
    - Balances are updated and settled parties are removed from consideration
4. **Final State**: A simplified list of debts representing "who owes whom" with minimal  
 transactions

## 10. Security Design

- Authentication: Token-based authentication using custom session tokens stored in the  
sessions table.
- Password Storage: Passwords are hashed using SHA-256 with a unique  
randomly-generated salt (16-byte hexadecimal) per user before storage.
- Data Isolation: All API endpoints require a valid Bearer token in the Authorization header.  
The backend enforces strict checks ensuring users can only access data belonging to  
their specific house_id.

**Implementation Decisions**

- FastAPI: Chosen for high performance, automatic OpenAPI documentation (Swagger UI),  
and native support for asynchronous operations.
- Streamlit: Selected for the frontend to enable rapid prototyping and development of  
data-driven dashboards without complex JavaScript frameworks.
- SQLite: Used for data persistence due to its serverless nature, zero configuration, and  
suitability for the project's scale and portability requirements.
- Pydantic: Utilized for data validation and serialization, ensuring type safety across API  
boundaries.

This chapter explains the strategies and design decisions used to meet the requirements  
identified in the analysis.
