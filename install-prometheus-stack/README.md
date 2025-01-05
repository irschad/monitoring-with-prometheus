# Install Prometheus Stack on EKS

## Project Overview
**Project:** Install Prometheus Stack in Kubernetes  
**Technologies:** Prometheus, Kubernetes, Helm, AWS EKS, eksctl, Grafana, Linux  

### Project Description:
- Setup an EKS cluster using `eksctl`.
- Deploy Prometheus, Alertmanager, and Grafana as part of the Prometheus Operator using Helm charts.

---

## Steps to Install the Prometheus Stack on EKS

### 1. Create an EKS Cluster
Use the following command to provision a new EKS cluster with managed node groups:  
```bash
eksctl create cluster
```
Ensure the cluster is operational:  
```bash
kubectl get nodes
```

### 2. Deploy Applications
Deploy your application workloads (e.g., microservices) using the appropriate YAML configurations:  
```bash
kubectl apply -f config-microservices.yaml
```
Verify the deployments:  
```bash
kubectl get pods
```

### 3. Install Helm
Download and install Helm:  
```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```
Confirm Helm installation:  
```bash
helm version
```

### 4. Add Prometheus Helm Repository
Add the Prometheus Helm chart repository:  
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
Update Helm repositories:  
```bash
helm repo update
```

### 5. Create a Namespace for Monitoring
Create a dedicated namespace for monitoring components:  
```bash
kubectl create namespace monitoring
```

### 6. Install Prometheus Stack
Deploy the Prometheus stack in the `monitoring` namespace:  
```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring
```

### 7. Verify Installation
Check the status of the Prometheus components:  
```bash
kubectl --namespace monitoring get pods -l "release=monitoring"
```

### 8. Access Prometheus/Grafana
Expose the Prometheus and Grafana services for external access:  
- **Option 1:** Use LoadBalancers.
- **Option 2:** Use port-forwarding.

For port-forwarding, you can run:
```bash
kubectl port-forward svc/<service-name> <local-port>:<service-port> -n monitoring
```

