---
title: Self-evaluation
has_children: false
nav_order: 12
---

# Self Evaluation

## 1. Giacomo Lancerin

This project represents one of my first experiences developing a full-stack web application completely autonomously, coming from a bachelor in Economics. Undertaking a project that required managing a backend API, database interactions, and a frontend interface was a significant challenge, but one that provided immense learning value.

Thanks to my internship as a Data Scientist and AI Developer at Technogym, I had the opportunity to learn many coding best practices from the professional world. This experience proved invaluable for the project, as I was able to apply industry-standard approaches to version control, deployment, documentation, and code quality throughout the development process.

### 1.1 Strengths

- **End-to-End Implementation:** I successfully designed and deployed a functioning application that solves a real-world problem. The system handles complex logic like expense splitting and debt simplification, which required careful algorithmic thinking.
- **Modern Tech Stack:** I utilized a modern, industry-standard stack (FastAPI, Streamlit, Pydantic) rather than relying on simpler, legacy tools. This required learning asynchronous programming concepts and stateless API design.
- **User-Centric Design:** I designed the application with careful attention to creating a simple, clear, and intuitive user experience. The interface was built to be easy to navigate, with straightforward workflows and immediate visual feedback. I approached the design by first building something I would want to use myself, ensuring that every feature felt natural and accessible.
- **Code Quality and Best Practices:** I maintained a clean separation of concerns by isolating the database logic (backend/db), API routes (backend/routers), and frontend UI (frontend/pages). This modularity makes the codebase maintainable and scalable. Drawing from my internship experience, I implemented proper naming conventions for branches, comprehensive documentation through docstrings and API specs, and established a robust pull request workflow with automated checks. These practices ensured code quality and facilitated collaboration.

### 1.2 Weaknesses

- **Testing Depth:** While I implemented basic unit tests for the backend logic, the frontend lacks automated testing. End-to-end tests (simulating a user logging in and adding an expense) are currently manual, which increases the risk of regressions during updates.
- **Database Scalability:** The current use of SQLite is excellent for development and small-scale deployment but is not suitable for high concurrency. As a file-based database, it risks "database locked" errors if multiple flatmates attempt to write data simultaneously.
- **Limited Error Feedback:** In some edge cases (e.g., network timeout between frontend and backend), the user receives a generic error message rather than specific guidance on how to retry.

### 1.3 What I Learned

The most significant learning from this project was understanding how to build an application from scratch starting from a real need. Throughout the development process, I made a conscious effort to adhere to software engineering best practices, from code organization and version control to testing and documentation, drawing on the professional standards I learned during my internship.

Perhaps most importantly, I learned to navigate the ecosystem of third-party services required to make an application truly accessible. I had to figure out and adapt to deployment platforms like Railway for the backend and Streamlit Community Cloud for the frontend, understanding their constraints and how to make them work together seamlessly. This process of transforming the project from an idea into a real product that others can actually use was challenging but essential.

The collaboration with Matteo also taught me how technical implementation and business requirements must work hand-in-hand—his input on user needs and edge cases helped me refine the technical implementation significantly.


