---
title: Future work
has_children: false
nav_order: 13
---

# Future Work

## 1. Known Issues

1. **Session Persistence:** The Streamlit frontend occasionally loses session state upon browser refresh, requiring the user to log in again.
2. **Frontend lacks mobile responsiveness**; Streamlit's table widgets become cramped on smaller screens, obscuring expense details.
3. **Authentication weakness**: Usernames are non-unique at the database level. Password registration skips validation for minimum complexity or format rules. Users cannot recover lost passwords. The system permits identical username-password combinations.
4. **Mobile Responsiveness:** While functional on mobile, the Streamlit table widgets can be cramped on smaller screens, making it hard to view detailed expense splits.

## 2. Future Improvements

This section outlines the roadmap for transforming **Flatmates** from a working prototype into a robust, production-ready product.

### 2.1 Short-Term

- **Authentication**: enforce unique usernames with database constraints, add password validation rules for minimum length and complexity, integrate password recovery via email or security questions.
- **User Role**: Implement admin user role for changing house settings and create houses to invite normal users.
- **Developing improvements**: CI workflow + precommit + automatic reviewer assignee for the PR + mandatory PR approval and a new test approach.

### 2.2 Medium-Term

- **Recurring Expenses:** Add logic to automatically generate expenses for fixed monthly costs like Rent or Internet. Users would define the recurrence rule (e.g., "every 1st of the month") and the system creates the expense entry automatically.
- **Export and Reporting:** Allow users to export expense reports (CSV/PDF) for tax or financial documentation purposes. This is particularly useful for shared houses with complex tax arrangements.
- **Cleaning/Trash Schedule:** Implement a new feature under the calendar section that allow the user to schedule a cleaning plan among the flatmates that create automatic events assigned to specific flatmates, with reminders for each flatmate when it's their turn. And also a trash section to allow the flatmates to remember who has to dispose of the trash, what kind of trash it is and when it's their turn.
- **Chore Completion Tracking:** Enhance the calendar to include a "Completion Status" for chores, allowing flatmates to mark tasks as done and track accountability.
- **Push Notifications:** Integrate a service to notify users via email/SMS when they are assigned a new chore or debt.

### 2.3 Long-Term

- **Receipt Scanning:** Implement OCR (Optical Character Recognition) to allow users to take a photo of a receipt and automatically populate the expense form, extracting item names and amounts.
- **Chore Gamification:** Introduce a point system where completing chores earns "Karma Points" that can be redeemed for "Skip Chore" passes, adding a fun incentive to household maintenance.
- **Native Mobile App:** Transition the frontend from Streamlit (which is optimized for data apps) to React Native. This would provide a true mobile-first experience with offline capabilities and smoother animations suitable for on-the-go expense logging.
- **Analytics Dashboard:** Provide flatmates with insights into spending patterns (e.g., "We spend €X per month on groceries"). This could inform budgeting discussions.

## 3. Conclusion
The **Flatmates** project demonstrates the value of combining technical implementation with domain expertise. Giacomo's engineering rigor ensured the system was built on a solid technical foundation, while Matteo's business logic ensured the product actually solved real problems for real users. Even if the current version presents some limitations, the modular architecture and clean code make it straightforward to add the features outlined above. The project is well-positioned for future growth and could serve as a foundation for a commercial product targeting the shared housing market.

