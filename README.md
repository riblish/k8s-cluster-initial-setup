# Initial Setup for a K8s cluster

This repo contains all the configuration files required to set up a new Kubernetes cluster with GitOps.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Repository Structure](#repository-structure)
- [Github Actions](#github-actions)
- [Running Locally](#running-locally)
  - [ArgoCD Installation](#argocd-installation)
    - [Change ArgoCD admin password](#change-argocd-admin-password)

## Prerequisites

Before using this repository, ensure you have the following:

- A newly provisioned Kubernetes cluster (AKS, EKS, GKE, or on-prem).
- `kubectl` installed and configured.
- `helm` installed on your local machine.

## Repository Structure

```plaintext
.
├── .github/
│   └── workflows/
│       └── deploy-argocd.yml  # GitHub Actions workflow for deploying ArgoCD
├── helm/
│   └── argocd/
│       └── values.yaml        # Helm values configuration for ArgoCD
└── README.md                  # This README file
```

## Github Actions

Update `KUBECONFIG` in repository secrets with cluster config before running the workflows. Below are the available options:

1. Deploy ArgoCD

## Running Locally

### ArgoCD Installation

To install ArgoCD with Helm, use the below commands.

```bash
helm repo add argo https://argoproj.github.io/argo-helm
```

```bash
helm repo update
```

```bash
helm upgrade --install argocd argo/argo-cd \
          --namespace argocd \
          --create-namespace \
          -f helm/argocd/values.yaml
```

You can customize the ArgoCD deployment by modifying the `values.yaml` file:

#### Change ArgoCD admin password

To change the ArgoCD admin password, use bcrypt to hash a new password like:

```bash
htpasswd -nbBC 10 "" 'your-new-password' | tr -d ':\n' | sed 's/$2y/$2a/'
```

Then replace the value of `adminPassword` field in `values.yaml` file (path: `./helm/argocd/`) with the new hash.
