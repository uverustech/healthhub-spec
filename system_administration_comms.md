# System Administration & Communications

This document covers the foundational systems that enable the Ashia Portal to function securely and allow its users to communicate and manage medical services.

## 1. Internal Messaging System
The portal includes a private messaging feature to facilitate communication between different user roles (e.g., an HCP and a Claims Officer).

- **Purpose**: Clarifying rejected claims, coordination for urgent PA requests, and administrative announcements.
- **Workflow**: 
    - Users can send messages to specific individuals or roles.
    - Message history is preserved for audit and accountability.
    - (Optional) SMS integration for critical alerts (birthday messages, high-priority notifications).

---

## 2. Role-Based Access Control (RBAC)
The system is strictly governed by roles and permissions to ensure data security and clinical integrity.

### Major Internal Roles
- **Super Admin**: Full system access, including configuration, audit logs, and user management.
- **Claims Officer**: Responsible for reviewing and approving/rejecting medical claims.
- **Registration Officer**: Manages enrollee registration, bio-data collection, and ID cards.
- **Accountant (Finance)**: Manages bulk transfers, reconciliations, and financial reporting.
- **Account Management**: Handles high-level approval workflows like Pre-Authorization (PA) requests.

### Technical Implementation
- Roles are managed via the `UserController` and `RolesController`.
- Permissions are enforced at the API route level using middleware and at the UI level to hide/show specific features.

---

## 3. Medical Services & Tariff (The Catalogue)
The "Tariff" is the system's price list for all medical services, drugs, and laboratory tests.

- **Service Categories**: Services are grouped into categories (e.g., General Consultation, Primary Care, Secondary Care, Drugs, Radiology).
- **Service Items**: Individual items within a category have a predefined `amount` and `code`.
- **Diagnosis Codes**: Standardized codes used across the system to ensure clinical consistency in encounters, referrals, and claims.

| Entity | Description |
| :--- | :--- |
| **Service Category** | High-level grouping of services. |
| **Service Item** | The specific billable item (e.g., "Paracetamol 500mg"). |
| **Pricing** | The negotiated amount that an HCP can claim for a service item. |

### Importance in Claims
When an HCP creates a claim, they **must** select items from this authorized list. The system automatically calculates the claim amount based on these predefined prices, preventing billing errors and fraud.

---
*Documentation Version: 1.0 (2026-03-26)*
