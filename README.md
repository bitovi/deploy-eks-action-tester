# deploy-eks-action-tester

A repo to test [the Bitovi deploy-eks-helm action](https://github.com/bitovi/github-actions-deploy-eks-helm).

## Cluster Access

You need an existing Kubernetes cluster to target with this Action. 

Don't have one yet? Spin one up in AWS with [our Deploy EKS GitHub Action!](https://github.com/bitovi/github-actions-deploy-eks)

## Testing the Helm Action

This demo repo will deploy an NGINX Ingress Controller to the target Kubernetes cluster, using Helm.

## The Workflow

In this repo: `.github/workflows/deploy-helm.yml`

```yaml
name: Helm Deploy
on:
  push:
    branches: [ main ]

jobs:
  Helm-Deploy:
    runs-on: ubuntu-latest
    environment:
      name: ${{ github.ref_name }}  # the branch name
    steps:
      - id: Deploy-Helm
        uses: bitovi/github-actions-deploy-eks-helm@v1.2.9
        with:
            aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID_SANDBOX }}
            aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY_SANDBOX }}
            aws-region: us-east-1
            cluster-name: eks-action-test
            chart-repository: oci://ghcr.io/
            chart-path: nginxinc/charts/nginx-ingress
            namespace: ingress
            name: demo
            version: 1.2.2
```