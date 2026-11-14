# FlyScanner target AWS architecture

## Current stage

No cloud infrastructure is provisioned by this repository. The first milestone is a reliable local backend and repeatable tests. AWS deployment follows after supplier access, traffic assumptions and a monthly budget guardrail are clear.

## Initial pilot topology

```text
Browser
  |
CloudFront + WAF (edge delivery and protection)
  |---------------- S3: React static assets
  |
HTTPS entry point
  |
One Spring Boot backend deployment
  |         |                 |
  |         |                 +-- Approved supplier APIs
  |         |
PostgreSQL (private network)
  |
Background processing (only when required)
```

The backend starts as a modular monolith with account, search, affiliate, alerts and notification modules. It is stateless and can later run as multiple identical instances.

## Candidate AWS components

| Need | Candidate | Initial treatment |
|---|---|---|
| Static frontend | S3 + CloudFront | Candidate for public UI delivery |
| Edge protection | AWS WAF | Add before public production launch; review rule cost |
| HTTPS entry | Application Load Balancer | Evaluate fixed and usage cost against pilot alternatives |
| Java runtime | ECS Fargate | Candidate for long-running backend |
| Database | RDS PostgreSQL | Small single-AZ dev; production topology based on availability objective |
| Identity | Amazon Cognito | Integrate when account APIs replace frontend demo storage |
| Background jobs | SQS | Add when asynchronous work is required |
| Secrets | Secrets Manager or SSM Parameter Store | Never commit credentials |
| Logs and metrics | CloudWatch | Set retention and control metric cardinality |

These are candidates, not a claim that every component is required on day one.

## Network rules

- Public subnets only for public entry points that need them.
- Application tasks and database in private subnets for production.
- Database has no public endpoint.
- Security groups allow only required paths.
- Supplier API egress is explicit and monitored.
- Estimate NAT Gateway cost before using it; compare approved alternatives without exposing the database.

## Region selection

Choose the first region based on expected customer location, supplier API latency, data residency, service availability and current pricing. Do not hard-code a region into reusable modules. Add another region only when recovery objectives or demand require it.

## Production evolution

1. One backend deployment and one PostgreSQL primary.
2. Increase backend task count using measured CPU, memory and request metrics.
3. Extract flight search when its independent scaling or release needs justify it.
4. Add shared cache and asynchronous workers when measurements or product features require them.
5. Add database read scaling, partitioning or sharding only after measurements.
6. Consider multi-region deployment only with explicit recovery objectives and tested data replication.
