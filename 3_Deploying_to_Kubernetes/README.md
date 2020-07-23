# Qwiklab 3: Deploying to Kubernetes

## Introduction
This lab is simply modified to use a wordcount image for deployment. The wordcount image is stored in Google Container Registry.



## Start a Kubernetes Cluster
```
$ gcloud config set compute/zone us-central1-a
$ gcloud container clusters create bootcamp --num-nodes 5 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
$ kubectl version
$ kubectl cluster-info
```

## Learn About Deployment Objects
```
kubectl explain deployment
kubectl explain deployment --recursive
kubectl explain deployment.metadata.name
```

## Create a Deployment
**Look at deployments/wordcount.yaml file**
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: wordcount
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: wordcount
        track: stable
        version: v1
    spec:
      containers:
        - name: wordcount
          image: "gcr.io/my-first-gcp-project-271812/wordcount:v1"
          resources:
            limits:
              cpu: 0.2
              memory: "1G"
```

kubectl create will create the wordcount deployment with three replicas, using version v1 of the wordcount container. The number of pods could be simply scaled by chaning the replicas field.

**Create the deployment object using kubectl create.**
```
kubectl create -f deployments/wordcount.yaml
kubectl get pods

wordcount-6bf578cf6d-5s6br    1/1     Running   0          5s
wordcount-6bf578cf6d-brnnt    1/1     Running   0          5s
wordcount-6bf578cf6d-mflzl    1/1     Running   0          5s
```
```
kubectl get replicasets

NAME                    DESIRED   CURRENT   READY   AGE
wordcount-6bf578cf6d    3         3         0       2m22s
```




