<div align="center">
  <h1>Email Newsletter</h1>
  <p>This repository contains my implementation of the project outlined in <strong>Zero to Production in Rust</strong> by <strong>Luca Palmieri</strong>.
  </p>
  <p>

<!-- prettier-ignore-start -->

[![Rust](https://github.com/marian-stanciulica/EmailNewsletter/actions/workflows/general.yml/badge.svg)](https://github.com/marian-stanciulica/EmailNewsletter/actions/workflows/general.yml)
[![Security audit](https://github.com/marian-stanciulica/EmailNewsletter/actions/workflows/audit.yml/badge.svg)](https://github.com/marian-stanciulica/EmailNewsletter/actions/workflows/audit.yml)
![MSRV](https://img.shields.io/badge/rustc-1.82+-ab6000.svg)


<!-- prettier-ignore-end -->
  </p>
</div>



## Overview

This project showcases a fully functional web application built using Rust while following **TDD**. It follows a layered architecture, with clearly separated concerns, and is designed with scalability, maintainability, and security in mind. The project demonstrates how to leverage Rust’s type system, error handling, concurrency model, and ecosystem of crates to build a robust backend service.

The following are the core technologies and libraries used in this project:

- **[Actix Web](https://actix.rs/)**: A fast and powerful web framework for building HTTP services in Rust.
- **[SQLx](https://github.com/launchbadge/sqlx)**: An async, compile-time checked SQL query builder and database driver.
- **[Tokio](https://tokio.rs/)**: An asynchronous runtime used to handle non-blocking I/O and concurrency.
- **[Docker](https://www.docker.com/)**: Containerization for deployment and local development environments.
- **[PostgreSQL](https://www.postgresql.org/)**: The database backend used for persistence.

## Project Structure

### Production

- **`src/authentication/`**: Logic for validating credentials, store credentials and change password.
- **`src/domain/`**: Core business logic and types, representing the domain model of the application.
- **`src/routes/`**: Handlers for HTTP requests such as user registration, login, and health checks.
- **`src/main.rs`**: Entry point for the application.
- **`src/configuration.rs`**: Configuration management for different environments (development, testing, production).
- **`src/email_client.rs`**: Email client type and sending email logic
- **`src/issue_delivery_worker.rs`**: Logic for delivering newsletter issues using a worker pool to dequeue task from the database, send the email and delete the task.
- **`src/startup.rs`**: Application bootstrap logic, including server setup, configuration, and dependency injection.

### Tests

- **`src/domain`**: 10 unit tests
- **`src/email_client.rs`**: 4 unit tests
- **`tests/api`**: 28 integration tests

## Running the Project Locally (MacOS)

### Prerequisites

- Rust (latest stable version)
- Docker
- PostgreSQL (if not using Docker)
- SQLx CLI (for running migrations)

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/marian-stanciulica/EmailNewsletter.git
   cd EmailNewsletter
   ```

2. Open Docker Desktop to start the docker daemon
3. Run the scripts to add and start the Postgres and Redis containers in Docker
	```bash
   ./scripts/init_db.sh
   ./scripts/init_redis.sh
   ```

4. Run the app
	```bash
	cargo run | bunyan
	```

5. Optionally, you can run the tests by running:
	```bash
	cargo test
	```

## Key Concepts and Learnings

### 1. Setting Up a Rust Web Server with Actix Web

- **Actix Web** was used as the foundation for building the HTTP server. The framework’s modularity and async capabilities allowed for the creation of a performant and scalable web API.
- **Routing**: Defined routes for handling different HTTP methods (GET, POST) and how to work with URL parameters and query strings in a type-safe manner.
- **Request/Response**: Actix Web's ability to easily parse incoming requests and format structured responses is central to the architecture.

### 2. Asynchronous Programming with Tokio

- A key focus was learning how to write non-blocking, async code with **Tokio**. This allowed the application to scale efficiently by leveraging Rust's concurrency model.
- Handling futures and async I/O was essential for managing multiple database connections, long-running tasks, and HTTP requests concurrently without sacrificing performance.

### 3. Database Integration with SQLx

- **SQLx** is a compile-time verified, async database driver. By writing queries in plain SQL, SQLx ensures queries are validated at compile time, preventing common runtime errors.
- **Migrations**: Managed schema changes using SQLx’s migration system.
- **Query Execution**: Implemented CRUD operations using SQLx’s query macros to perform parameterized queries safely and asynchronously.
- **Connection Pooling**: A connection pool optimized database performance, enabling connection reuse and reducing query overhead.

### 4. Authentication and Security

- A robust authentication system was implemented using hashed and salted passwords via the **argon2** crate and **JWT** tokens for session management.
- Secure communication was established by configuring TLS and serving the application over HTTPS.
- Input validation and sanitization were enforced to prevent common security vulnerabilities like SQL injection and XSS attacks.

### 5. Error Handling and Logging

- The project emphasizes Rust’s powerful error handling model, utilizing the `Result` and `Option` types to gracefully handle both expected and unexpected failures.
- Custom error types encapsulate failure scenarios, and consistent patterns are used for error propagation across async boundaries.
- **Tracing** was integrated to provide structured logging, improving debugging in local and production environments.

### 6. Testing and Continuous Integration

- Unit and integration tests ensure the application behaves as expected. Actix Web’s test utilities were used to mock requests and responses.
- Database testing was facilitated using Docker to spin up isolated PostgreSQL instances during tests, ensuring reproducible and side-effect-free tests.
- CI was set up using GitHub Actions, running tests automatically on each commit.

## Third-Party Libraries Used

### 1. **actix-web**: Web Framework

- **Crate**: [`actix-web`](https://crates.io/crates/actix-web)
- **Role**: A powerful, asynchronous web framework for building scalable HTTP servers and web services in Rust.
- **Usage**: Actix Web serves as the core framework to handle routing, middleware, and HTTP request/response lifecycles.

### 2. **sqlx**: Async SQL Toolkit

- **Crate**: [`sqlx`](https://crates.io/crates/sqlx)
- **Role**: An asynchronous SQL toolkit for Rust, providing a compile-time checked API for interacting with databases.
- **Usage**: Utilized for performing database operations (CRUD) with PostgreSQL, along with support for migrations and UUID/chrono types.

### 3. **reqwest**: HTTP Client

- **Crate**: [`reqwest`](https://crates.io/crates/reqwest)
- **Role**: A higher-level HTTP client for Rust that supports asynchronous requests and response handling.
- **Usage**: Used to make HTTP requests (such as REST API calls), handling JSON responses and managing cookies for stateful interactions.

### 4. **tokio**: Asynchronous Runtime

- **Crate**: [`tokio`](https://crates.io/crates/tokio)
- **Role**: An asynchronous runtime for Rust, providing tools for non-blocking I/O, concurrency, timers, and more.
- **Usage**: Used as the underlying async runtime for handling concurrent HTTP requests and managing non-blocking I/O tasks.

### 5. **serde**: Serialization and Deserialization

- **Crate**: [`serde`](https://crates.io/crates/serde)
- **Role**: A serialization/deserialization framework used for converting data between formats like JSON.
- **Usage**: Parsing JSON payloads from incoming requests and serializing responses in various formats.

### 6. **config**: Configuration Management

- **Crate**: [`config`](https://crates.io/crates/config)
- **Role**: A configuration manager for loading settings from environment variables, files, or other sources.
- **Usage**: Manage configuration options for different environments (development, testing, production) in a unified way.

### 7. **chrono**: Date and Time Handling

- **Crate**: [`chrono`](https://crates.io/crates/chrono)
- **Role**: A date and time library for Rust, providing precise handling of timestamps, dates, and time zones.
- **Usage**: Handling timestamps and date-time operations for logging, database records, and session expiration.

### 8. **tracing**: Structured Logging

- **Crate**: [`tracing`](https://crates.io/crates/tracing)
- **Role**: A structured logging framework that supports event-driven logging, useful in asynchronous applications.
- **Usage**: Provides structured logging for capturing detailed information about HTTP requests, errors, and database operations.

### 9. **tracing-subscriber**: Log Subscriber

- **Crate**: [`tracing-subscriber`](https://crates.io/crates/tracing-subscriber)
- **Role**: A library for subscribing to `tracing` logs and filtering/logging events.
- **Usage**: Subscribes to structured logs generated by `tracing`, filters them by level, and formats them for output.

### 10. **tracing-bunyan-formatter**: Bunyan Formatting for Logs

- **Crate**: [`tracing-bunyan-formatter`](https://crates.io/crates/tracing-bunyan-formatter)
- **Role**: Formats logs from `tracing` in a Bunyan-compatible format.
- **Usage**: Ensures that logs are formatted for structured log aggregators like Elasticsearch or Fluentd that use the Bunyan format.

### 11. **tracing-log**: Log Integration

- **Crate**: [`tracing-log`](https://crates.io/crates/tracing-log)
- **Role**: Provides compatibility between the `log` crate and `tracing`, allowing logs from dependencies to be captured by `tracing`.
- **Usage**: Bridges the `log` crate and `tracing`, so that logs from crates that use `log` are captured in structured form.

### 12. **secrecy**: Secret Management

- **Crate**: [`secrecy`](https://crates.io/crates/secrecy)
- **Role**: Provides abstractions to handle secret values like passwords or API tokens securely.
- **Usage**: Manages sensitive information (such as passwords or tokens) by ensuring they are handled securely in memory.

### 13. **tracing-actix-web**: Actix Web Tracing Integration

- **Crate**: [`tracing-actix-web`](https://crates.io/crates/tracing-actix-web)
- **Role**: Provides integration between `tracing` and Actix Web, enabling automatic tracing of HTTP request lifecycles.
- **Usage**: Automatically captures structured logs for Actix Web requests, including HTTP method, path, and response time.

### 14. **unicode-segmentation**: Unicode Text Segmentation

- **Crate**: [`unicode-segmentation`](https://crates.io/crates/unicode-segmentation)
- **Role**: Provides utilities for working with Unicode strings, such as splitting them into grapheme clusters.
- **Usage**: Used for handling Unicode text segmentation (e.g., splitting words into grapheme clusters) in user input or textual data.

### 15. **validator**: Input Validation

- **Crate**: [`validator`](https://crates.io/crates/validator)
- **Role**: Provides a declarative syntax for validating input data in Rust.
- **Usage**: Used for validating user inputs such as email addresses, passwords, and other fields in requests.

### 16. **rand**: Random Number Generation

- **Crate**: [`rand`](https://crates.io/crates/rand)
- **Role**: A random number generation library for Rust, providing utilities for generating secure random numbers.
- **Usage**: Used for generating secure random tokens, session identifiers, and cryptographic keys.

### 17. **thiserror**: Error Handling

- **Crate**: [`thiserror`](https://crates.io/crates/thiserror)
- **Role**: Provides a convenient way to derive error types in Rust.
- **Usage**: Used to define custom error types throughout the application, improving error handling and consistency.

### 18. **anyhow**: General Error Handling

- **Crate**: [`anyhow`](https://crates.io/crates/anyhow)
- **Role**: A library for error handling that simplifies working with errors in Rust, especially in larger applications.
- **Usage**: Provides an easy-to-use `Result` type for handling errors across the application, including both recoverable and unrecoverable errors.

### 19. **argon2**: Password Hashing

- **Crate**: [`argon2`](https://crates.io/crates/argon2)
- **Role**: Provides secure password hashing using the Argon2 algorithm.
- **Usage**: Hashes passwords before storing them in the database to ensure they are stored securely.

### 20. **actix-web-flash-messages**: Flash Messages for Actix Web

- **Crate**: [`actix-web-flash-messages`](https://crates.io/crates/actix-web-flash-messages)
- **Role**: A middleware for Actix Web that provides support for flash messages, which are temporary messages used to inform users about actions taken.
- **Usage**: Enables sending temporary notifications (like success or error messages) across redirects in a web application.

### 21. **actix-session**: Session Management

- **Crate**: [`actix-session`](https://crates.io/crates/actix-session)
- **Role**: Provides session management middleware for Actix Web, allowing you to store user sessions across requests.
- **Usage**: Utilizes Redis for session storage (when configured), enabling persistent user sessions for web applications.

### 22. **uuid**: Universally Unique Identifiers

- **Crate**: [`uuid`](https://crates.io/crates/uuid)
- **Role**: A library for generating and parsing UUIDs (Universally Unique Identifiers) in Rust.
- **Usage**: Used for generating unique identifiers for database records, session IDs, and other entities that require uniqueness.

### 23. **serde_json**: JSON Serialization and Deserialization

- **Crate**: [`serde_json`](https://crates.io/crates/serde_json)
- **Role**: A JSON library that leverages Serde for converting between JSON data and Rust data structures.
- **Usage**: Used for parsing JSON data from requests and serializing Rust data structures into JSON responses.
