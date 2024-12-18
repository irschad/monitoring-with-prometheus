# Monitoring a Third Party Application (Redis) with Prometheus

This project demonstrates how to monitor a Redis application deployed in Kubernetes (Amazon EKS) using Prometheus and Grafana. The aim is to provide real-time monitoring, alerting, and visualization of Redis metrics to ensure efficient operation and quick issue resolution.

## Project Description
Monitor Redis by using Prometheus Exporter
- Deploy Redis service in our cluster Deploy Redis exporter using Helm Chart
- Configure Alert Rules (when Redis is down or has too many
connections)
- Import Grafana Dashboard for Redis to visualize monitoring
data in Grafana

## Key Features

1. **Deploy Redis Exporter:** Use a Helm chart to deploy the Redis Exporter for Prometheus in a Kubernetes cluster.
2. **Configure Monitoring:** Set up Prometheus to scrape metrics from Redis Exporter and configure alerts for critical scenarios like downtime or high connection usage.
3. **Visualize Data in Grafana:** Import a pre-built Redis dashboard in Grafana for detailed metric analysis and troubleshooting.

## Project Highlights

### Monitoring Setup
- **Deploy Redis Service:** The Redis service is deployed as part of the online shop microservices application.
- **Redis Exporter Deployment:** Install the Redis Exporter using a Helm chart to expose Redis metrics in Prometheus format.

### Alerts Configuration
- Define alerts for critical Redis events:
  - **Redis Down:** Triggers when Redis becomes unreachable.
  - **Too Many Connections:** Notifies when Redis exceeds 90% of its maximum connections.

### Grafana Integration
- Import an existing Grafana dashboard for Redis metrics visualization.
- Analyze metrics like connected clients, memory usage, and query performance.

## How It Works
1. **Service Discovery:** Prometheus discovers the Redis Exporter through a Kubernetes ServiceMonitor.
2. **Metrics Scraping:** Redis Exporter provides metrics such as `redis_connected_clients` and `redis_up`.
3. **Alerting:** Prometheus rules trigger alerts based on pre-configured conditions.
4. **Visualization:** Grafana displays metrics on an intuitive dashboard for analysis.

## Benefits
- Real-time visibility into Redis performance.
- Quick detection and response to critical issues.
- Improved application reliability and user experience.

This project showcases the integration of monitoring tools to efficiently manage and maintain a Redis application in a Kubernetes environment.

