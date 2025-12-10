# Kubernetes Practice Repository

A comprehensive collection of Kubernetes manifests and configurations for learning and practicing Kubernetes concepts, including microservices deployment, monitoring, logging, and storage management.

## 📁 Repository Structure

### Core Application Components
- **`workloads.yaml`** - Main application deployments (Fleet Management System)
- **`services.yaml`** - Service definitions for application components
- **`storage.yaml`** - Persistent storage configurations
- **`mongo-stack.yaml`** - MongoDB database deployment

### Monitoring & Observability
- **`eks-monitoring.yaml`** - Complete EKS monitoring stack with Prometheus, Grafana, and Alertmanager
- **`alertmanager.yaml`** - Alertmanager configuration for Slack notifications
- **`elastic-stack.yaml`** - ELK stack (Elasticsearch, Logstash, Kibana) for log aggregation
- **`fluentd-config.yaml`** - Fluentd configuration for log collection

### Custom Resources & Testing
- **`crds.yaml`** - Custom Resource Definitions for Prometheus Operator
- **`bad-pod.yaml`** - Intentionally misconfigured pod for troubleshooting practice

## 🚀 Applications & Services

### Fleet Management Microservices
This repository contains a complete microservices-based fleet management application:

#### Core Services
- **Web Application** (`webapp`) - Angular frontend (2 replicas)
- **API Gateway** (`api-gateway`) - Service mesh entry point
- **Position Tracker** (`position-tracker`) - Vehicle position tracking service
- **Position Simulator** (`position-simulator`) - Simulates vehicle movements
- **Message Queue** (`queue`) - ActiveMQ message broker
- **Database** (`mongodb`) - MongoDB for data persistence

#### Service Architecture
```
Internet → LoadBalancer → WebApp → API Gateway → Position Tracker
                                              ↓
                                         Message Queue ← Position Simulator
                                              ↓
                                           MongoDB
```

## 🔧 Infrastructure Components

### Storage
- **StorageClass**: `cloud-ssd` (AWS EBS gp2)
- **PersistentVolumeClaim**: 7Gi for MongoDB data persistence

### Monitoring Stack
- **Prometheus** - Metrics collection and alerting
- **Grafana** - Visualization dashboards
- **Alertmanager** - Alert routing and notification
- **Node Exporter** - System metrics collection
- **Kube State Metrics** - Kubernetes object metrics

### Logging Stack
- **Elasticsearch** - Log storage and indexing (2 replicas, 31Gi storage)
- **Fluentd** - Log collection from all nodes (DaemonSet)
- **Kibana** - Log visualization and analysis

## 📊 Monitoring & Alerting

### Grafana Dashboards
The monitoring stack includes pre-configured dashboards for:
- Kubernetes API Server performance
- Cluster networking metrics
- Alertmanager overview
- Node and pod resource utilization

### Alert Configuration
- **Slack Integration**: Alerts sent to `#alerts` channel
- **Alert Grouping**: By alertname with 5s group wait
- **Repeat Interval**: 10 minutes for persistent issues

## 🗂️ Key Features

### Production-Ready Configurations
- **Environment Variables**: All services configured for `production-microservice` profile
- **Resource Limits**: Appropriate CPU and memory constraints
- **Health Checks**: Readiness and liveness probes
- **Persistent Storage**: Stateful data persistence for MongoDB

### Security & RBAC
- **Service Accounts**: Dedicated accounts for monitoring components
- **Cluster Roles**: Appropriate permissions for system components
- **Network Policies**: Implicit through service definitions

### Scalability
- **Horizontal Scaling**: Multiple replicas for web application
- **Storage Scaling**: Configurable persistent volume sizes
- **Monitoring Scaling**: Distributed monitoring architecture

## 🚀 Quick Start

### Prerequisites
- Kubernetes cluster (1.14+)
- kubectl configured
- StorageClass support (for persistent volumes)

### Deployment Order

1. **Storage & Database**
   ```bash
   kubectl apply -f storage.yaml
   kubectl apply -f mongo-stack.yaml
   ```

2. **Core Application**
   ```bash
   kubectl apply -f workloads.yaml
   kubectl apply -f services.yaml
   ```

3. **Monitoring (Optional)**
   ```bash
   kubectl apply -f crds.yaml
   kubectl apply -f eks-monitoring.yaml
   kubectl apply -f alertmanager.yaml
   ```

4. **Logging (Optional)**
   ```bash
   kubectl apply -f fluentd-config.yaml
   kubectl apply -f elastic-stack.yaml
   ```

### Access Applications

- **Web Application**: Access via LoadBalancer service
- **Grafana**: Port-forward to access dashboards
- **Kibana**: Port-forward to access logs
- **MongoDB**: Internal cluster access only

## 🔍 Troubleshooting

### Common Issues
- **Storage**: Ensure StorageClass `cloud-ssd` is available
- **Images**: Verify all container images are accessible
- **Resources**: Check cluster has sufficient CPU/memory
- **Networking**: Ensure LoadBalancer support for external access

### Debug Resources
- **`bad-pod.yaml`**: Intentionally broken deployment for practice
- **Logs**: Use Kibana or `kubectl logs` for troubleshooting
- **Metrics**: Monitor resource usage via Grafana

## 📚 Learning Objectives

This repository helps practice:
- **Microservices Architecture**: Multi-service application deployment
- **Storage Management**: Persistent volumes and storage classes
- **Monitoring & Observability**: Prometheus, Grafana, and alerting
- **Log Management**: Centralized logging with ELK stack
- **Service Discovery**: Kubernetes native service networking
- **Configuration Management**: ConfigMaps and environment variables
- **Troubleshooting**: Debugging failed deployments and services

## 🛠️ Technologies Used

- **Container Orchestration**: Kubernetes
- **Monitoring**: Prometheus, Grafana, Alertmanager
- **Logging**: Elasticsearch, Fluentd, Kibana
- **Database**: MongoDB
- **Message Queue**: ActiveMQ
- **Frontend**: Angular
- **Backend**: Spring Boot microservices
- **Storage**: AWS EBS (via StorageClass)

## 📝 Notes

- All services are configured for production environments
- Monitoring stack includes comprehensive Kubernetes dashboards
- Logging configuration captures system and application logs
- Storage is configured for cloud environments (AWS EBS)
- Alert notifications require Slack webhook configuration

---

**Happy Learning!** 🎓 This repository provides a comprehensive environment for practicing Kubernetes concepts from basic deployments to advanced monitoring and logging configurations.