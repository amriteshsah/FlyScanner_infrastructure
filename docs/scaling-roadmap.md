# FlyScanner scaling roadmap

The long-term ambition is to support millions of users. This is a direction for architecture, not a reason to provision large infrastructure at the start.

| Stage | Application shape | Data and infrastructure |
|---|---|---|
| Local foundation | One Spring Boot modular monolith | Local PostgreSQL, mock supplier, no AWS required |
| Development | One backend deployment | Small managed database if needed; basic logs and alarms |
| Public pilot | One or a few stateless backend instances | Managed PostgreSQL, backups, TLS, edge protection, measured autoscaling |
| Growth | Extract flight search if its load profile differs | Shared cache if beneficial, SQS workers, database read scaling |
| Large scale | Independently deployable domain services | Partition only with evidence, regional resilience, capacity testing |
| Global scale | Region-aware services and routing | Replication/failover strategy, explicit RTO/RPO and tested recovery |

## Extraction triggers

Extract a module only when there is a concrete reason: a different scaling profile, independent release cadence, unacceptable deployment blast radius, or separate operational ownership.

## First extraction candidate

Flight Search is the likely first extraction because supplier calls are latency-sensitive, rate-limited and network-heavy. Keep its interfaces and data ownership clean from the beginning.

## Avoid premature infrastructure

Do not add Kubernetes, Kafka, service mesh, distributed cache, sharding or active-active multi-region deployment solely to meet a headline user count. Each addition must solve a measured bottleneck or an explicit availability requirement.
