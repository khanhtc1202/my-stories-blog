---
title: Note - Kubernetes in Action 
date: 2019-01-24 17:41:03
thumbnail: 'https://i.imgur.com/uarzO5N.jpg'
tags: technical
---

I took notes during reading "Kubernetes in Action". 

It can be found on [Amazon](https://www.amazon.co.jp/Kubernetes-Action-Marko-Luksa/dp/1617293725/). 

<!-- more -->

# Part 1: Overview

# Part 2: Core concepts

## Chapter 3:

- Po/pods/pod is similar. For example: 
```
kubectl get po
kubectl get pod
kubectl get pods
``` 
It's the same with service/services.

### Introducing pods

**A pod** is a Kubernetes abstraction that represents a group of one or more application containers (such as Docker or rkt), and some shared resources for those containers. You can have several related docker containers run on a pod.

A Pod always runs on a Node. **A Node** is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster. 

#### Create pod with yaml/json
- Create pod from yaml file
```
kubectl create -f [yaml/json file]
```
- Get a list of pods
```
kubectl get po
```
- Get describes of pods
```
kubectl get po [pod_name] -o yaml
```
- Get log of container (-c is optional, when you want to view the log of a specific container)
```
kubectl logs [pod_name] -c [container_name]
```
- Forward port 80 from pod to port 8888 local (effective way to test pod)
```
kubectl port-forward [pod_name] 8888:80
```

### Organizing resources with labels
- Label is an arbitrary key-value attached to a resources. It can be defined under metadata.labels tag. 
- Each pod is recommended to be labeled with 2 labels: app (which microservice it belong to), env (which environment: staging, dev,..)
- Get the list of pods with label
```
kubectl get po --show-labels
```
- Get the pods with specific label by column (in this case is app & rel)
```
kubectl get po -L app,rel
``` 
- Get the pods with label selector
```
///Get the pods have env label
kubectl get po -l env
///Get the pods have specific env label 
kubectl get po -l env=staging
```
- Change label of pods
```
kubectl label po [pod_name] app=frontend
kubectl label po [pod_name] --overwrite app=backend
```

#### Constrain pod scheduling with labels and selectors

All the pods is scheduled randomly across the worker nodes by default.

- Add the label to specific nodes 
```
//get list of nodes
kubectl get nodes --show-labels
//add the label
kubectl label node [node_name] gpu=true
```
- Schedule pods to specific nodes which have gpu label is ```true``` by editing in yaml/json file. Should do like this instead of scheduling to a specific node because the node can be offline.
```
...
spec:
  nodeSelector:
    gpu: "true"
...

```
### Annotation
Mostly the same with label. But annotation is used for large blobs of data (256KB in total); mostly use of annotations is adding descriptions for each pod.

```
kubectl annotate pod [pod_name] mycompany.com/someannotation="this pos is used for..."
```

### Namespaces

