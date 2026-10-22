# Local development

The current POM targets Java 21 and Spring Boot 3.4.1, replacing older Java 17 / Boot 3.2.5 descriptions. This is a proposed setup based on supplied files, not an executed installation.

## Configuration

Set private process environment values before Maven execution:

- DB_PASSWORD: PostgreSQL password.
- JWT_SECRET: Base64-encoded signing key decoded by JwtService.
- PINATA_API_KEY and PINATA_SECRET_KEY: used by PinataService.
- PINATA_JWT: referenced by properties, but not read by the upload service.

JWT_SECRET must decode to an adequate HMAC key. An arbitrary long phrase is not equivalent to valid Base64. No validated sample key is supplied. Configure the unused PINATA_JWT reference too, or deliberately revise it after checking startup behaviour. No .env loader is supplied for direct Maven execution.

The properties target a local PostgreSQL database named api_blog with user postgres. Use a disposable database. Existing setup instructs creating the database separately, then applying:

```powershell
psql -U postgres -d api_blog -f src/main/resources/schema.sql
```

Review the target first. CREATE TABLE IF NOT EXISTS is not a migration for incompatible existing tables. No versioned migration or recovery workflow is supplied.

With configuration available, start from the repository root:

```powershell
.\mvnw.cmd spring-boot:run
```

POSIX equivalent: `sh mvnw spring-boot:run`. See the main README for routes. Use synthetic accounts and non-private uploads.

## Compose caveats

Compose defines app/database services, persistent db_data storage and a schema initialisation mount. Review existing-volume behaviour rather than treating initialisation as a migration.

Its fallback JWT secret is not valid Base64 for the supplied decoder, Pinata values are placeholders, and PINATA_JWT is not passed to the app despite the properties reference. Docker packaging skips tests. Supply deliberate private configuration and verify startup/auth/uploads separately before claiming the container workflow works.

This overlay changes no application, Compose or environment configuration.
