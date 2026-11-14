# FlyScanner cloud cost guardrails

## Goal

Keep the first pilot inexpensive without creating security shortcuts that would be unsafe to carry into production.

## Before provisioning

- Create an AWS Budget with agreed monthly thresholds and email alerts.
- Enable billing alerts and cost allocation tags.
- Identify an environment owner.
- Separate configuration and secrets by environment.
- Review a Terraform plan before every apply.
- Confirm current free-tier or promotional-credit eligibility rather than assuming it.

## Cost-sensitive defaults to evaluate

| Resource | Development / pilot guardrail |
|---|---|
| Application | One small task/instance initially; scale after load testing |
| Database | Small single-AZ PostgreSQL for development; test backups and recovery |
| Cache | No managed Redis until there is a measured use case |
| Messaging | No broker until asynchronous features need one |
| NAT Gateway | Do not add by default; estimate traffic and compare secure alternatives |
| Load balancer | Include fixed and usage cost in estimates |
| WAF | Start with a reviewed ruleset; monitor request and rule costs |
| Logs | Set retention; avoid full supplier payloads and personal data |
| Multi-AZ | Use in production when the availability target requires it |
| Multi-region | Defer until business continuity and demand justify the expense |

These are architecture guardrails, not a price quote. AWS rates vary by region, usage, data transfer, storage and current pricing.

## Security minimums

- Never make the database publicly accessible.
- Never commit credentials, access keys, private keys or Terraform state.
- Use managed secrets storage and least-privilege IAM roles.
- Restrict inbound rules to required sources and ports.
- Encrypt data in transit and at rest.
- Use synthetic data in development wherever possible.

## Scale from evidence

Use load tests and production telemetry to decide when to increase app capacity, add a shared cache, scale the database, extract services, or introduce queues. Track monthly active users, peak concurrent searches, searches per second, supplier quotas, p95/p99 latency and database connection usage. Registered-user count alone is not a capacity metric.
