# Qwiklab 3: Deploying to Kubernetes

## Introduction
This lab is simply modified to use a wordcount image for deployment. The wordcount image is stored in Google Container Registry.

## Project Files
The project workspace includes 2 files

1. deployments/wordcount.yaml  The deployment configuration file.
2. README.md Provides project info

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

kubectl create will create the wordcount deployment with **three** replicas, using version **v1** of the wordcount container. The number of pods could be simply scaled by chaning the replicas field.

**Create the deployment object using kubectl create.**
```
kubectl create -f deployments/wordcount.yaml
```

**Get Deployments**
```
kubectl get deployments

NAME          READY   UP-TO-DATE   AVAILABLE   AGE
wordcount     3/3     3            3           6s
```

**Get Replicasets**
```
kubectl get replicasets

NAME                    DESIRED   CURRENT   READY   AGE
wordcount-57749b848c    3         3         0       82s
```

**Get Pods**
```
kubectl get pods

NAME                          READY   STATUS    RESTARTS   AGE
wordcount-57749b848c-b8w46    1/1     Running   1          13s
wordcount-57749b848c-ckthp    1/1     Running   1          13s
wordcount-57749b848c-z99hc    1/1     Running   1          13s
```

**Get Logs**
```
kubectl logs -f --tail 50 wordcount-57749b848c-b8w46 

('cat', 2)
('dog', 2)
('snake', 1)
('elephant', 2)
('sheep', 1)
('mouse', 1)
```
**Scale the number of pods**
```
kubectl scale deployment wordcount --replicas=5
```

**Verify there are 5 pods running.**
```
kubectl get pods | grep wordcount- | wc -l

5
```

To learn more about deployment, look at [**this reference**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

## Author
Xingya Zhou
