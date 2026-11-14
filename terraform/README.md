# Terraform implementation plan

Terraform resources are intentionally not created in this initial commit.

Before writing deployable modules, agree:
- AWS account and environment separation.
- First deployment region.
- Monthly budget and alert recipients.
- Backend runtime and networking model.
- Database backup, retention and recovery objectives.
- Secrets and CI/CD identity strategy.

## Planned layout

```text
terraform/
  modules/
    networking/
    application/
    database/
    observability/
  environments/
    dev/
    pilot/
```

## Required practices

- Remote state in a protected, encrypted, versioned S3 bucket with restricted access.
- Use the locking mechanism supported by the selected Terraform version and backend.
- No secrets in tfvars, state outputs or committed files.
- Plan in CI; apply only through an authorized, reviewed workflow.
- Tag resources with Project=FlyScanner, Environment, Owner and ManagedBy=Terraform.
- Pin provider and module versions after compatibility testing.
- Keep modules small and environment inputs explicit.

Do not run terraform apply until the plan and cost estimate have been reviewed.
