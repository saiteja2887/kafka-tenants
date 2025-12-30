# kafka-tenants

Helm-based repository to manage per-tenant Apache Kafka resources (clusters, topics, connectors).

## Repository layout

- `helm-kafka-tenants/` — Helm chart and client overlays
  - `Chart.yaml` — Helm chart metadata
  - `clients/` — per-client configuration and overlays
    - `dev/` — example/dev clients
      - `acme/` — example client (contains `acme.yaml`, `values.yaml`, and GitHub Actions workflow)
  - `templates/` — chart templates
    - `00-namespace.yaml` — namespace manifest
    - `01-kafka-cluster.yaml` — Kafka cluster custom resource manifest
    - `02-kafka-topic.yaml` — Kafka topic custom resource manifest
    - `03-kafka-connector.yaml` — Kafka connector custom resource manifest

## Purpose

This chart is intended to provision tenant-scoped Kafka resources using Kubernetes/Helm. Each client can keep values and small overlay manifests under `clients/<env>/<client>/` to customize deployment.

## Quickstart

1. Add your cluster's Kafka operator and CRDs (not provided here).
2. Customize values for a tenant at `helm-kafka-tenants/clients/<env>/<tenant>/values.yaml`.
3. Install or upgrade the chart (example):

```bash
helm upgrade --install acme-kafka helm-kafka-tenants \
  -n acme-namespace \
  -f helm-kafka-tenants/clients/dev/acme/values.yaml
```

Adjust release name and namespace as needed. The chart templates create namespace, cluster, topic, and connector resources from the provided values.

## Client overlays and CI

- Per-client overlays live under `helm-kafka-tenants/clients/`.
- Individual clients may include GitHub Actions workflows (for example `clients/dev/acme/.github/workflows/client_deploy.yaml`) to automate validation and deployment.

## Contributing

1. Open an issue describing your change.
2. Add a small, focused change set and include relevant updates under `clients/` if adding a tenant.

## Notes

- This repository contains only the Helm chart and client overlay artifacts. You must have a running Kubernetes cluster and a Kafka operator (such as Strimzi) capable of reconciling the provided Kafka CRs.
- This README is intentionally minimal — expand with operator-specific instructions, CRD requirements, and examples as you adopt the chart.

