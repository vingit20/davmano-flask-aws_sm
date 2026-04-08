# 🧱 Repos

   App1 Repo: [**davmano/flask-app_sm**](https://github.com/davmano-flask-aws_sm)
   
   GitOps Repo: [**davmano/k8s-gitops-repo**](https://github.com/davmano/k8s-gitops-repo)
  


## 🔁 CI/CD Pipeline Breakdown
### Step	Action
- ✅ checkout	Pulls source code (app repo)
- 🐳 docker build	Builds image with short SHA + latest tag
- 🔍 Trivy scan	Scans image for critical/high vulnerabilities
- ☁️ docker push	Pushes both tags to Docker Hub
- 📦 checkout GitOps repo	Pulls deployment YAML from davmano/k8s-gitops-repo
- ✏️ sed update	Replaces image tag in deployment.yaml to new SHORT_SHA
- 🔄 git commit + push	Pushes change to GitOps repo
- 🔁 ArgoCD watches the GitOps repo and syncs the change into your Kubernetes cluster	

## 🧱 Here's how the full GitOps workflow looks for your case:
```
                   ┌────────────────────────┐
                   │     App Repo (CI)      │
                   │  davmano/flask-app_sm  │
                   └────────────────────────┘
                             │
           Push to main      ▼
                        GitHub Actions CI
                             │
        ┌───────────────────────────────┐
        │                               │
        ▼                               ▼
 Build + Tag Docker Image        Trivy Security Scan
        │                               │
        ▼                               ▼
 Push image:                            ✅ Passed
 davmano/flask-app_sm:<sha>, latest     |
        │                               ▼
        └────── Update GitOps Repo ─────►
                sed image tag in deployment.yaml
                             │
                             ▼
                Commit & Push to GitOps Repo
                   davmano/k8s-gitops-repo
                             ▼
                       ⏱ ArgoCD Sync
                             ▼
                    🧩 Kubernetes Cluster
               Runs flask-app_sm:<sha> image
```


## CI/CD Pipeline Badge

[![CI/CD](https://github.com/davmano/davmano-flask-aws_sm/actions/workflows/cicd.yml/badge.svg)](https://github.com/davmano/davmano-flask-aws_sm/actions/workflows/cicd.yml)
