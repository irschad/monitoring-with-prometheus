# Prometheus Alerting with Email Notifications

This project demonstrates how to set up and manage alerts using Prometheus and Alertmanager with Kubernetes. It utilizes three key YAML configuration files: alert-rules.yaml, alert-manager-configuration.yaml, and email-secret.yaml.

**Objective**: The setup defines monitoring rules for CPU load and Kubernetes pod crashes and sends email notifications when these alerts are triggered.

**Alert Rules**:

alert-rules.yaml: This file contains Prometheus alert rules defined under the PrometheusRule custom resource.
HostHighCpuLoad: Triggers when the CPU load of a host exceeds 50% for 2 minutes.
KubernetesPodCrashLooping: Triggers if a pod restarts more than 5 times in a short period.
Each rule includes labels to classify severity (warning or critical) and annotations to provide detailed descriptions and summaries.

Namespace: All configurations are deployed in the monitoring namespace.

alert-manager-configuration.yaml: This file defines the routing and email notification settings for alerts.

The AlertmanagerConfig resource specifies a route that filters alerts based on their alertname.

Routes are configured to:

Send HostHighCpuLoad alerts every 30 minutes.
Send KubernetesPodCrashLooping alerts every 10 minutes.
A receiver named email is defined to handle email notifications.

The email receiver is configured to send notifications to a Gmail address via the Gmail SMTP server.

email-secret.yaml: Contains the Gmail App password stored as a Kubernetes Secret.

The gmail-auth secret is referenced in the Alertmanager configuration for authentication.

The authUsername, authIdentity, and authPassword fields in the Alertmanager configuration are dynamically linked to the secret.

The Gmail App password in the secret is base64-encoded to ensure security.

**Deployment Steps**:

- Apply the alert-rules.yaml file toto deploy Prometheus rules.
- Apply the email-secret.yaml file to deploy secret.
- Apply the alert-manager-configuration.yaml file to configure Alertmanager.
  
The Prometheus Operator processes the rules and forwards triggered alerts to Alertmanager.

Alertmanager handles routing and ensures notifications are sent to the configured email.

The project leverages Prometheus Operator's custom resources for seamless integration.

Testing: Simulate high CPU usage or pod crashes to verify that alerts are triggered and email notifications are sent.

Future Enhancements: Add more alert rules, integrate with Slack or PagerDuty, and use additional receivers for diversified notifications.







