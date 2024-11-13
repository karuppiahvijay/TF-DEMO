Tutorial: CI/CD pipeline using Helm as a base, Kustomize for environment-specific configurations, and deploying with GitHub Actions

Creating a CI/CD pipeline using Helm as a base, Kustomize for environment-specific configurations, and deploying with GitHub Actions is a great way to manage and deploy applications consistently across different environments. Here's a step-by-step tutorial to help you get started: 

demo/
├── backend/                           # Backend application code
│   ├── Dockerfile                     # Dockerfile for backend
│   └── ...                            # Other backend source files
├── frontend/                          # Frontend application code
│   ├── Dockerfile                     # Dockerfile for frontend
│   └── ...                            # Other frontend source files
├── myapp/                             # Helm chart for the application
│   ├── Chart.yaml                     # Helm chart metadata
│   ├── values.yaml                    # Default values file
│   └── .helmignore                    # Ignore files for Helm packaging
├── kustomize/                         # Kustomize configurations
│   ├── base/                          # Base Kustomize configurations
│   │   ├── backend-deployment.yaml    # Base backend deployment
│   │   ├── frontend-deployment.yaml   # Base frontend deployment
│   │   ├── postgres-deployment.yaml   # Base Postgres deployment
│   │   ├── backend-service.yaml       # Base backend service
│   │   ├── frontend-service.yaml      # Base frontend service
│   │   ├── postgres-service.yaml      # Base Postgres service
│   │   └── kustomization.yaml         # Kustomize base config file
│   └── overlays/                      # Overlays for different environments
│       ├── dev/                       # Dev environment-specific configs
│       │   ├── kustomization.yaml     # Kustomization file for dev
│       │   ├── release-dev.yaml       # Helm release file for dev
│       │   ├── configmap.yaml         # ConfigMap with dev-specific settings
│       │   └── values-dev.yaml        # Helm values for dev environment
│       └── staging/                   # Staging environment-specific configs
│           ├── kustomization.yaml     # Kustomization file for staging
│           ├── release-staging.yaml   # Helm release file for staging
│           ├── configmap.yaml         # ConfigMap with staging-specific settings
│           └── values-staging.yaml    # Helm values for staging environment
└── .github/
    └── workflows/
        ├── ci.yaml                    # CI pipeline file
        └── cd.yaml                    # CD pipeline file
Prerequisites
To follow this tutorial, you need the following tools installed:
  Minikube: To create a local Kubernetes cluster. You can install Minikube using:
  macOS (Homebrew): brew install minikube
  Linux: curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && sudo install minikube-linux-amd64 /usr/local/bin/minikube
  Windows (Chocolatey): choco install minikube
  kubectl: The command-line tool for managing Kubernetes clusters. Install using:
macOS (Homebrew): brew install kubectl
Linux: curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
Windows (Chocolatey): choco install kubernetes-cli
Helm: A package manager for Kubernetes that helps manage application deployments. Install using:
macOS (Homebrew): brew install helm
Linux/macOS: curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
Windows (Chocolatey): choco install kubernetes-helm
Docker: Minikube will use Docker as the container runtime. Make sure Docker is installed and running:
macOS/Windows: Install Docker Desktop from Docker's official website.
Linux: Install Docker using:
sudo apt-get update
sudo apt-get install -y docker.io
Kustamize:
      macOS (Homebrew):brew install kustomize
     MacPorts:sudo port install kustomize
    Linux:Snap - sudo snap install kustomize
             Binary -  curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | bash
           Windows - choco install kustomize



Deployment and Access Instructions
1. Start Up Minikube
Ensure that Minikube is installed on your local machine. Start Minikube with sufficient resources:
minikube start --cpus=4 --memory=8192mb
2. Install the Application Again
To deploy the application using Helm, run:
helm install myapp
3. Arrange the structure as above and build and push the docker images 
docker build -t username/image:tag .
docker push username/image:tag

4.Step 5: Set Up GitHub Secrets**
Go to your GitHub repository settings
Navigate to "Secrets and variables" → "Actions"
Add the following secrets:
**DOCKERHUB_USERNAME**: Your Docker Hub username
**DOCKERHUB_TOKEN**: Your Docker Hub access token

5. Upgade helm with respective environment 
Dev - helm upgrade --install myapp ./myapp -f ./kustomize/overlays/dev/values-dev.yaml -n dev
Staging - helm upgrade --install myapp ./myapp -f ./kustomize/overlays/staging/values-staging.yaml -n staging
6. Creating different environment configurations specified in the mentioned namespace
kubectl apply -f release-dev.yaml -n dev
kubectl apply -f release-staging.yaml -n staging

7. Run self-hosted system
Git- settings - actions -runners- set new self-hosted machine - follow the steps for the respective OS.
8.Run CI and CD in github actions.


















