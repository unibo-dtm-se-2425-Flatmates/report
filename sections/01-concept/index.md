---
title: Concept
has_children: false
nav_order: 2
---

# Concept

## 1) Type of Product

Flatmates is a web application with a graphical user interface (GUI) designed to simplify shared living by centralizing household management. Accessible through any standard web browser on desktop and mobile devices, Flatmates enables flatmates to organize schedules, manage expenses, coordinate shopping, and configure house settings through a single, intuitive platform. The application functions as a unified household management system that consolidates traditionally fragmented workflows into one cohesive solution.

## 2) Idea

The motivation behind Flatmates stems from the practical challenges of coordinating life with multiple housemates. Managing shared calendars, tracking expenses, splitting bills, and coordinating household shopping typically relies on fragmented tools (email, messaging apps, spreadsheets) or manual communication, often leading to confusion, disputes, and inefficiency. Flatmates addresses these critical gaps by providing a unified, transparent platform where all household activities, financial transactions, and coordination happen in one accessible location, promoting accountability and reducing misunderstandings among residents.

## 3) Key Features

- **Calendar**: Schedule and track shared events, cleaning duties, and household activities with real-time visibility.
- **Shopping List**: Collaborative shopping list to coordinate household purchases and prevent duplicate buying.
- **Expense Manager**: Track shared expenses and debt simplifier for flatmates, allowing transparent financial records.
- **House Settings**: Configure house details, manage user profiles, and customize system preferences.

## 4) Use Cases and User Interactions

### 4.1 Primary Users and Roles

At the moment, Flatmates has a single user role. All members are standard users with equal permissions and access to all main features. The system does not include administrators or guests, as login is required to use the application; in future versions, administrator roles with extended privileges for managing the household and other users are planned.

### 4.2 Usage Frequency

- Daily interactions: Checking the shared calendar, adding items to the shopping list, or viewing recent expenses.
- Weekly activities: Reviewing expense summaries, settling bills, and planning household events or cleaning schedules.

### 4.3 Device Access and Interaction Methods

Users interact with Flatmates through desktop browsers (Chrome, Firefox, Safari, Edge) for detailed expense management and administrative tasks. Responsive design ensures a seamless experience across all screen sizes, and the web-based approach eliminates platform dependency while maintaining accessibility from any internet-connected device; in the future, this implementation could be embedded in a mobile application.

## 5) Technical Implementation

- Backend: FastAPI with Uvicorn.
- Frontend: Streamlit framework.
- Database: SQLite.
- Deployment: Streamlit Community Cloud (frontend), Railway (backend).

