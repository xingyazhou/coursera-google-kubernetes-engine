# Qwiklab 2: Kubernetes Basics

## Get the sample code from the Git repository.
git clone https://github.com/googlecodelabs/orchestrate-with-kubernetes.git

## Review the app layout.
```
$cd orchestrate-with-kubernetes/kubernetes
$ls
```
| Folder |Description |
| --- | ----------- |
| deployments/   |Deployment manifests       |
| nginx/   | nginx config files       |
| pods/  | pod manifests       |
| tls/   | TLS certificates      |
| cleanup.sh   | Cleanup script       |



## Starts a Kubernetes Cluster
```
$ gcloud config set compute/zone us-central1-a
$ gcloud container clusters create bootcamp --num-nodes 5 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
$ kubectl version
$ kubectl cluster-info
```

## Run and Deploy a Container
```
$ kubectl create deployment nginx --image=nginx:1.10.0
$ kubectl get pods
```

### Use the kubectl expose command to expose the nginx container outside Kubernetes.
```
$ kubectl expose deployment nginx --port 80 --type LoadBalancer
$ kubectl get services 

NAME         TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
kubernetes   ClusterIP      10.51.240.1     <none>         443/TCP        4d16h
nginx        LoadBalancer   10.51.240.255   34.67.173.82   80:30148/TCP   53s
```

```
$ kubectl scale deployment nginx --replicas 3 
$ kubectl get pods

NAME                     READY   STATUS    RESTARTS   AGE
nginx                    1/1     Running   0          58m
nginx-574c99d7c8-h2rxf   1/1     Running   0          24s
nginx-574c99d7c8-sk52m   1/1     Running   0          2m30s
nginx-574c99d7c8-w5q6d   1/1     Running   0          24s
```
