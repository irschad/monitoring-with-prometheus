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

NAME                                                   READY   STATUS    RESTARTS   AGE
monitoring-kube-prometheus-operator-696c7b59b4-ppjdm   1/1     Running   0          48s
monitoring-kube-state-metrics-545974fbd4-pk2fj         1/1     Running   0          48s
monitoring-prometheus-node-exporter-9zggs              1/1     Running   0          48s
monitoring-prometheus-node-exporter-bf4hv              1/1     Running   0          48s
```

```bash
kubectl get all -n monitoring
NAME                                                         READY   STATUS    RESTARTS   AGE
pod/alertmanager-monitoring-kube-prometheus-alertmanager-0   2/2     Running   0          89s
pod/monitoring-grafana-5c46fb854f-6gzzc                      3/3     Running   0          95s
pod/monitoring-kube-prometheus-operator-696c7b59b4-ppjdm     1/1     Running   0          95s
pod/monitoring-kube-state-metrics-545974fbd4-pk2fj           1/1     Running   0          95s
pod/monitoring-prometheus-node-exporter-9zggs                1/1     Running   0          95s
pod/monitoring-prometheus-node-exporter-bf4hv                1/1     Running   0          95s
pod/prometheus-monitoring-kube-prometheus-prometheus-0       2/2     Running   0          89s

NAME                                              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/alertmanager-operated                     ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP   89s
service/monitoring-grafana                        ClusterIP   10.100.38.23     <none>        80/TCP                       95s
service/monitoring-kube-prometheus-alertmanager   ClusterIP   10.100.216.130   <none>        9093/TCP,8080/TCP            95s
service/monitoring-kube-prometheus-operator       ClusterIP   10.100.110.235   <none>        443/TCP                      95s
service/monitoring-kube-prometheus-prometheus     ClusterIP   10.100.174.215   <none>        9090/TCP,8080/TCP            95s
service/monitoring-kube-state-metrics             ClusterIP   10.100.181.123   <none>        8080/TCP                     95s
service/monitoring-prometheus-node-exporter       ClusterIP   10.100.65.249    <none>        9100/TCP                     95s
service/prometheus-operated                       ClusterIP   None             <none>        9090/TCP                     89s

NAME                                                 DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
daemonset.apps/monitoring-prometheus-node-exporter   2         2         2       2            2           kubernetes.io/os=linux   95s

NAME                                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/monitoring-grafana                    1/1     1            1           95s
deployment.apps/monitoring-kube-prometheus-operator   1/1     1            1           95s
deployment.apps/monitoring-kube-state-metrics         1/1     1            1           95s

NAME                                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/monitoring-grafana-5c46fb854f                    1         1         1       95s
replicaset.apps/monitoring-kube-prometheus-operator-696c7b59b4   1         1         1       95s
replicaset.apps/monitoring-kube-state-metrics-545974fbd4         1         1         1       95s

