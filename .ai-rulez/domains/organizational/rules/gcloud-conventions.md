______________________________________________________________________

## priority: medium

# GCloud Conventions

Conventions for the kreuzberg.dev GCP project infrastructure.

## Project Structure

- Primary project: `kreuzberg-dev` on Google Cloud Platform
- Environments: dev, staging, production with separate service accounts
- Region preference: `europe-west1` (Belgium) for primary workloads

## Service Usage

- **Cloud Run**: Primary deployment target for containerized services
- **Cloud Storage**: Artifact storage, document processing buckets
- **Cloud SQL / AlloyDB**: Managed PostgreSQL instances
- **Pub/Sub**: Event-driven messaging (complement to NATS)
- **Artifact Registry**: Container image and package storage
- **Secret Manager**: Secrets and credentials management

## CLI Standards

- Use `gcloud` CLI for all infrastructure operations
- Configure with `gcloud config set project kreuzberg-dev`
- Use `gcloud auth application-default login` for local development
- CI uses workload identity federation (no service account keys)

## Naming Conventions

- Resources: `kreuzberg-{service}-{environment}` (e.g., `kreuzberg-api-prod`)
- Buckets: `kreuzberg-dev-{purpose}-{environment}`
- Service accounts: `{service}@kreuzberg-dev.iam.gserviceaccount.com`

## IAM and Security

- Principle of least privilege for all service accounts
- No owner-level permissions for service accounts
- Use Workload Identity for GKE and Cloud Run

## Anti-Patterns

- Using service account keys instead of workload identity
- Creating resources without environment suffix
- Not using Secret Manager for credentials
- Manual console operations instead of gcloud CLI/Terraform
