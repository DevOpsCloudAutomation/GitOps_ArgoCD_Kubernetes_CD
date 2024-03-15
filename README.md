# End to End CD Pipeline Project GitOps ArgoCD

## This Project can be used to Build an End to End CD Pipeline GitOps ArgoCD.

## CD Pipeline Stages

- Checkout Code from GitHub.
- Update Docker Image Tag in Kubernetes Manifest.
- Push Updated Kubernetes Deployment File to GitHub.
- Send CICD Pipeline Execution Status to Slack.

### Tools and Technologies used are Java, Git, GitHub, Maven, SonarQube, Sonatype Nexus, Jenkins, Docker, AWS ECR Registry, Kubernetes and Amazon Web Services.

![CICD](https://github.com/DevOpsCloudAutomation/TestRepository/assets/123757746/e0380bd0-f586-4e1e-b341-11b78fe9f8d1)
  
# Project Execution
## Git Checkout
```bash
  git branch: 'main', url: 'https://github.com/DevOpsCloudAutomation/Kubernetes_GitOps_ArgoCD.git'
```

## Update Docker Image Tag in Kubernetes Manifest
```bash
  sed -i 's/${application_Name}.*/${application_Name}:${buildNumber}/g' Deployment.yaml
```

## Push Updated Kubernetes Deployment File to GitHub
```bash
  git config --global user.name "DevOpsCloudAutomation"
  git config --global user.email "Pavankumarkj347@gmail.com"
  git add Deployment.yaml
  git commit -m "Updated Build Number in Deployment Manifest File"
  git push https://github.com/DevOpsCloudAutomation/Kubernetes_GitOps_ArgoCD main
```