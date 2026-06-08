# Final Audit Report Example

```text
Release Readiness: Ready with risks
Overall Risk: Medium
Audit Scope: ./backend

Audit Progress
[##########] 100%

Stage                         Status      Findings
Project discovery             PASS        Node.js API, 24 endpoints found
Dependency/config audit        WARN        2 medium dependency issues
Source security review        WARN        1 medium auth boundary concern
API documentation             WARN        4 undocumented endpoints
API testing                   BLOCKED     Missing test auth token
Final report                  PASS        Report generated
```

## Security Findings

```text
SEC-001 | Medium | Open
Title: CORS allows broad localhost origins
Location: src/config/cors.ts:12
Evidence: origin regex allows all localhost ports
Impact: local malicious pages may interact with authenticated dev API
Recommendation: restrict dev origins and document production CORS separately
Retest: start API and verify disallowed origin is rejected
```

## API Test Issues

```text
No API tests were executed because auth token was missing.
```

## Documentation Nonconformance

```text
DOC-001 | Open
Endpoint: POST /login
Problem: response schema missing refresh_token field
```

## Final Recommendation

Ready with risks. Fix documentation gaps and rerun API tests with a valid test token before release.
