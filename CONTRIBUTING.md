# Contributing

Keep changes focused on existing controller/service/MyBatis boundaries. Use synthetic accounts, an isolated database and a controlled provider account.

Run wrapper tests/package and report actual results. Mocked repositories/uploads do not verify SQL, JWT filters or provider behaviour. Test ownership denial, missing records, invalid/expired/version-mismatched tokens, duplicate registration and repeated likes where relevant.

For uploads, test partial provider success, database rollback and cleanup. For schema changes, verify existing data and recovery rather than relying on CREATE TABLE IF NOT EXISTS.

Describe the problem, focused change, checks, data/provider impact and rollback. Keep credentials, tokens and private uploads out of reports. See [SECURITY.md](SECURITY.md).
