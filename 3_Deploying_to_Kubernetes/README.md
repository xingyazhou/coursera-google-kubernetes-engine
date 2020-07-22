# Qwiklab 3: Deploying to Kubernetes

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

