# API Test Matrix Example

```text
ID       Method   Path          Case Type        Expected                  Actual    Status
TC-001   GET      /health       happy path       200 + service status      200       PASS
TC-002   POST     /login        happy path       200 + token pair          -         BLOCKED
TC-003   POST     /login        missing field    400 validation error      -         BLOCKED
TC-004   GET      /users/me     missing auth     401 unauthorized          401       PASS
TC-005   GET      /users/{id}   no permission    403 forbidden             -         PENDING
```

Blocked cases must include a reason:

```text
BLK-001 | BLOCKED
Missing: test credentials
Needed: provide test username/password or token
```
