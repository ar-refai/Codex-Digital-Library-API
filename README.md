┌─────────────────────────────────────────────────────────┐
   │                      Presentation                       │
   │                   (Web API / Endpoints)                 │
   │                            ▼                            │
   │   ┌─────────────────────────────────────────────────┐   │
   │   │                 Infrastructure                  │   │
   │   │        (EF Core, SQL Server, Identity)          │   │
   │   │                        ▼                        │   │
   │   │   ┌─────────────────────────────────────────┐   │   │
   │   │   │               Application               │   │   │
   │   │   │      (CQRS Handlers, MediatR, DTOs)     │   │   │
   │   │   │                    ▼                    │   │   │
   │   │   │   ┌─────────────────────────────────┐   │   │   │
   │   │   │   │             Domain              │   │   │   │
   │   │   │   │  (Entities, Aggregates, Specs)  │   │   │   │
   │   │   │   └─────────────────────────────────┘   │   │   │
   │   │   └─────────────────────────────────────────┘   │   │
   │   └─────────────────────────────────────────────────┘   │
   └─────────────────────────────────────────────────────────┘



   ### 🧱 Project Layers

1. **`DigitalLibrary.Domain` (Core)**
   * The innermost layer containing corporate business rules, entities, value objects, domain exceptions, and repository abstractions.
   * Completely isolated; has **zero** dependencies on external frameworks or other projects.

2. **`DigitalLibrary.Application` (Core)**
   * Defines application-specific business workflows. 
   * Implements **CQRS via MediatR**, validation pipelines (FluentValidation), mapping definitions, and application service interfaces.
   * Depends only on the *Domain* layer.

3. **`DigitalLibrary.Infrastructure` (Outer)**
   * Manages external concerns such as database persistence (Entity Framework Core, SQL Server), caching, cryptography, and external mailing services.
   * Contains concrete implementations of abstractions defined in lower layers.

4. **`DigitalLibrary.Presentation` or `DigitalLibrary.API` (Outer)**
   * The entry point of the application (ASP.NET Core Web API).
   * Responsible for routing, middlewares (global exception handling), JWT configuration, Swagger/OpenAPI documentation, and dependency injection wire-up.

---

## 🛠️ Tech Stack & Patterns

* **Framework:** .NET 9 (ASP.NET Core Web API)
* **Database & ORM:** SQL Server via Entity Framework Core (EF Core 9)
* **Design Patterns & Architecture:**
  * **Onion (Clean) Architecture** for strict separation of concerns.
  * **CQRS Pattern** using `MediatR` to isolate read operations from write actions.
  * **Repository & Unit of Work Patterns** to encapsulate data access and transaction boundaries.
  * **Domain-Driven Design (DDD)** tactical patterns (Entities, Aggregates, Value Objects).
* **Validation:** `FluentValidation` implemented via pipeline behaviors.
* **Security:** JWT Authentication and Role-Based Authorization.

---

## 📂 Directory Structure

```text
📁 digital-library-onion-architecture
│
├── 📁 src
│   ├── 📁 DigitalLibrary.Domain
│   │   ├── 📁 Common           # Base Entity, ValueObject implementations
│   │   ├── 📁 Entities         # Book, Author, BorrowRecord, User
│   │   ├── 📁 Exceptions       # Domain-specific exceptions
│   │   └── 📁 Interfaces       # IGenericRepository, IBookRepository, IUnitOfWork
│   │
│   ├── 📁 DigitalLibrary.Application
│   │   ├── 📁 Books            # Commands, Queries, Handlers, and DTOs per feature
│   │   ├── 📁 Borrowing        # Business rules for issuing/returning books
│   │   ├── 📁 Behaviors        # MediatR pipeline behaviors (Logging, Validation)
│   │   └── 📁 Common           # Mapping profiles, Interfaces
│   │
│   ├── 📁 DigitalLibrary.Infrastructure
│   │   ├── 📁 Persistence      # ApplicationDbContext, Migrations
│   │   ├── 📁 Repositories     # Concrete Repository implementations
│   │   └── 📁 Services         # TokenService, DateTimeProvider
│   │
│   └── 📁 DigitalLibrary.API
│       ├── 📁 Controllers      # BaseApiController, BooksController, etc.
│       ├── 📁 Middleware       # ExceptionHandlingMiddleware
│       ├── 📄 appsettings.json
│       └── 📄 Program.cs
│
└── 📄 DigitalLibrary.sln
