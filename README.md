# FlyScanner Infrastructure

Infrastructure architecture and infrastructure-as-code foundation for FlyScanner.

This repository is documentation-first. It records the low-cost deployment path, security boundaries and scale-out strategy before AWS resources are provisioned.

**No AWS resources are created by this repository yet.** Terraform modules and deployment workflows will be added after the AWS account, region, budget alerts and environment requirements are agreed.

## Current decisions

- Affiliate redirect model; FlyScanner does not process payments or issue tickets in v1.
- Backend starts as one stateless Spring Boot modular monolith.
- Backend: https://github.com/amriteshsah/FlyScanner_backend
- Frontend: https://github.com/amriteshsah/FlyScanner_ui
- Long-term ambition: support substantial growth without paying for that capacity before it is needed.

## Documents

- [Target AWS architecture](docs/target-architecture.md)
- [Cost guardrails](docs/cost-guardrails.md)
- [Scaling roadmap](docs/scaling-roadmap.md)
- [Terraform implementation notes](terraform/README.md)

## Principles

1. Local development must work without AWS.
2. Use managed services where their operational savings justify their cost.
3. Keep production credentials out of Git and frontend builds.
4. Start with the smallest environment that can validate the product safely.
5. Add high availability and multi-region cost only when availability objectives and traffic justify it.
6. Tag every provisioned resource with project, environment, owner and purpose.
7. Require reviewed Terraform plans before applying infrastructure.
