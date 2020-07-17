# Qwiklab 2: Kubernetes Basics

## Get the sample code from the Git repository.
git clone https://github.com/googlecodelabs/orchestrate-with-kubernetes.git

## Review the app layout.
cd orchestrate-with-kubernetes/kubernetes

ls

You'll see the following structure.

>deployments/	Deployment manifests

>nginx/	nginx config files

>pods/	pod manifests

>services/	Services manifests

>tls/	TLS certificates

>cleanup.sh	Cleanup script

## Starts a Kubernetes Cluster

gcloud config set compute/zone us-central1-a
gcloud container clusters create bootcamp --num-nodes 5 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
kubectl version

Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.5", GitCommit:"e6503f8d8f769ace2f338794c914a96fc335df0f", GitTreeState:"clean", BuildDate:"2020-06-26T03:47:41Z", GoVersion:"go1.13.9",
 Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"14+", GitVersion:"v1.14.10-gke.36", GitCommit:"34a615f32e9a0c9e97cdb9f749adb392758349a6", GitTreeState:"clean", BuildDate:"2020-04-06T16:33:17Z", GoVersion:"g
o1.12.12b4", Compiler:"gc", Platform:"linux/amd64"}

kubectl cluster-info
Kubernetes master is running at https://35.192.176.131
GLBCDefaultBackend is running at https://35.192.176.131/api/v1/namespaces/kube-system/services/default-http-backend:http/proxy
Heapster is running at https://35.192.176.131/api/v1/namespaces/kube-system/services/heapster/proxy
KubeDNS is running at https://35.192.176.131/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://35.192.176.131/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

## Run and Deploy a Container

kubectl create deployment nginx --image=nginx:1.10.0
kubectl get pods
### Use the kubectl expose command to expose the nginx container outside Kubernetes.
kubectl expose deployment nginx --port 80 --type LoadBalancer
kubectl get services
kubectl scale deployment nginx --replicas 3