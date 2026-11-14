# Architecture and Design Boundaries

## Context

NetSchool Plus is designed as a modular Windows desktop application using C#, .NET and Entity Framework. The application emphasizes explicit dependencies and coherent business workflows rather than placing domain rules directly in UI event handlers.

## Logical responsibilities

1. **Presentation layer** — desktop views, shell navigation and user interaction.
2. **Application layer** — use-case orchestration such as sign-in, school configuration and billing operations.
3. **Domain models** — user authorization concepts and financial entities.
4. **Persistence** — Entity Framework-backed access to application data.
5. **Cross-cutting concerns** — audit logging, licensing, permissions and automated validation.

These are **logical boundaries**, not assertions that separate deployable services, assemblies or database schemas exist for every line item.

## Security foundations

Reported work includes users, roles, permissions, user sessions, audit data, login and forced password change. The objective is to avoid relying solely on whether a screen or navigation item happens to be visible: authorization should also be enforced when performing sensitive operations.

### Trade-offs and risks

- Desktop applications require careful handling of local access, secure configuration and credential storage.
- Role and permission behavior should be tested at both UI-navigation and application-operation boundaries.
- Audit trails are only as useful as their coverage, integrity and access restrictions.
- Implementation-level security claims require code review and threat modeling; this case study is not a security audit.

## Why dependency injection?

Dependency injection helps isolate persistence and infrastructure components from application workflows, simplifies substitution in tests and makes dependencies more visible during maintenance.

## Scope note

Descriptions are grounded in recorded project milestones; they do not imply independent verification of source code, security controls or operational deployment.
