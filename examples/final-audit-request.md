# Example Final Audit Request

```yaml
scope: ./backend
base_url: http://localhost:3000
auth_notes: JWT test token is available in .env.test; test user is audit@example.com
environment: local
write_outputs: false
language: zh-CN
allowed_actions:
  - read-only
  - run-tests
  - api-smoke-test
destructive_testing_allowed: false
```

Natural language version:

```text
Run Project Final Audit for ./backend. The local API is http://localhost:3000. Use only non-destructive tests. JWT test token notes are in .env.test. Output in Chinese and do not write files unless I approve.
```
