# Security Policy

Project Final Audit is intended for authorized defensive review and release readiness checks.

## Allowed Use

- Auditing projects you own or are authorized to test.
- Defensive vulnerability analysis.
- API documentation and API test validation.
- Security checklist improvements.

## Disallowed Use

Do not use this workflow for:

- unauthorized testing,
- destructive exploitation,
- denial-of-service or high-volume attack simulation without authorization,
- mass targeting,
- credential theft,
- supply-chain compromise,
- detection evasion for malicious purposes.

## Production Testing

Production API testing requires explicit confirmation and should default to non-destructive checks.

## Secrets

Do not include full secrets, tokens, private keys, production credentials, or sensitive customer data in reports, examples, issues, or pull requests.

## Reporting Security Issues in This Repository

If you find a security problem in the workflow itself, report it privately to the maintainers if a private channel is available. If no private channel exists, open a minimal public issue without exploit details or secrets.
