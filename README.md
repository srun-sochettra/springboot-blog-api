# Spring Boot Blog API

**A PostgreSQL-backed blog backend with JWT authentication, owner-checked posts and comments, likes and multipart uploads.**

Built with Java 21, Spring Boot 3.4.1 and MyBatis. This project explores authenticated content workflows, database-backed token invalidation and Pinata file uploads. It is a development project, not an audited production service or a verified live deployment.

## Features

- Register and log in with BCrypt password hashing and JWT issuance.
- Logout-all increments a database token version checked on later requests.
- List posts, retrieve a post and view the current user's posts.
- Check ownership before editing/deleting posts and comments.
- Like/unlike posts and return counts/current-user state.
- Upload files to Pinata and store gateway URLs in PostgreSQL.
- Use request/response DTOs and a relational schema with foreign keys.

All post/comment routes require authentication under SecurityConfig, including reads. Service branches accommodating anonymous users do not make those routes public. No roles, pagination, moderation or refresh-token workflow is supplied.

## Stack and setup

The POM declares Java 21, Spring Boot 3.4.1, MyBatis 3.0.3, Springdoc 2.8.5 and JJWT 0.12.6. The wrapper selects Maven 3.9.14. Docker uses Java 21 and PostgreSQL 15 Alpine. These are source declarations, not freshly tested dependencies.

Read [local setup](README.onboarding.md) before starting. Suggested Windows command after configuration:

```powershell
.\mvnw.cmd spring-boot:run
```

POSIX equivalent: `sh mvnw spring-boot:run`. Initial use may download Maven and dependencies. Existing docs list Swagger paths `/swagger-ui/index.html` and `/v3/api-docs`; no startup or endpoint availability was checked here.

## Endpoints

| Method | Path | Behaviour |
| --- | --- | --- |
| POST | `/api/v1/auths/register` | Public registration; HTTP 201 with user DTO |
| POST | `/api/v1/auths/login` | Public login; token response |
| POST | `/api/v1/auths/logout-all` | Authenticated token-version increment |
| GET | `/api/v1/posts` | Authenticated list |
| GET | `/api/v1/posts/{id}` | Authenticated read |
| GET | `/api/v1/posts/my-posts` | Current user's posts |
| POST | `/api/v1/posts/add-post` | Multipart title, description and required files part |
| PUT | `/api/v1/posts/{id}` | Owner-only multipart update; optional files |
| DELETE | `/api/v1/posts/{id}` | Owner-only deletion |
| POST / DELETE | `/api/v1/posts/{id}/like` | Like/unlike |
| POST | `/api/v1/comments/add-comment` | JSON content and postId |
| PUT / DELETE | `/api/v1/comments/{id}` | Owner-only edit/delete |

Post creation returns HTTP 200. Updates preserve omitted title/description and append non-empty uploaded files rather than replacing images. Database deletion cascades related rows, but no remote unpin/delete operation is supplied.

Registration/login validate non-blank fields and email syntax; comments require non-blank content. PostRequest has no validation annotations, and post controllers do not apply @Valid. No application-level file-type checks or explicit multipart limits are configured; framework defaults are not a reviewed upload policy.

## Architecture and boundaries

Controllers -> services -> MyBatis mappers -> PostgreSQL. Post operations call Pinata inside database transactions. A rollback cannot undo a completed external upload; no compensating cleanup is supplied.

UserResponse excludes passwords but includes email. Nested mapper queries and per-post like checks need performance review before claiming scale. Response envelopes contain message, payload, numeric status and dateTime, but authentication/filter errors do not consistently use that envelope. Timestamps have no explicit timezone.

## Verification

Suggested Windows checks:

```powershell
.\mvnw.cmd test
.\mvnw.cmd package
```

POSIX equivalents use `sh mvnw test` and `sh mvnw package`. Mocked service tests and a context smoke test are supplied, not fresh passing results or proof of SQL/JWT-filter/provider integration. test_script.sh only prints a message. Docker packaging skips test execution.

No installation, compilation, tests, Docker startup, database operation, HTTP request or provider upload was performed during this documentation refresh.

See [CONTRIBUTING.md](CONTRIBUTING.md), [SECURITY.md](SECURITY.md), [portfolio summary](README.portfolio.md) and [refresh notes](docs/repository-refresh.md).

## Licence

No root project licence file is supplied. Wrapper notices do not establish an application licence. This refresh assigns none.
