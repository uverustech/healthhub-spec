# Ashia Portal: Technical Architecture & System Design

As the Engineering Lead, I’ve architected Ashia Portal to be a high-performance, fault-tolerant, and strictly typed ecosystem. We prioritize security, data integrity, and a modern developer experience through a decoupled Next.js + Laravel architecture.

---

## 🏗️ System Architecture

### 1. Backend: Laravel 12 (Service-Oriented Monolith)
The backend is built on the latest **Laravel 12** framework, adopting a **Service Layer Pattern** to ensure business logic remains testable and decoupled from the transport layer (HTTP Controllers).

*   **Authentication**: Implemented via **Laravel Sanctum** for stateful SPA authentication and API token management.
*   **Authorization**: Granular **Role-Based Access Control (RBAC)** using `spatie/laravel-permission` on the backend, complemented by **Role Guards** on the frontend to ensure a secure, role-aware user experience.
*   **Financial Integrity**: We use `decimal` types for all monetary columns (Premiums, Payouts, Claims) to avoid floating-point inaccuracies. 
*   **Infrastructure**: 
    *   **PostgreSQL**: The primary relational database (leveraging advanced indexing and data integrity features).
    *   **Redis**: High-speed caching and queue management for asynchronous tasks (e.g., SMS/Email notifications).
    *   **Dockerized Environment**: Fully containerized with dedicated services for `app`, `web`, `queue-worker`, `cron`, and `redis`. We are also exploring **Caddy** as a modern, automated reverse proxy solution.

### 2. Frontend: Next.js 15 (App Router & React 19)
The frontend utilizes a modern **Next.js 15** stack, leveraging Server Components for performance and Client Components for rich interactivity.

*   **State Management**:
    *   **Server State**: Managed by **TanStack Query (React Query) v5**, providing robust caching, background revalidation, and optimistic updates.
    *   **Client State**: Lightweight **Zustand** stores for transient UI states (e.g., multi-step registration forms, OTP flows).
*   **UI Architecture**: Built with **Tailwind CSS** and **Radix UI** primitives, ensuring high accessibility (WAI-ARIA) and a consistent design language.
*   **Form Governance**: Standardized on **React Hook Form** paired with **Zod** for compile-time and runtime type safety on all user inputs.

---

## 🛠️ Key Technical Decisions

### High-Integrity Sequencing & Unique Identifiers
Instead of relying solely on auto-incrementing IDs for public-facing entities, we implemented a custom **Sequences** system. This ensures that identifiers like `PA-Codes` and `Claim-Numbers` follow a strict, gapless, and predictable format required for auditing.

We are also experimenting with **ULID** (Universally Unique Lexicographically Sortable Identifier) to explore sequential, globally unique IDs as an alternative to standard UUIDs, maintaining performance while ensuring uniqueness across distributed systems.

### Observability & Resilience
*   **Error Tracking**: Unified **Sentry** integration across both the Laravel API and the Next.js frontend, including Edge and Server-side monitoring.
*   **Audit Trails**: Every sensitive mutation is automatically tracked via `spatie/laravel-activitylog`, providing a tamper-evident record of who did what and when.
*   **Soft Deletes**: Implemented on all primary records (Enrollees, HCPs) to ensure data recoverability and referential integrity for historical reporting.

### Media & Storage Evolution
*   **Cloudflare (S3-Compatible)**: Moving away from Cloudinary to **Cloudflare R2** to leverage AWS S3 compatibility. This strategic shift provides greater flexibility to explore other storage providers while optimizing delivery and costs.
*   **Remita Gateway**: Direct integration for RRR (Remita Retrieval Reference) generation and bulk bank transfers, strictly handled via a dedicated `RemitaService` to isolate third-party dependency logic.

---

## 🧪 Engineering Standards

*   **Strict Typing**: Full TypeScript implementation on the frontend and strict PHP 8.2+ typing (with return types) on the backend.
*   **Asynchronous Excellence**: Heavy use of Laravel Queues for non-blocking operations, ensuring the API remains responsive during high-volume registration events.
*   **Testing Philosophy**:
    *   **Backend**: PHPUnit for Unit and Feature tests.
    *   **Performance**: **k6** scripts integrated for load testing critical paths like Claim submission and Concurrent Registrations.
    *   **Frontend**: ESLint/Prettier enforcement via Husky pre-commit hooks to maintain code consistency.

---

## 📦 Deployment & DevOps
The application is deployment-ready via **Docker Compose**, supporting multiple environments (Dev, Staging, Prod). The production-grade [Dockerfile](file:///Users/deji/Workspace/uverus/_uverustech/ashia-portal/Dockerfile) utilizes multi-stage builds to minimize image sizes and maximize security by excluding development dependencies in the final artifact.
