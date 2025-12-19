# Kubernetes Services – ClusterIP Demo Project

##  Project Overview

This project demonstrates the **core Kubernetes Service concept (ClusterIP)** using a simple backend application. The goal is to understand how Kubernetes provides **stable networking** to ephemeral Pods and performs **load balancing** across multiple replicas.

This project is intentionally application-light and operations-heavy, making it ideal for **DevOps / System Engineer learning**.

---

## 🎯 Objectives

* Understand why Kubernetes Services are required
* Learn how **ClusterIP** works
* Observe Pod IP changes vs Service IP stability
* Verify internal load balancing across replicas

---

## 🏗️ Architecture

```
Test Pod (busybox)
        ↓
   ClusterIP Service
        ↓
Backend Deployment (3 Pods)
```

---

## 🧱 Components Used

### 1️⃣ Backend Application

* **Image**: `nginxdemos/hello`
* **Type**: Stateless HTTP application
* **Container Port**: `80`

The application displays:

* Pod name
* Pod IP
* Request details

This makes it easy to verify load balancing.

---

### 2️⃣ Deployment

* **Replicas**: 3
* Ensures high availability
* Automatically replaces failed Pods

---

### 3️⃣ Service (ClusterIP)

* **Type**: ClusterIP (default)
* Provides a stable virtual IP
* Load-balances traffic across backend Pods
* Accessible only **inside the cluster**

---

## 📂 File Structure

```
clusterip-demo/
├── backend-deployment.yaml
├── backend-service.yaml
└── README.md
```

---

## 🚀 Deployment Steps

### 1️⃣ Apply Deployment

```bash
kubectl apply -f backend-deployment.yaml
```

Verify:

```bash
kubectl get pods -o wide
```

---

### 2️⃣ Apply Service

```bash
kubectl apply -f backend-service.yaml
```

Verify:

```bash
kubectl get svc
```

---

## 🧪 Testing the Service

A temporary test Pod is used to access the ClusterIP Service.

```bash
kubectl run testpod --image=busybox -it --rm -- sh
```

Inside the Pod:

```bash
wget -qO- http://backend-service
```

---

## 🔍 Observations

* Each request returned a **different Pod name**
* Pod IPs changed when Pods were recreated
* **Service IP remained constant**
* Traffic was load-balanced automatically

Example output:

```
Server address: 10.244.2.4:80
Server name: backend-deploy-7f9c9dd8bd-nwngg
URI: /
```
