# **Elta Kubernetes Project 🚀**

This project demonstrates how to deploy Jenkins in a Kubernetes environment and automate the build and deployment of a .NET Core web application.

[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.22+-blue.svg)](https://kubernetes.io/)
[![Helm](https://img.shields.io/badge/Helm-v3.9+-blue.svg)](https://helm.sh/)
[![Jenkins](https://img.shields.io/badge/Jenkins-2.479.1-blue.svg)](https://www.jenkins.io/)

---

## 📚 **Table of Contents**
1. [Features](#features)
2. [Technologies Used](#technologies-used)
3. [Installation](#installation)
4. [Pipeline Workflow](#pipeline-workflow)
5. [Troubleshooting](#troubleshooting)
6. [Contributions](#contributions)
7. [License](#license)

---

## <a name="features"></a>⚡ **Features**
- Deployment of Jenkins in a Kubernetes namespace using Helm.
- Persistent storage for Jenkins configuration and data.
- Automated CI/CD pipeline to:
  - Build a .NET Core web application.
  - Deploy the application to a production namespace.
- Port-forwarding for Jenkins access.
- Configurable resource limits for Pods.

---

## <a name="technologies-used"></a>🛠️ **Technologies Used**
- **Orchestration**: Kubernetes (Minikube).
- **CI/CD**: Jenkins.
- **Infrastructure**: Helm Charts.
- **Programming Language**: .NET Core for the web application.
- **Containerization**: Docker.

---

## <a name="installation"></a>🔧 **Installation**

### **Prerequisites**
Ensure the following tools are installed:
- Kubernetes cluster (e.g., Minikube).
- `kubectl` CLI.
- Helm CLI.
- Docker (for building images).
- Access to the GitHub repository: [https://github.com/yonig15/elta-proj.git](https://github.com/yonig15/elta-proj.git).

---

### **Steps**

#### 1. Create Namespaces
Run the following commands to create the required namespaces:
```bash
kubectl apply -f K8s/namespaces/devops-namespace.yaml
kubectl apply -f K8s/namespaces/prod-namespace.yaml
```

#### 2. Configure Persistent Storage
Deploy the Persistent Volume and Persistent Volume Claim for Jenkins:

```bash
kubectl apply -f K8s/storage/pv.yaml
kubectl apply -f K8s/storage/pvc.yaml
```
Verify the PVC status:

```bash
kubectl get pvc -n devops
```
#### 3. Deploy Jenkins
Use Helm to install Jenkins:

```bash
helm install jenkins helm/ -n devops
```
Verify the Pod status:

```bash
kubectl get pods -n devops
```

#### 4. Expose Jenkins via Service
Apply the NodePort service to expose Jenkins:

```bash
kubectl apply -f K8s/services/jenkins-service.yaml -n devops
```

#### 5. Port-Forward Jenkins
Access Jenkins locally by forwarding the port:

```bash
kubectl port-forward svc/jenkins-service 8080:8080 -n devops
```

Open http://localhost:8080 in your browser.

## <a name="pipeline-workflow"></a>📋 Pipeline Workflow
#### Pipeline Stages
1. Checkout: Clones the code from GitHub.
2. Build: Builds the .NET Core application.
3. Dockerize: Creates a Docker image of the application.
4. Deploy to DevOps: Deploys the application to the devops namespace.

## <a name="troubleshooting"></a>🛠️ Troubleshooting
### Common Issues and Solutions:
1. Jenkins Pod stuck in ContainerCreating.

* Solution: Verify Persistent Volume and Claim configurations:
```bash
kubectl describe pvc jenkins-pvc -n devops
```
2. Jenkins not accessible.
  * Solution: Ensure the NodePort service is correctly configured and use port-forwarding.
3. Pipeline fails at deployment stages.
  * Solution: Check the Kubernetes resource configurations in the deployments/ directory.

## <a name="contributing"></a>👥 Contributing
We welcome contributions! Please fork the repository, make your changes, and submit a pull request.

## <a name="license"></a>📄 License
This project is independent and not licensed under any specific terms.
