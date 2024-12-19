# Demo Project: Configure Monitoring for Own Application

This project showcases how to set up monitoring for a Node.js application using **Prometheus**, **Kubernetes (Amazon EKS)**, **Grafana**, and **Docker**. The aim is to collect, expose, and visualize metrics, providing insights into the application's performance and behavior.

## Project Objectives
- Enable the application to collect and expose metrics.
- Deploy the application in a Kubernetes cluster using Docker.
- Configure Prometheus to scrape metrics from the application.
- Create Grafana dashboards to visualize these metrics.

## Steps to Set Up Monitoring

### 1. Configure Metrics in the Node.js Application
- Add the **Prometheus Client Library** to collect metrics.
- Expose metrics at the `/metrics` endpoint.
- Metrics include:
  - `http_request_operations_total`: Counts HTTP requests.
  - `http_request_duration_seconds`: Tracks request durations.

### 2. Build and Push Docker Image
- Build the Docker image:
  `docker build -t irschad/demo-app:nodeapp .`
- Push the Docker image to DockerHub:
  `docker push irschad/demo-app:nodeapp`

### 3. Deploy the Application in EKS cluster
- Define Kubernetes configurations for:
   Deployment: To run the application through pods.
   Service: To expose the application within the cluster.
- Create a Docker registry secret:
  kubectl create secret docker-registry my-registry-key \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=irschad \
  --docker-password=***

- Apply the Kubernetes configuration:
   `kubectl apply -f k8s-config.yaml`
- Verify the deployment and access the application.

### 4. Configure Prometheus
- Add a ServiceMonitor resource for Prometheus to discover the metrics endpoint.
- Apply the ServiceMonitor configuration: kubectl apply -f k8s-config.yaml
- Verify the target in Prometheus UI under Status > Targets.
- Ensure the target is listed and its status is "UP".

### 5. Visualize Metrics in Grafana
- Create a dashboard and add visualization panels (Add Prometheus as a data source.)
- Requests per Second: Query using `rate(http_request_operations_total[2m])`.
- Request Duration Trends: Query using `rate(http_request_duration_seconds_sum[2m])`.
- Save the dashboard and customize it as needed.


  

