Steps to Install the Prometheus Stack on EKS

Create an EKS Cluster:
Use eksctl create cluster to provision a new EKS cluster with managed node groups. Ensure the cluster is operational using kubectl get nodes.

Deploy Applications (Optional):
Deploy your application workloads (e.g., microservices) using kubectl apply with appropriate YAML configurations. Verify deployments with kubectl get pods.

Install Helm:

Download Helm using the script:
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh

Confirm Helm installation: helm version.

Add Prometheus Helm Repository:

Add the repository:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

Update Helm repositories:
helm repo update

Create a Namespace for Monitoring:
kubectl create namespace monitoring

Install Prometheus Stack:
Use Helm to deploy the Prometheus stack in the monitoring namespace:
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring

Verify Installation:
Check the status of the Prometheus components:

bash
Copy code
kubectl --namespace monitoring get pods -l "release=monitoring"
Access Prometheus/Grafana (Optional):
Expose the Prometheus and Grafana services as LoadBalancers or through port-forwarding for external access.