NAME                                                                    READY   AGE
statefulset.apps/alertmanager-monitoring-kube-prometheus-alertmanager   1/1     89s
statefulset.apps/prometheus-monitoring-kube-prometheus-prometheus       1/1     89s
```

```bash
kubectl get configmap -n monitoring
NAME                                                           DATA   AGE
kube-root-ca.crt                                               1      7m11s
monitoring-grafana                                             1      6m36s
monitoring-grafana-config-dashboards                           1      6m36s
monitoring-kube-prometheus-alertmanager-overview               1      6m36s
monitoring-kube-prometheus-apiserver                           1      6m35s
monitoring-kube-prometheus-cluster-total                       1      6m36s
monitoring-kube-prometheus-controller-manager                  1      6m35s
monitoring-kube-prometheus-etcd                                1      6m35s
monitoring-kube-prometheus-grafana-datasource                  1      6m36s
monitoring-kube-prometheus-grafana-overview                    1      6m35s
monitoring-kube-prometheus-k8s-coredns                         1      6m36s
monitoring-kube-prometheus-k8s-resources-cluster               1      6m36s
monitoring-kube-prometheus-k8s-resources-multicluster          1      6m36s
monitoring-kube-prometheus-k8s-resources-namespace             1      6m35s
monitoring-kube-prometheus-k8s-resources-node                  1      6m36s
monitoring-kube-prometheus-k8s-resources-pod                   1      6m35s
monitoring-kube-prometheus-k8s-resources-workload              1      6m36s
monitoring-kube-prometheus-k8s-resources-workloads-namespace   1      6m36s
monitoring-kube-prometheus-kubelet                             1      6m36s
monitoring-kube-prometheus-namespace-by-pod                    1      6m36s
monitoring-kube-prometheus-namespace-by-workload               1      6m35s
monitoring-kube-prometheus-node-cluster-rsrc-use               1      6m36s
monitoring-kube-prometheus-node-rsrc-use                       1      6m36s
monitoring-kube-prometheus-nodes                               1      6m35s
monitoring-kube-prometheus-nodes-aix                           1      6m36s
monitoring-kube-prometheus-nodes-darwin                        1      6m36s
monitoring-kube-prometheus-persistentvolumesusage              1      6m36s
monitoring-kube-prometheus-pod-total                           1      6m36s
monitoring-kube-prometheus-prometheus                          1      6m35s
monitoring-kube-prometheus-proxy                               1      6m35s
monitoring-kube-prometheus-scheduler                           1      6m35s
monitoring-kube-prometheus-workload-total                      1      6m36s
prometheus-monitoring-kube-prometheus-prometheus-rulefiles-0   35     6m29s
```

```bash
kubectl get secrets -n monitoring
NAME                                                                                  TYPE                 DATA   AGE
alertmanager-monitoring-kube-prometheus-alertmanager                                  Opaque               1      35m
alertmanager-monitoring-kube-prometheus-alertmanager-generated                        Opaque               1      35m
alertmanager-monitoring-kube-prometheus-alertmanager-tls-assets-0                     Opaque               0      35m
alertmanager-monitoring-kube-prometheus-alertmanager-web-config                       Opaque               1      35m
monitoring-grafana                                                                    Opaque               3      35m
monitoring-kube-prometheus-admission                                                  Opaque               3      35m
prometheus-monitoring-kube-prometheus-prometheus                                      Opaque               1      35m
prometheus-monitoring-kube-prometheus-prometheus-thanos-prometheus-http-client-file   Opaque               1      35m
prometheus-monitoring-kube-prometheus-prometheus-tls-assets-0                         Opaque               1      35m
prometheus-monitoring-kube-prometheus-prometheus-web-config                           Opaque               1      35m
sh.helm.release.v1.monitoring.v1                                                      helm.sh/release.v1   1      35m
```

```bash
kubectl get crd -n monitoring
NAME                                         CREATED AT
alertmanagerconfigs.monitoring.coreos.com    2024-12-15T08:00:49Z
alertmanagers.monitoring.coreos.com          2024-12-15T08:00:49Z
cninodes.vpcresources.k8s.aws                2024-12-15T07:39:05Z
eniconfigs.crd.k8s.amazonaws.com             2024-12-15T07:43:08Z
podmonitors.monitoring.coreos.com            2024-12-15T08:00:50Z
policyendpoints.networking.k8s.aws           2024-12-15T07:39:06Z
probes.monitoring.coreos.com                 2024-12-15T08:00:50Z
prometheusagents.monitoring.coreos.com       2024-12-15T08:00:50Z
prometheuses.monitoring.coreos.com           2024-12-15T08:00:51Z
prometheusrules.monitoring.coreos.com        2024-12-15T08:00:51Z
scrapeconfigs.monitoring.coreos.com          2024-12-15T08:00:51Z
securitygrouppolicies.vpcresources.k8s.aws   2024-12-15T07:39:05Z
servicemonitors.monitoring.coreos.com        2024-12-15T08:00:52Z
thanosrulers.monitoring.coreos.com           2024-12-15T08:00:52Z
```

```bash
kubectl get statefulset -n monitoring
NAME                                                   READY   AGE
alertmanager-monitoring-kube-prometheus-alertmanager   1/1     37m
prometheus-monitoring-kube-prometheus-prometheus       1/1     37m
```

```bash
kubectl describe statefulset prometheus-monitoring-kube-prometheus-prometheus -n monitoring > prom.yaml
kubectl describe statefulset alertmanager-monitoring-kube-prometheus-alertmanager -n monitoring > alert.yaml
```

```bash
kubectl get deployment -n monitoring
NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
monitoring-grafana                    1/1     1            1           41m
monitoring-kube-prometheus-operator   1/1     1            1           41m
monitoring-kube-state-metrics         1/1     1            1           41m
```

```bash
kubectl describe deployment monitoring-kube-prometheus-operator -n monitoring > oper.yaml
```

```bash
kubectl get secret -n monitoring
NAME                                                                                  TYPE                 DATA   AGE
alertmanager-monitoring-kube-prometheus-alertmanager                                  Opaque               1      77m
alertmanager-monitoring-kube-prometheus-alertmanager-generated                        Opaque               1      76m
alertmanager-monitoring-kube-prometheus-alertmanager-tls-assets-0                     Opaque               0      76m
alertmanager-monitoring-kube-prometheus-alertmanager-web-config                       Opaque               1      76m
monitoring-grafana                                                                    Opaque               3      77m
monitoring-kube-prometheus-admission                                                  Opaque               3      77m
prometheus-monitoring-kube-prometheus-prometheus                                      Opaque               1      76m
prometheus-monitoring-kube-prometheus-prometheus-thanos-prometheus-http-client-file   Opaque               1      76m
prometheus-monitoring-kube-prometheus-prometheus-tls-assets-0                         Opaque               1      76m
prometheus-monitoring-kube-prometheus-prometheus-web-config                           Opaque               1      76m
sh.helm.release.v1.monitoring.v1                                                      helm.sh/release.v1   1      77m
```

```bash
kubectl get secret prometheus-monitoring-kube-prometheus-prometheus -n monitoring -o yaml > secret.yaml
kubectl get configmap prometheus-monitoring-kube-prometheus-prometheus-rulefiles-0 -o yaml -n monitoring > config.yaml
```


### 8. Access Prometheus/Grafana
Expose the Prometheus and Grafana services for external access:  
- **Option 1:** Use LoadBalancers.
- **Option 2:** Use port-forwarding.

For port-forwarding, run:
```bash
kubectl port-forward svc/<service-name> <local-port>:<service-port> -n monitoring
```

