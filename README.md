# Kubernetes Networking & Ingress (Learning Project)

## Overview

This project demonstrates my understanding of Kubernetes networking concepts, including Services and Ingress.

It focuses on how Kubernetes efficiently exposes applications and routes external traffic using a single entry point.

---

## Objective

* Understand Kubernetes Service abstraction
* Explore different service types
* Learn how Ingress solves real-world networking challenges
* Design cost-effective traffic routing architecture

---

## ❗ Problem Statement

In Kubernetes, using Service Type **LoadBalancer** for every application can lead to:

* Multiple public IP allocations
* Increased infrastructure cost
* Limited Layer 4 (TCP/UDP) routing
* No support for URL-based or domain-based routing

---

## Proposed Solution

To address these challenges, Kubernetes provides **Ingress**, which enables:

* Path-based routing
* Host-based routing
* Centralized traffic management
* Reduced dependency on multiple LoadBalancers

---

## Key Concepts Learned

### 🔹 Kubernetes Service

A Service provides a stable network identity (IP/DNS) to access a dynamic set of Pods.

Types explored:

* ClusterIP (internal communication)
* NodePort (basic external access)
* LoadBalancer (cloud-based external access)

---

### 🔹 Ingress

Ingress is a Kubernetes API object used to define routing rules for external traffic.

⚠️ Note:
Ingress alone does not work — it requires an Ingress Controller to function.

---

### 🔹 Ingress Controller

An Ingress Controller (such as NGINX) watches Ingress resources and implements routing rules by acting as a reverse proxy and Layer 7 load balancer.

---

### 🔹 Path-Based Routing

Traffic is routed based on URL path.

**Example:**

* `/api` → API Service
* `/admin` → Admin Service

---

### 🔹 Host-Based Routing

Traffic is routed based on domain name.

**Example:**

* `api.myapp.com` → API Service
* `admin.myapp.com` → Admin Service

👉 Multiple domains can point to the same IP, and routing is handled using the Host header.

---

### 🔹 SSL Termination (Concept)

Ingress Controller can handle HTTPS traffic by decrypting it and forwarding plain HTTP traffic to backend services.

---

## Architecture

User → Ingress → Service → Pods

---

## 📂 Project Structure

```
kubernetes-networking-ingress-demo/
│
├── app/
│   └── deployment.yaml
│
├── services/
│   ├── clusterip.yaml
│   ├── nodeport.yaml
│   └── loadbalancer.yaml
│
├── ingress/
│   ├── ingress.yaml
│   └── tls.yaml
│
├── docs/
│   └── architecture.md
│
└── README.md
```

---

## Sample Configurations

### 🔹 Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: nginx
        ports:
        - containerPort: 80
```

---

### 🔹 Service (ClusterIP)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: clusterip-service
spec:
  type: ClusterIP
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 80
```

---

### 🔹 Ingress (Example)

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  rules:
  - host: myapp.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: clusterip-service
            port:
              number: 80
```

---

## Current Status

✔ Concepts understood and documented
✔ YAML configurations prepared
❗ Hands-on deployment in progress / planned

---

## Future Improvements

* Deploy using Minikube / Kind
* Install and test Ingress Controller
* Validate routing using curl/browser
* Implement HTTPS with TLS
* Deploy on cloud (AWS/GCP)

---

## Key Takeaways

* Services provide stable access to dynamic pods
* Ingress enables advanced Layer 7 routing
* Efficient architecture reduces infrastructure cost
* Kubernetes networking is abstraction-driven

---

## Author

Bhoopendra Singh Bhadauria
