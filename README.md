# CargoTrack Helm

Helm charts for the CargoTrack Logistics Platform Kubernetes deployment.

## Repository Structure

```
cargotrack-helm/
└── cargotrack/             # Main Helm chart
    ├── Chart.yaml
    ├── values.yaml         # Default values (all services)
    ├── values-dev.yaml     # Dev environment overrides (image tags updated by CI)
    ├── values-prod.yaml    # Production environment overrides
    └── templates/
        ├── core-service.yaml      # Deployment + Service + ServiceAccount
        ├── ai-service.yaml        # Deployment + Service + ServiceAccount
        ├── document-service.yaml  # Deployment + Service + ServiceAccount
        ├── frontend.yaml          # Deployment + Service + ServiceAccount
        ├── configmap.yaml         # cargotrack-aws-config ConfigMap
        ├── ingress.yaml           # ALB Ingress
        ├── hpa.yaml               # HorizontalPodAutoscaler (all services)
        └── namespace.yaml         # cargotrack namespace
```

## How Image Tags Are Updated

The `build.yml` pipeline in `cargotrack-app` automatically updates image tags:

```
cargotrack-app CI push → build + push to ECR
  → deploy.yml commits new SHA to values-dev.yaml in this repo
  → ArgoCD detects git change → auto-sync → rolling deploy
```

The `values-dev.yaml` image tags are updated by the deploy pipeline. Do not edit them manually.
