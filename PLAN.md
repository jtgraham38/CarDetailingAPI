# Project Plan: Laravel API for Xelerate Auto Spa (with Nuxt.js Integration)

## Overview
This plan focuses exclusively on the development of the Laravel RESTful API for Xelerate Auto Spa. The API will serve as the backend for a Nuxt.js frontend, providing all business logic, data management, and secure endpoints required for car detailing businesses and their customers. The Nuxt.js frontend will interact with the API via HTTP requests for all features.

---

## 1. API Architecture & Technology Stack
- **Framework:** Laravel (latest LTS)
- **Database:** MySQL or PostgreSQL
- **Authentication:** JWT or OAuth2 (for business owners)
- **ORM:** Eloquent
- **API:** RESTful, JSON responses
- **Testing:** PHPUnit
- **Security:** CSRF, XSS, SQL Injection protection, rate limiting, CORS
- **Documentation:** OpenAPI/Swagger for endpoint contracts (for Nuxt.js integration)

---

## 2. Data Models & Relationships
Refer to the ER diagram for detailed relationships. Key models:
- **User:** Authenticated business owners
- **Business:** Profile, contact info, owned by User
- **Service (Product):** Offered by Business, includes name, descriptions, price. Customers see these as "products" (not "packages").
- **Service Grouping:** API must support grouping services under a single price for customer-facing booking flows. Groupings are internal, not exposed as "packages" to customers.
- **Appointment:** Linked to Business, Customer, and Services; includes date, time, location, notes. Detailer will input appointment duration when approving.
- **Customer:** Info provided during quote/booking (no account required)
- **Quote:** (Can be implemented as Appointment with status or separate table; see ERD note). Customers can attach notes to quote requests.

---

## 3. Core API Endpoints (for Nuxt.js Frontend)

### Authentication
- `POST /api/login` — Business owner login (returns JWT/token)
- `POST /api/logout` — Logout (invalidate token/session)
- `GET /api/me` — Get authenticated user info

### Business Management
- `GET /api/businesses/{id}` — Get business profile
- `PUT /api/businesses/{id}` — Update business profile

### Services & Price Packages
- `GET /api/businesses/{id}/services` — List services for a business
- `POST /api/businesses/{id}/services` — Add new service
- `PUT /api/services/{id}` — Update service
- `DELETE /api/services/{id}` — Remove service
- `GET /api/services/{id}/price-packages` — List price packages for a service
- `POST /api/services/{id}/price-packages` — Add price package
- `PUT /api/price-packages/{id}` — Update price package
- `DELETE /api/price-packages/{id}` — Remove price package

### Quote Management
- `POST /api/quotes` — Customer generates quote (no login)
- `GET /api/quotes/{id}` — Get quote status/details
- `POST /api/quotes/{id}/approve` — Business approves quote (inputs appointment duration)
- `POST /api/quotes/{id}/reject` — Business rejects quote

### Product/Service Management
- `GET /api/businesses/{id}/products` — List products/services for a business (grouped as needed)
- `POST /api/businesses/{id}/products` — Add new product/service
- `PUT /api/products/{id}` — Update product/service
- `DELETE /api/products/{id}` — Remove product/service
- (Internal grouping logic for upselling/future package support)

### Appointment Calendar
- `GET /api/appointments` — Business views all appointments (with date, time, address, notes, services, etc.)
- `GET /api/appointments?date=YYYY-MM-DD` — Filter appointments by day for calendar preview
- `GET /api/appointments/{id}` — Get appointment details
- `POST /api/appointments` — Customer books appointment (no login)
- `PUT /api/appointments/{id}` — Reschedule appointment
- `DELETE /api/appointments/{id}` — Cancel appointment

### Customer Details
- `GET /api/customers/{id}` — View customer info (for business)
- `GET /api/customers/{id}/history` — View customer service/appointment history

---

## 4. Backend Implementation Requirements
- **Products/Services:** API must present all bookable options as "products" to customers, with support for grouping and future upselling logic.
- **Notes:** Quotes and appointments must support customer-attached notes (text field in DB and API).
- **Appointment Duration:** API must allow detailer to specify duration when approving an appointment.
- **Calendar Data:** Endpoints must provide all necessary data for frontend calendar views (date, time, address, notes, services, etc.).
- **No Brand Customization:** No color/branding customization logic in API for sprint one.
- **Monetization:** Design API and data models to be extensible for future freemium/monetization features (e.g., feature flags, subscription status), but do not implement in sprint one.
- **Recurring Appointments:** Plan data models and endpoints to allow for recurring appointments in future sprints (e.g., recurrence rules, parent/child appointment relationships).
- **Consistent JSON structure** for all responses (success, error, validation)
- **Authentication middleware** for protected endpoints
- **CORS enabled** for Nuxt.js frontend communication
- **Comprehensive validation** and clear error messages
- **Pagination and filtering** for list endpoints (e.g., appointments, services)
- **OpenAPI/Swagger documentation** for all endpoints (to support frontend integration)
- **Unit and integration tests** for all endpoints
- **Error handling:** Standardized error codes/messages for frontend consumption

---

## 5. Milestones & Timeline (Backend Focus)
1. **Project Setup & Planning**
   - Repo setup, environment configuration, CI/CD pipeline
2. **Database Schema & Migrations**
   - Implement schema per ER diagram, migrations, seeders
   - Ensure models support notes, grouping, and extensibility for monetization/recurring
3. **Authentication & User Endpoints**
   - Secure login/logout, user info
4. **Product/Service, Quote, and Appointment Endpoints**
   - CRUD for all business data, booking, and calendar flows
   - Support for notes, grouping, and appointment duration
5. **Customer Info Endpoints**
   - For business dashboard
6. **API Documentation**
   - OpenAPI/Swagger for all endpoints
7. **Testing & QA**
   - Unit/integration tests, bug fixing

---

## 6. Best Practices & Guidelines
- **Code Quality:** Follow PSR-12 (PHP) standards
- **Version Control:** Use Git, feature branching, code reviews
- **Documentation:** Inline code comments, API docs, README updates
- **Security:** Sanitize inputs, secure authentication, HTTPS-only, CORS
- **Scalability:** Modular code, efficient DB queries, caching where needed
- **Testing:** Maintain high test coverage, automate tests in CI

---

## 7. Collaboration & Handover
- Coordinate with Nuxt.js frontend team on endpoint contracts and error handling
- Regular syncs to resolve integration issues
- Handover of API documentation and test credentials for frontend development

---

This plan ensures the Laravel API is robust, secure, and fully aligned with the requirements of the Nuxt.js frontend, supporting all business and customer flows for Xelerate Auto Spa, and is designed for future extensibility (monetization, recurring appointments, upselling, etc.). 