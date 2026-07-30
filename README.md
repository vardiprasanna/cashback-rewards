# Cashback Rewards

A Spring Boot service that rewards customers with cashback on their purchases.

## Tech Stack

- Java 25
- Spring Boot 3.5.13 (Web, Data JPA, Validation)
- H2 (in-memory database)
- Maven

## Prerequisites

- JDK 25
- Maven (no wrapper is checked in — use your system `mvn`)

## Getting Started

```bash
# Build
mvn clean install

# Run the app
mvn spring-boot:run

# Run all tests
mvn test

# Run a single test class
mvn test -Dtest=ClassName

# Run a single test method
mvn test -Dtest=ClassName#methodName
```

Once running, the app is available at `http://localhost:8080`, and the H2 console at `http://localhost:8080/h2-console` (JDBC URL: `jdbc:h2:mem:cashbackrewards`, user `sa`, no password).

## Project Structure

Standard layered architecture under `com.vardiprasanna.cashbackrewards`:

```
controller/   HTTP endpoints
service/      Business logic (impl/ holds implementations)
repository/   Spring Data JPA repositories
model/        JPA entities
dto/          Request/response data transfer objects
mapper/       Entity <-> DTO mapping
exception/    Custom exceptions and handlers
config/       Spring configuration
util/         Shared helpers
```

## Specs

Feature and business rule specs live in [`specs/`](specs/), written using the Example Mapping approach (rules, examples, counter-examples, open questions) — see [`specs/cashback-earning.md`](specs/cashback-earning.md) for the cashback earning rules.

## Guidance for Contributors

See [`CLAUDE.md`](CLAUDE.md) for build commands and architecture notes intended for AI coding assistants (and useful background for human contributors too).
