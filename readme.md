A robust, production grade backend service built from scratch in Rust, designed to handle user subscriptions, email verification flows, and asynchronous communications. Far beyond a basic CRUD application, this project focuses on high availability, enterprise level testing, type driven design, and comprehensive observability for cloud native environments.

---

## 🚀 Key Engineering Highlights

### 1. Advanced Test Isolation & Infrastructure

- **Parallel Test Isolation**: Overcame the challenge of database driven integration tests acting as shared global state by programmatically spawning a unique, dynamically named logical PostgreSQL database with automated runtime migrations for every single test run.
- **Zero-Mocks Approach**: Utilizes black box integration testing against a real database engine and external HTTP endpoints rather than fragile stubs, ensuring high fidelity with production behavior.

### 2. Type-Driven Domain Modeling (Make Illegal States Unrepresentable)

- **New-Type Pattern**: Avoids primitive obsession by wrapping raw inputs into domain specific types (`SubscriberName`, `SubscriberEmail`).
- **Parse, Don't Validate**: Implements strict validation routines (`parse` and `TryFrom`) as constructors, ensuring that unvalidated or malicious data can never cross domain boundaries or reach the database layer.

### 3. Distributed Observability & Telemetry

- **Structured Logging & Tracing**: Replaced traditional loggers with the modern `tracing` ecosystem, capturing contextual key-value metadata across asynchronous tasks.
- **Request Correlation**: Automatically propagates unique `request_id`s across incoming HTTP requests and database operations to easily correlate logs during concurrency.
- **Secure Credential Masking**: Leverages the `secrecy` crate (`Secret<T>`) to prevent sensitive variables like database credentials from accidentally leaking into logs or traces via debug formatting.

### 4. Resilient Database & Compile-Time Safety

- **Compile-Time SQL Verification**: Utilizes `sqlx` query macros with offline mode caching, enabling the Rust compiler to verify raw SQL queries against the actual database schema at compile-time.
- **Connection Pooling**: Manages database access concurrently and safely using `sqlx::PgPool` and lazy connection initialization.

### 5. Production Containerization & CI/CD

- **Optimized Docker Builds**: Engineered lightweight, security hardened production container images (~125MB) using Debian slim base images and multi-stage builds.
- **Dependency Layer Caching**: Accelerated massive Rust release compilations in Docker using `cargo-chef` recipe preparation to cache dependency trees effectively.
- **Automated Quality Gates**: Integrated static analysis (`clippy`), code formatting checks (`rustfmt`), security vulnerability audits (`cargo-audit`), and test suites into continuous integration pipelines.

---

## 🛠️ Tech Stack & Ecosystem

- **Language**: Rust (Stable Edition)
- **Web Framework**: `actix-web` (built on the `tokio` asynchronous runtime)
- **Database & Migrations**: PostgreSQL managed via `sqlx`
- **Validation & Parsing**: `validator`, `unicode-segmentation`
- **Telemetry**: `tracing`, `tracing-actix-web`, `tracing-bunyan-formatter`
- **Security & Config**: `secrecy`, `config`, `cargo-audit`
- **Testing & Quality**: `claims`, `quickcheck`, `fake`, `cargo-chef`

How to build

Launch a (migrated) Postgres database via Docker:
./scripts/init_db.sh

Launch a Redis instance via Docker:
./scripts/init_redis.sh

Launch cargo:
cargo build

You can now try with opening a browser on http://127.0.0.1:8000/login after having launch the web server with cargo run.
There is a default admin account with password everythinghastostartsomewhere. The available entrypoints are listed in src/startup.rs

How to test

Launch a (migrated) Postgres database via Docker:
./scripts/init_db.sh

Launch a Redis instance via Docker:
./scripts/init_redis.sh

Launch cargo:
cargo test
