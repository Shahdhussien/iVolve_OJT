 # Lab 20: Namespace Management and Resource Quota Enforcement

##  Objective:
Create a namespace named ivolve.

Apply a resource quota that limits the number of pods to a maximum of 2 in that namespace.

## Step 1: Create a Namespace
You can create a namespace called ivolve using this command:

```
kubectl create namespace ivolve
```

### To verify:
```
kubectl get namespaces
```
```
NAME              STATUS   AGE
default           Active   5h20m
ivolve            Active   4m46s
kube-node-lease   Active   5h20m
kube-public       Active   5h20m
kube-system       Active   5h20m
```

## Step 2: Apply a ResourceQuota to Limit Pods

Create a YAML file to define the resource quota.

### 🔹 File: quota.yaml
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: pod-limit
  namespace: ivolve
spec:
  hard:
    pods: "2"
```
##  Step 3: Apply the Resource Quota

```
kubectl apply -f quota.yaml
```

## Step 4: Verify the Quota
To check that the quota was successfully applied:
```
kubectl get resourcequota -n ivolve
```
```
NAME        REQUEST     LIMIT   AGE
pod-limit   pods: 0/2           4m37s
```
