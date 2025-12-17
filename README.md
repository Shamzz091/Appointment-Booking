Appointment Booking System

A robust, multi-tenant appointment booking platform supporting business management, scheduling, lead tracking, and communications, built on PostgreSQL with advanced row-level security (RLS) and extensibility in mind.

Features:
-Multi-Tenant Business Management: Each business operates in an isolated, secure environment with unique staff, services, policies, and clients.
-Appointment Lifecycle: Reserve, confirm, reschedule, and cancel appointments, including soft-hold support and client self-service via secure tokens.
-Granular Staff Scheduling: Supports staff working hours, time off, and dynamic service-staff mapping for flexible assignments.
-Lead & CRM Management: Capture leads, track stages and sources, and manage engagement via activity logging and categorization.
-Resource Mapping: Assign and manage business resources, track availability to prevent booking conflicts.
-Row Level Security: Enforced throughout via PostgreSQL RLS, ensuring users can only access data related to their business.
-Communication & Automation: Centralized comms tracking for SMS, email, and voice notifications. Batch and individual policy management for reminders, deposits, and cancellations.
-Tokenized Appointment Access: Public functions allow clients to manage their appointments without login—from retrieval to rescheduling and cancellation.

Database Schema Overview:
The core architecture is defined in the Business Management System Schema:

Tables:
businesses, business_users: Multi-tenant business structure and user roles.
staff, staff_working_hours, staff_time_off: Staff details and availability.
services, service_staff, resources: Services, staff mapping, and optional resources linked to each business.
clients, leads, lead_activity: CRM and lead lifecycle tracking.
appointments, waitlist, comms: Scheduling system, waitlisting, and communications log.
policies, integrations: Per-business policy & integration configuration.
Schema Extensions: Uses pgcrypto (for UUID and random token generation) and btree_gist (for advanced indexing).

Security Model:
Multi-tenant Row Level Security is enforced throughout all core tables. Policies and helper functions ensure that users may only access or modify data linked to their businesses, utilizing role-based access and mapping via views such as v_user_business.

Multi-tenant Row Level Security and Business Access Policy manager show dynamic policy creation for all tables.
Data access and mutations are scoped via security-definer functions, leveraging the current authenticated user’s context.

Core Functions:
Business Configuration & Policy Management:
Functions allow users to create businesses, fetch or upsert policy sections (cancellation, deposit, reminders, quiet_hours), update multiple sections at once, and ensure default policies always exist.
Policies are always mapped to the current user's business context.

Appointment Management:
Token System: Unique token management for appointments enables secure, public, client-side appointment actions.
Management Operations:
Issue management tokens for client self-service.
Fetch appointment summary by token.
Reschedule or cancel appointments using tokens, with built-in checks on staff availability, time conflicts, and appointment state.
Automatic holds, confirmation logic, and release of expired holds ensure reliable scheduling.
See Appointment Management Token System and Appointment Booking System Functions.

Staff & Service Management:
Set staff working hours via upserts, ensuring normalization and safety checking.
Soft delete for services (deactivate without hard deletion).

Lead Management:
Update lead stages with checks on allowed enum values for category.

Performance & Indexing:
Indexes for frequently queried columns to optimize operational speed on large datasets.

Additional Utilities:
Placeholder/test operations for working with appointment tokens, confirming bookings, and querying recent changes (“debug” and manual operation scripts).

Getting Started:
Prerequisites
PostgreSQL (with superuser rights to install extensions)
pgcrypto, btree_gist extensions enabled

Usage:
Apply schema and all policy scripts in a fresh or existing Postgres database.
Use the API functions for all client, business, and system access—direct table writes should be avoided for safety.
Client-side applications may interact via SQL functions, using Supabase or direct Postgres connections as needed.

File Reference:
Business Management System Schema: Main schema, tables, enums, demo data
Multi-tenant Row level Security & Policy manager: All RLS setup and dynamic access rules
Appointment management token System: Secure appointment self-service implementation
Functions: Business configuration, staff/service management, appointment lifecycle management, and more

License:
MIT

