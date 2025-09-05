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