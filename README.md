# Enterprise Java GitOps Deployment

This repository contains the **Kubernetes manifests and GitOps configuration** for deploying the Enterprise Java microservice to Amazon EKS using Argo CD.

## 🌟 What Is This Repo For?

- **Declarative Kubernetes Manifests:**  
  All deployment, service, and monitoring YAMLs are stored here.
- **GitOps Workflow:**  
  Argo CD watches this repo and syncs any changes to the EKS cluster.
- **Single Source of Truth:**  
  All infrastructure and app changes are versioned and auditable.

## 📦 Repository Structure

gitops/

deployment.yaml # Kubernetes Deployment for the app

service.yaml # Kubernetes Service (LoadBalancer)

servicemonitor.yaml # Prometheus ServiceMonitor for app metrics

kustomization.yaml # (If using Kustomize)

argocd-app.yaml # Argo CD Application manifest

observability/

grafana-dashboard.json # Custom Grafana dashboard (importable)

docs/


## 🚀 How the GitOps Flow Works

1. **Jenkins Pipeline:**  
   Updates the `deployment.yaml` image tag in this repo after a successful build.

2. **Argo CD:**  
   Watches this repo for changes.
   - On a new commit, Argo CD automatically applies the updated manifests to EKS.
   - Ensures the cluster matches the state defined in this repo.

3. **Monitoring:**  
   - `servicemonitor.yaml` ensures Prometheus scrapes the app's `/actuator/prometheus` endpoint.
   - Custom dashboards can be imported into Grafana using `grafana-dashboard.json`.

## 🛠️ Key Files Explained

- **`deployment.yaml`**  
  Defines the deployment (replicas, container image, ports, probes).

- **`service.yaml`**  
  Exposes the app via a LoadBalancer for external access.

- **`servicemonitor.yaml`**  
  Tells Prometheus to scrape metrics from the app.

- **`argocd-app.yaml`**  
  Registers this repo/path as an Argo CD Application.

- **`grafana-dashboard.json`**  
  Ready-to-import Grafana dashboard for app and cluster metrics.

## 📈 Monitoring Setup

- Prometheus and Grafana are deployed via the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts).
- After deploying the app, import `observability/grafana-dashboard.json` into Grafana for instant dashboards.

## 📝 How to Make Changes

1. Edit the relevant YAML file (e.g., update resources, add a ServiceMonitor).
2. Commit and push your changes:

git add .
git commit -m "Update deployment config"
git push origin main

3. **Argo CD will automatically sync and apply the changes to the cluster.**

## 💡 Best Practices

- **Never apply manifests directly with `kubectl apply`!**  
Always update this repo and let Argo CD manage the deployment.
- **Keep all configuration as code** for auditability and rollback.

## 📚 Documentation

- [Project Report](../docs/Project_Report.md)
- [Sample Dashboards](observability/grafana-dashboard.json)

---

**Questions or contributions? Open an issue or pull request!**
