# CCS3308 - Virtualization and Containers

# Kubernetes Fundamentals with Minikube
## Practical Lab 06


## Student Information

**Student Name:** Mohammed Ashrif  
**Module:** CCS3308 - Virtualization and Containers  
**Lab:** Kubernetes Fundamentals with Minikube  


---

# 1. Introduction

This practical demonstrates the fundamentals of Kubernetes container orchestration using Minikube.

The purpose of this lab is to move from basic Docker container management to Kubernetes-based application deployment and management.

In this practical, a local single-node Kubernetes cluster was created using Minikube. A multi-container application was deployed using publicly available Docker Hub images.

The application architecture used in this lab follows:


Frontend
|
|
API
|
|
Cache
|
|
Database


The lab focuses on Kubernetes Pods, Deployments, Services, StatefulSets, Persistent Storage, Scaling, Rolling Updates, Rollbacks, and Troubleshooting.


---

# 2. Learning Objectives

The main objectives of this practical are:

- Understand why Kubernetes is required beyond Docker.
- Create and manage a local Kubernetes cluster using Minikube.
- Explore Kubernetes control plane and worker node architecture.
- Create Pods using Kubernetes YAML manifests.
- Create Deployments and understand self-healing behaviour.
- Scale applications using Kubernetes replicas.
- Expose applications using Services.
- Perform rolling updates and rollback operations.
- Deploy a multi-tier container application.
- Understand StatefulSets and PersistentVolumeClaims.
- Perform troubleshooting using kubectl commands.


---

# 3. Tools and Technologies Used


| Technology | Purpose |
|------------|---------|
| Ubuntu Linux | Operating System |
| Docker | Container Runtime |
| Kubernetes | Container Orchestration Platform |
| Minikube | Local Kubernetes Cluster |
| kubectl | Kubernetes Command Line Interface |


---

# 4. Kubernetes Cluster Setup


The Kubernetes cluster was created using Minikube.


Command used:

