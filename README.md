# 🛡️ Kubernetes Security Monitoring Lab

[![Kubernetes](https://img.shields.io/badge/Kubernetes-≥_1.25-326ce5.svg)](https://kubernetes.io/)
[![Helm](https://img.shields.io/badge/Helm-≥_3.0-0f1689.svg)](https://helm.sh/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A practical lab for Kubernetes security monitoring with Prometheus, Grafana, and Trivy Operator. This guide will help you set up a complete security monitoring stack on your local macOS environment using Docker.

## 🚀 Features

- ✅ **Real-time monitoring** with Prometheus + Grafana
- ✅ **Vulnerability scanning** with Trivy Operator  
- ✅ **Pre-configured dashboards** and alerts
- ✅ **Vulnerable applications** for testing
- ✅ **Multi-node Kubernetes cluster** (Docker Desktop)

## 📋 Prerequisites

### 🖥️ macOS Tools
```bash
# Required tools
brew install kubectl helm k9s
```

### Docker Desktop (from docker.com)
![k8s-docker](./images/k8s-docker.png)

```bash
# Check tools are installed
kubectl version --client
helm version
docker version

# Verify Kubernetes cluster (Docker Desktop)
kubectl get nodes
# Should show: 1 control-plane + 1 worker node
```

## ⚡ Quick Installation

### 1. Install Kube-Prometheus-Stack

### Add Helm Repository and Update
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

```bash
helm upgrade --install prometheus prometheus-community/kube-prometheus-stack \
  --version 45.7.1 \
  --namespace monitoring \
  --create-namespace \
  --values obs-conf/prom-values.yml
```

### ✅ Validate Installation
```bash
# Check pods status
kubectl get pods -n monitoring --watch

# Verify all components are running
kubectl get all -n monitoring
```

### 🌐 Access Dashboards (Port Forwarding)
```bash
# Access Grafana Dashboard
kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80

# Access Prometheus UI
kubectl port-forward svc/prometheus-operated -n monitoring 9090:9090
```

- **Grafana Access** : http://localhost:3000
- **Credentials**: admin / prom-operator
- **Prometheus Access**: http://localhost:9090

![grafana-dash](./images/grafana-dash.png)

![prometheus-dash](./images/prometheus-dash.png)

### 🛡️ 2. Install Trivy Operator

### Add Helm Repository and Update
```bash
helm repo add aqua https://aquasecurity.github.io/helm-charts
helm repo update
```

```bash
helm upgrade --install trivy-operator aqua/trivy-operator \
  --namespace trivy-system \
  --create-namespace \
  --version v0.30.0 \
  --values obs-conf/trivy-values.yml
```

### ✅ Validate Trivy Installation
```bash
# Check Trivy Operator pods
kubectl get pods -n trivy-system

# Verify node collectors are running on all nodes
kubectl get pods -n trivy-system -o wide | grep node-collector

# Check vulnerability reports
kubectl get vulnerabilityreports --all-namespaces
```

### 🌐 Access Trivy metrics (Port Forwarding)
```bash
kubectl port-forward svc/trivy-operator -n trivy-system 5000:8080
```

![trivy-metrics](trivy-metrics.png)

### 🎯 3. Deploy Vulnerable Applications

```bash
# Deploy both vulnerable applications
kubectl apply -f app-manifests/vul-app-1.yml
kubectl apply -f app-manifests/vul-app-2.yml

# Verify applications are running
kubectl get pods | grep vul-app
```