---
priority: medium
---

- GCP project: kreuzberg-dev, region: europe-west1
- Services: Cloud Run (compute), Cloud Storage (artifacts), Cloud SQL/AlloyDB (Postgres), Pub/Sub (messaging), Artifact Registry (images), Secret Manager (credentials)
- Naming: kreuzberg-{service}-{environment}, buckets: kreuzberg-dev-{purpose}-{environment}
- Auth: workload identity federation — never service account keys
- IAM: least privilege, no owner-level for service accounts
- Use gcloud CLI for all operations — no manual console changes