```bash
minikube start --driver=docker

Kubernetes client verification:

kubectl version --client

Check Kubernetes nodes:

kubectl get nodes

The Minikube node was successfully started and the status was verified as Ready.

5. Part 1 - Explore Kubernetes Cluster Architecture

The Kubernetes cluster information was inspected using:

kubectl cluster-info
kubectl get nodes -o wide
kubectl get pods -n kube-system
Kubernetes Components
Control Plane Components

The Kubernetes control plane manages the complete cluster.

Components:

API Server
Scheduler
Controller Manager
etcd
Worker Node Components

Worker nodes run application workloads.

Components:

kubelet
kube-proxy
Container Runtime

The Kubernetes system pods were analysed to understand the cluster architecture.

6. Part 2 - Creating First Kubernetes Pod

A single frontend Pod was created using the nginx:alpine Docker image.

File:

pod-frontend.yaml

Pod configuration:

Pod Name: frontend
Label: app=frontend
Container Image: nginx:alpine
Container Port: 80

Create Pod:

kubectl apply -f pod-frontend.yaml

Check Pod:

kubectl get pods

View Pod details:

kubectl describe pod frontend

View logs:

kubectl logs frontend

Access application:

kubectl port-forward pod/frontend 8080:80

Browser:

http://localhost:8080

The nginx welcome page was successfully displayed.

7. Part 3 - Deployment and Self Healing

A Kubernetes Deployment was created to manage multiple frontend Pods.

File:

deployment-frontend.yaml

Deployment configuration:

Replicas: 3

Apply Deployment:

kubectl apply -f deployment-frontend.yaml

Check Deployment:

kubectl get deployments

Check Pods:

kubectl get pods
Self Healing Test

A running Pod was deleted:

kubectl delete pod <pod-name>

Kubernetes automatically created a replacement Pod.

This demonstrates Kubernetes control loop behaviour:

Desired State
      |
Controller Watches
      |
Actual State
      |
Difference Detected
      |
Reconciliation
8. Part 4 - Scaling Deployment

The frontend Deployment was scaled using replicas.

Scale up to 5 replicas:

kubectl scale deployment frontend --replicas=5

Check Pods:

kubectl get pods

Scale down to 2 replicas:

kubectl scale deployment frontend --replicas=2

Kubernetes automatically adjusted the running Pods.

9. Part 5 - Exposing Application Using Service

A NodePort Service was created to expose the frontend application.

File:

service-frontend.yaml

Apply Service:

kubectl apply -f service-frontend.yaml

Check Service:

kubectl get services

Access application:

minikube service frontend --url

The application was accessed through Kubernetes Service instead of direct Pod access.

10. Part 6 - Rolling Update and Rollback

The frontend Deployment image was updated.

Update command:

kubectl set image deployment/frontend frontend=nginx:1.27-alpine

Check rollout status:

kubectl rollout status deployment/frontend

Rollback command:

kubectl rollout undo deployment/frontend

The Deployment update and rollback process was completed successfully.

11. Part 7 - Multi Tier Application Deployment

The complete application contains four independent tiers.

Frontend Tier

Docker Image:

nginx:alpine

Kubernetes Resources:

Deployment
Service
API Tier

Docker Image:

kennethreitz/httpbin

Kubernetes Resources:

Deployment
ClusterIP Service

Files:

api-deployment.yaml
api-service.yaml
Cache Tier

Docker Image:

redis:7-alpine

Kubernetes Resources:

Deployment
ClusterIP Service

Files:

cache-deployment.yaml
cache-service.yaml
Database Tier

Docker Image:

postgres:16-alpine

Kubernetes Resources:

StatefulSet
PersistentVolumeClaim
Headless Service

Files:

postgres-statefulset.yaml
postgres-service.yaml
12. Part 8 - Database Persistence Verification

PostgreSQL persistence was tested.

Steps performed:

Connected to PostgreSQL database.
Created a test table.
Inserted sample data.
Deleted PostgreSQL Pod.
Kubernetes recreated PostgreSQL Pod.
Verified that the data still existed.

The data remained available because PersistentVolume storage was used.

13. Part 9 - Troubleshooting

A broken Pod was created intentionally using an invalid Docker image tag.

Example:

nginx:definitely-not-a-real-tag

The problem was investigated using:

kubectl get pods
kubectl describe pod broken-pod

The Events section was checked to identify the image pulling failure.

14. Part 10 - Cleanup

All Kubernetes resources were removed using:

kubectl delete -f k8s/

Verify:

kubectl get all

Stop Minikube:

minikube stop
15. Kubernetes Manifest Files

The following YAML files were created:

k8s/

├── pod-frontend.yaml

├── deployment-frontend.yaml

├── service-frontend.yaml

├── api-deployment.yaml

├── api-service.yaml

├── cache-deployment.yaml

├── cache-service.yaml

├── postgres-statefulset.yaml

└── postgres-service.yaml
16. Screenshots

The following screenshots are included:

screenshots/

Task-1.1.png

Task-2.1.png

Task-3.1.png

Task-4.1.png

Task-5.1.png

Task-6.1.png

Task-7.1.png

Task-8.1.png

Task-9.1.png

Task-10.1.png
17. Project Structure
lab_6/

│

├── k8s/

│   ├── pod-frontend.yaml

│   ├── deployment-frontend.yaml

│   ├── service-frontend.yaml

│   ├── api-deployment.yaml

│   ├── api-service.yaml

│   ├── cache-deployment.yaml

│   ├── cache-service.yaml

│   ├── postgres-statefulset.yaml

│   └── postgres-service.yaml


├── screenshots/


├── README.md


└── answers.md
18. Conclusion

This practical provided hands-on experience with Kubernetes container orchestration.

The following Kubernetes concepts were successfully implemented:

Kubernetes Cluster Setup
Pods
Deployments
Services
Self-Healing
Scaling
Rolling Updates
Rollbacks
StatefulSets
Persistent Storage
Troubleshooting

The complete multi-container application was successfully deployed, tested, and managed using Kubernetes and Minikube.
