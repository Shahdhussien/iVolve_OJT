# Lab 19: Node Isolation Using Taints in Kubernetes

## Objective:
Create a 3-node Kubernetes cluster using Minikube, and apply taints to control where workloads can be scheduled.

## Step 1: Start a Minikube Cluster with 3 Nodes
Run the following command to create a Kubernetes cluster with 3 nodes using Docker:
```
minikube start --nodes=3 --driver=docker
```

## Step 2 Verify Cluster Nodes
Check that all 3 nodes are up and running:

```
kubectl get nodes
```
```
NAME           STATUS   ROLES           AGE    VERSION
minikube       Ready    control-plane   5h9m   v1.33.1
minikube-m02   Ready    <none>          5h9m   v1.33.1
minikube-m03   Ready    <none>          5h8m   v1.33.1
```
## Step 3: Apply Taints to Nodes
Apply the required taints to isolate workloads based on node purpose:

```
kubectl taint nodes minikube workload=master:NoSchedule
kubectl taint nodes minikube-m02 workload=app:NoSchedule
kubectl taint nodes minikube-m03 workload=database:NoSchedule
```

## Step 4: Verify Taints
To confirm the taints were applied correctly, run:

```
kubectl describe nodes
```
