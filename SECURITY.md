# Security and privacy guidance

This is a development backend, not an audited service. No supported-release programme or response deadline is established here.

- Login distinguishes absent accounts from wrong passwords. No application rate limiter, lockout, email verification or password-strength policy is supplied.
- JWT version/user/database failures are not handled uniformly throughout the filter. Test malformed/missing claims rather than assuming every invalid token returns a clean envelope.
- Logout-all affects subsequent version checks, not already-running requests or refresh-token rotation.
- Uploads have no supplied content validation, explicit provider timeouts or compensating unpin operation. Do not upload confidential content or promise remote erasure after database deletion.
- Author/comment DTOs expose emails to authenticated readers. Review necessity.
- The catch-all handler returns exception messages. Review redaction before publication.
- Compose defaults are examples, not production secrets or verified configuration.

These are static observations, not executed exploits or a complete audit. No fixes are included.

Use a verified private maintainer channel if available, or request one without publishing sensitive details. No dedicated reporting address or enabled private-reporting feature was confirmed. Redact keys, passwords, tokens and personal content.
