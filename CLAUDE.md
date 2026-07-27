# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Spring Boot 3.5.13 / Java 25 Maven project (`com.vardiprasanna:cashback-rewards`), base package `com.vardiprasanna.cashbackrewards`. Still an early skeleton — only the main application class exists; no domain/business logic has been written yet.

Dependencies configured: `spring-boot-starter-web`, `spring-boot-starter-data-jpa`, `spring-boot-starter-validation`, H2 (runtime), `spring-boot-starter-test`.

## Commands

There is no Maven wrapper in this repo — use the system `mvn` (not `./mvnw`).

- Build: `mvn clean install`
- Run the app: `mvn spring-boot:run`
- Run all tests: `mvn test`
- Run a single test class: `mvn test -Dtest=ClassName`
- Run a single test method: `mvn test -Dtest=ClassName#methodName`

## Architecture

`CashbackRewardsApplication` (`src/main/java/.../CashbackRewardsApplication.java`) is the only entry point so far — standard `@SpringBootApplication` bootstrap, no custom configuration classes yet.

Persistence: H2 in-memory database (`jdbc:h2:mem:cashbackrewards`), configured in `src/main/resources/application.properties`. `spring.jpa.hibernate.ddl-auto=update` — schema is derived from JPA entities, not migrations (no Flyway/Liquibase dependency). H2 console is enabled at `/h2-console`. `spring.jpa.show-sql=true` — SQL statements are logged, useful when debugging repository/query behavior.

The package layout under `com.vardiprasanna.cashbackrewards` follows a standard layered structure — `controller`, `service` (with `service/impl`), `repository`, `model`, `dto`, `mapper`, `exception`, `config`, `util` — mirrored under `src/test/java` for `controller`/`service`/`repository` tests. Most of these packages are currently empty placeholders for the intended layering; as code is added, controllers should stay thin and delegate to services, with repositories as the only JPA/data-access layer.

## Specs

Feature/requirement specs live in `specs/` (one markdown file per feature). Check there for the intended behavior before implementing or modifying a feature.
