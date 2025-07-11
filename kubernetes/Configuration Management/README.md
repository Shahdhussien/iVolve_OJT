# Lab 21: Configuration Management with ConfigMaps and Secrets

## Objective

You will:
- Create a ConfigMap for non-sensitive MySQL configuration values.
- Create a Secret to store sensitive MySQL credentials (base64-encoded).

## Step 1: Create the ConfigMap

We'll create a ConfigMap called mysql-config with the following variables:
- DB_HOST → the hostname of the MySQL service 
- DB_USER → the database user 

### File: mysql-configmap.yaml

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-config
  namespace: ivolve
data:
  DB_HOST: mysql-service
  DB_USER: ivolve_user
```

### Apply the ConfigMap:

```
kubectl apply -f mysql-configmap.yaml
```

### To verify:

```
kubectl get configmap mysql-config -n ivolve -o yaml
```
```
pod-limit   pods: 0/2           21s
shahd@nti:~$ kubectl get resourcequota -n ivolve
NAME        REQUEST     LIMIT   AGE
pod-limit   pods: 0/2           28s
shahd@nti:~$ kubectl get namespaces
NAME              STATUS   AGE
default           Active   5h20m
ivolve            Active   4m46s
kube-node-lease   Active   5h20m
kube-public       Active   5h20m
kube-system       Active   5h20m
shahd@nti:~$ ^C
shahd@nti:~$ kubectl get resourcequota -n ivolve
NAME        REQUEST     LIMIT   AGE
pod-limit   pods: 0/2           4m37s
shahd@nti:~$ ^C
shahd@nti:~$ kubectl describe resourcequota pod-limit -n ivolve
Name:       pod-limit
Namespace:  ivolve
Resource    Used  Hard
--------    ----  ----
pods        0     2
shahd@nti:~$ vim ^[[200~mysql-configmap.yaml~
shahd@nti:~$ vim mysql-configmap.yaml
shahd@nti:~$ kubectl apply -f mysql-configmap.yaml
configmap/mysql-config created
shahd@nti:~$ kubectl apply -f mysql-configmap.yaml
configmap/mysql-config unchanged
shahd@nti:~$ kubectl get configmap mysql-config -n ivolve -o yaml
apiVersion: v1
data:
  DB_HOST: mysql-service
  DB_USER: ivolve_user
kind: ConfigMap
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","data":{"DB_HOST":"mysql-service","DB_USER":"ivolve_user"},"kind":"ConfigMap","metadata":{"annotations":{},"name":"mysql-config","namespace":"ivolve"}}
  creationTimestamp: "2025-07-11T01:28:43Z"
  name: mysql-config
  namespace: ivolve
  resourceVersion: "8859"
```

## Step 2: Create the Secret

We’ll now store these sensitive variables:
- DB_PASSWORD → the password for ivolve_user
- MYSQL_ROOT_PASSWORD → the MySQL root password

```
echo -n "ivolve123" | base64
echo -n "rootpass456" | base64
```
### 🔹 File: mysql-secret.yaml
```
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
  namespace: ivolve
type: Opaque
data:
  DB_PASSWORD: aXZvbHZlMTIz
  MYSQL_ROOT_PASSWORD: cm9vdHBhc3M0NTY=
```

### Apply the Secret:

```
kubectl apply -f mysql-secret.yaml
```

### To verify:

```
kubectl get secret mysql-secret -n ivolve
```
##### Output:
```
NAME           TYPE     DATA   AGE
mysql-secret   Opaque   2      18s
```
```
kubectl describe secret mysql-secret -n ivolve
```

##### Output:
```
Name:         mysql-secret
Namespace:    ivolve
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
DB_PASSWORD:          9 bytes
MYSQL_ROOT_PASSWORD:  11 bytes
```