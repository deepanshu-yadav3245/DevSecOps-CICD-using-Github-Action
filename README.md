# CI/CD Pipeline with GitHub Actions | Full DevSecOps Implementation

## Overview

This project demonstrates a complete CI/CD pipeline using GitHub Actions with integrated DevSecOps practices. It automates everything from code validation to deployment on Kubernetes.

The pipeline ensures:

- Code quality
- Security checks
- Containerization
- Automated deployment

## Tech Stack

- CI/CD: GitHub Actions
- Code Quality: SonarQube
- Secrets Detection: Gitleaks
- Security Scanning: Trivy
- Containerization: Docker
- Container Registry: Docker Hub
- Orchestration: Kubernetes (EKS)
- Cloud: AWS

## Pipeline Workflow

The pipeline is triggered on:

- Push to `main`
- Pull request to `main`

## Stages Breakdown

### 1. Code Compilation (Syntax Check)

- Validates both frontend and backend code.
- Uses Node.js runtime to detect syntax errors early.

### 2. Secrets Detection (Gitleaks)

Scans repository for:

- API keys
- Tokens
- Hardcoded credentials

Pipeline fails if secrets are detected.

### 3. File System Vulnerability Scan (Trivy)

Scans project dependencies and detects:

- OS vulnerabilities
- Library vulnerabilities

Focuses on `HIGH` and `CRITICAL` issues.

### 4. Code Quality Analysis (SonarQube)

#### Frontend Scan

- Analyzes code quality.
- Ignores build artifacts and `node_modules`.

#### Backend Scan

- Runs separate analysis for backend.
- Ensures maintainability and security standards.

### 5. Docker Image Build and Push

#### Backend Image

- Built first.
- Pushed to Docker Hub.

#### Frontend Image

- Built after backend.
- Pushed to Docker Hub.

Images:

- `deepanshuyadav23/backend:latest`
- `deepanshuyadav23/frontend:latest`

### 6. Container Image Scan (Trivy)

- Scans Docker images after build.
- Ensures production-ready images.
- Reports vulnerabilities but does not fail the pipeline.

### 7. Deployment to Kubernetes (EKS)

- Configures AWS credentials.
- Uses kubeconfig stored in GitHub Secrets.
- Deploys using Kubernetes manifests from `k8s-prod`.
- Creates namespace `prod`.
- Applies all resources automatically.

## Kubernetes Manifests

```text
k8s-prod/
├── sc.yaml
├── mysql.yaml
├── backend.yaml
├── frontend.yaml
├── ci.yaml
```

## Secrets Used

Stored securely in GitHub Secrets:

- `SONAR_TOKEN`
- `SONAR_HOST_URL`
- `DOCKERHUB_TOKEN`
- `DOCKERHUB_USERNAME` (repository variable)
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `EKS_KUBECONFIG`

## Project Structure

```text
.
├── client/              # Frontend
├── api/                 # Backend
├── k8s-prod/            # Kubernetes manifests
├── .github/workflows/   # CI/CD pipeline
```

## Key Highlights

- End-to-end automated pipeline.
- Integrated DevSecOps (security at every stage).
- Separate frontend and backend workflows.
- Docker multi-stage build support.
- Kubernetes deployment on AWS EKS.
- Real-world production-ready flow.

## Important Notes

- Ensure Docker Hub repositories exist before pushing images.
- SonarQube server must be accessible from GitHub Actions runners.
- AWS IAM user should have EKS access permissions.
- Kubernetes manifests should be production-ready and validated.

## Future Improvements

- Add Helm charts for deployment.
- Implement ArgoCD (GitOps).
- Add Slack or email notifications.
- Enable auto rollback strategy.
- Add unit and integration test stages.

## Conclusion

This project showcases a complete, real-world CI/CD pipeline with security, automation, and scalable deployment. It reflects how modern DevOps workflows are implemented in production.
