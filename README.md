<div align="center">

# 📚 Codex Digital Library API

**A clean, layered digital library management system built with ASP.NET Core and Onion Architecture.**

---

## Overview

**Codex Digital Library API** is a backend system for managing a digital library — books, authors, borrowing records, and users — built around **Clean (Onion) Architecture** and **Domain-Driven Design (DDD)** principles. It cleanly separates business rules from infrastructure concerns, uses **CQRS via MediatR** to handle reads and writes independently, and enforces validation, security, and consistency through well-defined pipeline behaviors.

The goal of this project is to demonstrate a production-style .NET backend: testable, maintainable, and organized around business capability rather than technical layers alone.

---

[![.NET](https://img.shields.io/badge/.NET-9-512BD4?logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/)
[![C#](https://img.shields.io/badge/C%23-100%25-239120?logo=csharp&logoColor=white)](https://learn.microsoft.com/dotnet/csharp/)
[![SQL Server](https://img.shields.io/badge/SQL%20Server-EF%20Core%209-CC2927?logo=microsoftsqlserver&logoColor=white)](https://www.microsoft.com/sql-server)
[![Architecture](https://img.shields.io/badge/Architecture-Onion%20%2F%20Clean-2E8B57)]()
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](#license)

</div>


## 🧱 Architecture

The solution follows **Onion (Clean) Architecture**, where dependencies always point inward toward the domain core.

```
┌─────────────────────────────────────────────────────────┐
│ Presentation                                             │
│ (Web API / Endpoints)                                    │
│  ▼                                                        │
│ ┌───────────────────────────────────────────────────┐   │
│ │ Infrastructure                                      │   │
│ │ (EF Core, SQL Server, Identity)                     │   │
│ │  ▼                                                  │   │
│ │ ┌─────────────────────────────────────────────┐   │   │
│ │ │ Application                                   │   │   │
│ │ │ (CQRS Handlers, MediatR, DTOs)                │   │   │
│ │ │  ▼                                            │   │   │
│ │ │ ┌───────────────────────────────────────┐   │   │   │
│ │ │ │ Domain                                  │   │   │   │
│ │ │ │ (Entities, Aggregates, Specifications)  │   │   │   │
│ │ │ └───────────────────────────────────────┘   │   │   │
│ │ └─────────────────────────────────────────────┘   │   │
│ └───────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Project layers

| Layer | Responsibility |
|---|---|
| **`DigitalLibrary.Domain`** | The innermost core. Entities, value objects, domain exceptions, and repository abstractions. Zero external dependencies. |
| **`DigitalLibrary.Application`** | Application business workflows. CQRS via **MediatR**, validation pipelines (**FluentValidation**), mapping, and service interfaces. Depends only on `Domain`. |
| **`DigitalLibrary.Infrastructure`** | Persistence (EF Core + SQL Server), caching, cryptography, and other external service integrations. Implements the abstractions defined in the inner layers. |
| **`DigitalLibrary.API` / Presentation** | Entry point (ASP.NET Core Web API). Routing, middleware, JWT configuration, Swagger/OpenAPI docs, and dependency injection wiring. |

This separation keeps the core business logic framework-agnostic and easy to test, while infrastructure details (database, auth, external services) stay swappable.

---

## 🛠️ Tech Stack & Patterns

- **Framework:** .NET 9 (ASP.NET Core Web API)
- **Database & ORM:** SQL Server via Entity Framework Core 9
- **Architecture & Patterns:**
  - Onion (Clean) Architecture for strict separation of concerns
  - CQRS pattern via `MediatR` to isolate reads from writes
  - Repository & Unit of Work patterns for data access and transaction boundaries
  - Domain-Driven Design (DDD) tactical patterns — entities, aggregates, value objects
- **Validation:** FluentValidation, wired through MediatR pipeline behaviors
- **Security:** JWT authentication with role-based authorization
- **Docs:** Swagger / OpenAPI

---

## 📂 Directory Structure

```
Codex-Digital-Library-API
│
├── DigitalLibrary.Domain
│   ├── Common           # Base Entity, ValueObject implementations
│   ├── Entities         # Book, Author, BorrowRecord, User
│   ├── Exceptions       # Domain-specific exceptions
│   └── Interfaces       # IGenericRepository, IBookRepository, IUnitOfWork
│
├── DigitalLibrary.Application
│   ├── Books            # Commands, Queries, Handlers, DTOs
│   ├── Borrowing        # Business rules for issuing/returning books
│   ├── Behaviors        # MediatR pipeline behaviors (Logging, Validation)
│   └── Common           # Mapping profiles, shared interfaces
│
├── DigitalLibrary.Infrastructure
│   ├── Persistence      # ApplicationDbContext, Migrations
│   ├── Repositories     # Concrete repository implementations
│   └── Services         # TokenService, DateTimeProvider
│
├── DigitalLibrary.Console   # Console runner / entry point utilities
│
└── DigitalLibrary.sln
```

---

## ✨ Core Features

- 📖 **Book management** — catalog, search, and organize books and authors
- 🔁 **Borrowing workflow** — issue and return records with business-rule enforcement
- 🔐 **Authentication & authorization** — JWT-based auth with role-based access control
- ✅ **Validation pipeline** — request validation decoupled from handlers via FluentValidation + MediatR behaviors
- 🧩 **Swagger/OpenAPI** — self-documenting API surface for exploration and testing

---

## 🚀 Getting Started

### Prerequisites

- [.NET 9 SDK](https://dotnet.microsoft.com/download)
- SQL Server (LocalDB, Express, or full instance)
- A REST client (Swagger UI, Postman, etc.)

### Setup

```bash
# Clone the repository
git clone https://github.com/ar-refai/Codex-Digital-Library-API.git
cd Codex-Digital-Library-API

# Restore dependencies
dotnet restore

# Update the connection string in appsettings.json (Infrastructure/API project)

# Apply EF Core migrations
dotnet ef database update --project DigitalLibrary.Infrastructure

# Run the API
dotnet run --project DigitalLibrary.API
```

Once running, open the Swagger UI (typically `https://localhost:{port}/swagger`) to explore and test the available endpoints.

---

## 🗺️ Roadmap

- [ ] Expand automated test coverage (unit + integration)
- [ ] Add pagination and filtering to catalog endpoints
- [ ] Containerize with Docker for one-command setup
- [ ] CI pipeline for build/test on push

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome. Feel free to open an issue or submit a pull request.

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">
Built by <a href="https://github.com/ar-refai">ar-refai</a>
</div>
