# ☸️ Kubernetes & Google Kubernetes Engine Labs

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Google Cloud](https://img.shields.io/badge/Google%20Cloud-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white)
![GKE](https://img.shields.io/badge/GKE-Cloud%20Kubernetes-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

> 🚀 Hands-on Kubernetes and Google Kubernetes Engine learning repository containing labs, commands, deployment strategies, troubleshooting, monitoring and container deployment.

---

# 📚 About This Repository

This repository contains my hands-on learning and practical implementation of:

- Kubernetes fundamentals
- Google Kubernetes Engine (GKE)
- Kubernetes Deployments
- ReplicaSets
- Pods
- Services
- Scaling
- Rolling Updates
- Rollbacks
- Canary Deployments
- Blue-Green Deployments
- Managed Prometheus
- Pod Monitoring
- Logs-based Metrics
- Alerting Policies
- Artifact Registry
- Docker containerization
- Application deployment on GKE

The objective is to understand not only **how to run Kubernetes commands**, but also **why and when each Kubernetes deployment strategy is used**.

---

# 🧠 Kubernetes Architecture

```text
                    ┌──────────────────────────┐
                    │        User / Client     │
                    └────────────┬─────────────┘
                                 │
                                 ▼
                    ┌──────────────────────────┐
                    │      LoadBalancer        │
                    │        Service           │
                    └────────────┬─────────────┘
                                 │
                  ┌──────────────┴──────────────┐
                  │                             │
                  ▼                             ▼
        ┌─────────────────┐           ┌─────────────────┐
        │   Deployment    │           │   Deployment    │
        │      Blue       │           │      Green      │
        └────────┬────────┘           └────────┬────────┘
                 │                             │
          ┌──────┼──────┐              ┌──────┼──────┐
          ▼      ▼      ▼              ▼      ▼      ▼
        Pod    Pod    Pod            Pod    Pod    Pod
````

# 🧪 Labs Completed

## 1️⃣ Managing Deployments Using Kubernetes Engine

**Lab:** GSP053

### Topics Covered

* `kubectl`
* Deployment YAML
* Deployment creation
* ReplicaSets
* Pods
* Kubernetes Services
* Scaling
* Rolling Updates
* Rollout History
* Pause / Resume Rollouts
* Rollback
* Canary Deployment
* Blue-Green Deployment

---

# 🚀 Kubernetes Deployment

A Deployment manages a group of Pods and makes it easier to:

* Deploy applications
* Scale applications
* Update applications
* Roll back applications

Example:

```bash
kubectl create -f deployment.yaml
```

Check deployments:

```bash
kubectl get deployments
```

Check ReplicaSets:

```bash
kubectl get replicasets
```

Check Pods:

```bash
kubectl get pods
```

---

# 📈 Scaling

Scale a Deployment:

```bash
kubectl scale deployment fortune-app-blue --replicas=5
```

Scale back:

```bash
kubectl scale deployment fortune-app-blue --replicas=3
```

Verify:

```bash
kubectl get pods
```

### Concept

```text
Deployment
     │
     ▼
ReplicaSet
     │
 ┌───┼───┐
 ▼   ▼   ▼
Pod Pod Pod
```

If we increase replicas from 3 → 5:

```text
3 Pods
  ↓
5 Pods
```

Kubernetes automatically creates the additional Pods.

---

# 🔄 Rolling Update

Rolling Update allows us to update an application without taking the entire application offline.

Example:

```bash
kubectl edit deployment fortune-app-blue
```

Change:

```text
1.0.0
```

to:

```text
2.0.0
```

Check rollout:

```bash
kubectl rollout status deployment/fortune-app-blue
```

View rollout history:

```bash
kubectl rollout history deployment/fortune-app-blue
```

---

# ⏸️ Pause Rolling Update

```bash
kubectl rollout pause deployment/fortune-app-blue
```

Check status:

```bash
kubectl rollout status deployment/fortune-app-blue
```

---

# ▶️ Resume Rolling Update

```bash
kubectl rollout resume deployment/fortune-app-blue
```

---

# ↩️ Rollback

If the new version has a problem:

```bash
kubectl rollout undo deployment/fortune-app-blue
```

Verify:

```bash
kubectl rollout status deployment/fortune-app-blue
```

### Easy Memory Trick

```text
Deploy
  ↓
Update
  ↓
Problem?
  ↓
Rollback
```

---

# 🐤 Canary Deployment

Canary deployment means:

> Send only a small amount of traffic to the new version first.

Example:

```text
Users
  │
  ▼
Service
  │
  ├───────────────┐
  ▼               ▼
Version 1.0      Version 2.0
  │               │
  │               │
  │               └── Small traffic
  │
  └── Majority traffic
```

Advantages:

* Safer releases
* Test new version with real traffic
* Reduce deployment risk

---

# 🔵🟢 Blue-Green Deployment

Blue-Green deployment keeps two versions available.

```text
              Service
                 │
       ┌─────────┴─────────┐
       ▼                   ▼
    🔵 BLUE             🟢 GREEN
    v1.0                  v2.0
```

Initially:

```text
Service → BLUE
```

After testing:

```text
Service → GREEN
```

Rollback:

```text
Service → BLUE
```

The important point is:

> We switch traffic by changing the Service selector.

---

# 🆚 Deployment Strategies

| Strategy       | Main Idea                                |
| -------------- | ---------------------------------------- |
| Rolling Update | Gradually replace old Pods               |
| Canary         | Send small traffic to new version        |
| Blue-Green     | Keep two environments and switch traffic |
| Rollback       | Return to previous version               |

---

# 2️⃣ GKE Challenge Lab

**Lab:** GSP510

This challenge lab tested practical GKE administration skills.

---

# ☁️ GKE Cluster

Created a GKE cluster with:

```text
Cluster Name:
hello-world-ptyy

Region:
us-east1

Zone:
us-east1-b

Release Channel:
Regular

Nodes:
3

Minimum Nodes:
2

Maximum Nodes:
6

Autoscaling:
Enabled
```

---

# 📊 Managed Prometheus

Enabled Managed Prometheus on GKE for monitoring.

Created namespace:

```bash
kubectl create namespace gmp-hpxg
```

Managed Prometheus allows Kubernetes workloads to expose metrics that can be collected and monitored.

---

# 📦 Prometheus Application

Configured:

```text
Image:
nilebox/prometheus-example-app:latest

Container:
prometheus-test

Port Name:
metrics
```

Deployed inside:

```text
gmp-hpxg
```

---

# 🔎 Pod Monitoring

Configured PodMonitoring with:

```text
Name:
prometheus-test

Label:
app.kubernetes.io/name: prometheus-test

Match Label:
app: prometheus-test

Interval:
45s
```

Conceptually:

```text
Prometheus
     │
     │ collects metrics
     ▼
PodMonitoring
     │
     ▼
prometheus-test Pods
```

---

# 🐳 Application Deployment

Downloaded the sample application:

```bash
gcloud storage cp -r gs://spls/gsp510/hello-app/ .
```

Initially deployed the application with an incorrect image.

This produced:

```text
InvalidImageName
```

This was intentional because the challenge lab required troubleshooting.

---

# 📜 Logs-Based Metric

Created a logs-based metric:

```text
pod-image-errors
```

Metric type:

```text
Counter
```

The metric tracks image-related errors in Kubernetes workloads.

---

# 🚨 Alerting Policy

Created:

```text
Pod Error Alert
```

Configuration:

```text
Rolling Window:
10 minutes

Function:
Count

Aggregation:
Sum

Condition:
Threshold

Trigger:
Any time series violates

Threshold:
Above 0

Notification:
Disabled
```

---

# 🔧 Fixing the Deployment

The incorrect image was replaced with:

```text
us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
```

The old deployment was deleted and the corrected deployment was created.

---

# 🐳 Containerizing Application

Updated:

```text
main.go
```

Application version:

```text
2.0.0
```

Created a Docker image using the application's Dockerfile.

Image tag:

```text
v2
```

Pushed the image to:

```text
Artifact Registry
```

---

# 🌐 Exposing Application

Created a LoadBalancer service:

```text
helloweb-service-qg0s
```

The application was exposed externally through the LoadBalancer.

Expected response:

```text
Hello, world!
Version: 2.0.0
Hostname: helloweb-xxxxxxxxxx
```

---

# 🧰 Important kubectl Commands

## Cluster

```bash
kubectl cluster-info
```

## Nodes

```bash
kubectl get nodes
```

## Pods

```bash
kubectl get pods
```

Detailed Pod information:

```bash
kubectl describe pod <pod-name>
```

## Deployments

```bash
kubectl get deployments
```

Detailed deployment:

```bash
kubectl describe deployment <deployment-name>
```

## ReplicaSets

```bash
kubectl get replicasets
```

## Services

```bash
kubectl get services
```

Short form:

```bash
kubectl get svc
```

## Namespaces

```bash
kubectl get namespaces
```

## Create Namespace

```bash
kubectl create namespace <namespace-name>
```

## Apply YAML

```bash
kubectl apply -f file.yaml
```

## Create from YAML

```bash
kubectl create -f file.yaml
```

## Delete Deployment

```bash
kubectl delete deployment <deployment-name>
```

## Scale

```bash
kubectl scale deployment <deployment-name> --replicas=5
```

---

# 🔄 Rollout Commands

Check rollout:

```bash
kubectl rollout status deployment/<deployment-name>
```

View history:

```bash
kubectl rollout history deployment/<deployment-name>
```

Pause:

```bash
kubectl rollout pause deployment/<deployment-name>
```

Resume:

```bash
kubectl rollout resume deployment/<deployment-name>
```

Rollback:

```bash
kubectl rollout undo deployment/<deployment-name>
```

---

# 🔍 Debugging Commands

Check Pods:

```bash
kubectl get pods
```

Detailed information:

```bash
kubectl describe pod <pod-name>
```

Check logs:

```bash
kubectl logs <pod-name>
```

Follow logs:

```bash
kubectl logs -f <pod-name>
```

Check events:

```bash
kubectl get events
```

---

# 🧠 Important Kubernetes Concepts

## Pod

Smallest deployable unit in Kubernetes.

```text
Pod
 └── Container
```

---

## ReplicaSet

Maintains the desired number of Pods.

```text
ReplicaSet
 ├── Pod
 ├── Pod
 └── Pod
```

---

## Deployment

Manages ReplicaSets and application updates.

```text
Deployment
     ↓
ReplicaSet
     ↓
Pods
```

---

## Service

Provides stable networking and access to Pods.

```text
Client
  ↓
Service
  ↓
Pods
```

---

# 🧠 Kubernetes Mental Model

Remember this hierarchy:

```text
Deployment
     ↓
ReplicaSet
     ↓
Pods
     ↓
Containers
```

And networking:

```text
User
 ↓
Service
 ↓
Pods
 ↓
Containers
```

---

# 🎯 What I Learned

Through these hands-on labs I practiced:

* Creating GKE clusters
* Managing Kubernetes clusters
* Working with kubectl
* Creating Kubernetes YAML manifests
* Deploying applications
* Scaling applications
* Managing ReplicaSets
* Performing rolling updates
* Pausing and resuming rollouts
* Rolling back deployments
* Implementing Canary deployments
* Implementing Blue-Green deployments
* Enabling Managed Prometheus
* Configuring PodMonitoring
* Debugging Kubernetes deployment errors
* Creating logs-based metrics
* Creating alerting policies
* Working with Artifact Registry
* Building Docker images
* Pushing images to Artifact Registry
* Exposing applications through LoadBalancer Services

---

# 🗺️ Learning Progress

```text
Docker
  │
  ▼
Kubernetes Basics
  │
  ▼
Pods
  │
  ▼
Deployments
  │
  ▼
ReplicaSets
  │
  ▼
Services
  │
  ▼
Scaling
  │
  ▼
Rolling Updates
  │
  ▼
Rollback
  │
  ▼
Canary Deployment
  │
  ▼
Blue-Green Deployment
  │
  ▼
GKE
  │
  ▼
Managed Prometheus
  │
  ▼
Monitoring & Alerting
  │
  ▼
Artifact Registry
  │
  ▼
Docker + GKE Deployment
```

---

# 🚀 Next Learning Goals

The next areas to explore are:

* Kubernetes ConfigMaps
* Kubernetes Secrets
* Persistent Volumes
* Persistent Volume Claims
* Ingress
* Helm
* Kubernetes networking
* RBAC
* Service Accounts
* Horizontal Pod Autoscaler
* Kubernetes troubleshooting
* GKE production architecture
* CI/CD with Kubernetes
* GitHub Actions + GKE
* Terraform + GKE
* Observability
* Prometheus & Grafana

---

# 🏆 Labs Completed

| Lab                 | Topic                                        | Status      |
| ------------------- | -------------------------------------------- | ----------- |
| GSP053              | Managing Deployments Using Kubernetes Engine | ✅ Completed |
| GSP510              | GKE Challenge Lab                            | ✅ Completed |
| Kubernetes Practice | Deployments & Rollouts                       | ✅ Completed |
| GKE Monitoring      | Managed Prometheus                           | ✅ Completed |

---

# 💡 Key Takeaway

Kubernetes is not just about running containers.

It is about **managing application lifecycle**:

```text
Deploy
  ↓
Scale
  ↓
Monitor
  ↓
Update
  ↓
Detect Problems
  ↓
Rollback / Fix
  ↓
Release Safely
```

The deployment strategies learned in these labs help achieve safer and more reliable application releases.

---

## 📌 Technologies

```text
Kubernetes
Google Kubernetes Engine (GKE)
kubectl
Docker
Artifact Registry
Managed Prometheus
PodMonitoring
Cloud Logging
Cloud Monitoring
YAML
Google Cloud
```

---

# ⭐ Repository Purpose

This repository documents my practical journey of learning and implementing Kubernetes and GKE concepts through hands-on Google Cloud labs.

It focuses on **practical commands, troubleshooting, deployment strategies, monitoring, and real-world DevOps practices**.
