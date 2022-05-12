---
layout: post
title:  "Continuous orchestration with Kubernetes"
date:   2021-09-24 00:00:00 +0200
categories: jekyll update
---

## Continuous orchestration

Continuous orchestration is the ability to automate the deployment, management, scaling and networking of containers. It responds dynamically to changes in the environment and ensures all deployed container instances get updated if a new version of a service is released.

There are several tools used for container lifecycle management, like the following:
* Kubernetes
* Docker Swarm
* Openshift
* Apache Mesos
* HashiCorp Nomad

## Kubernetes

Kubernetes is a powerfull open source container orchestration tool. It is also known as "*k8s*" or "*kube*".

It offers several features:
* Automatic rollouts and rollbacks
* Load balancing
* Simple horizontal scaling
* Self-healing
* Automatic bin packing
* Batch execution
* Storage orchestration
* IPv4 and IPv6 support

### Kubernetes components

A working Kubernetes component is called a cluster. It has two main components that make the architecture of the system:
* Control plane: Makes the global decisions about the cluster. It has three components:
  * kube apiserver
  * kube scheduler
  * kube controller manager
  * etcd: key-value storage.
* Compute machines: Also known as nodes. They run several components:
  * kubelet
  * kube proxy
  * container runtime
  * pod: A sigle instance of an application

### Kubernetes installation

Note that you must have Docker installed as a prerequisite.

The following is a script that installs Kubernetes on a system running Ubuntu, and it was made from the information provided [here](https://ubuntu.com/kubernetes/install):
```bash
sudo snap install microk8s --classic
sudo usermod -a -G microk8s $USER
sudo chown -f R $USER ~/.kube
su - $USER
microk8s status --wait-ready
microk8s enable dashboard dns ingress
microk8s kubectl get all --all-namespaces
microk8s dashboard-proxy
```

### Kubernetes basics

* Pods: Collection of one or more containers with shared storage.
  * They can contain multiple apps, and made up from a template.
  * They can run on a single container, or on multiple containers.
* Labels: key-value pairs that Kubernetes attaches to objects. Some examples are:
  * "release":"stable"
  * "tier":"frontend"
  * "track":"daily"
* Selectors: Core grouping primitives. There are two types of selectors:
  * Equality-based selectors: They use label keys and values.
  * Set-based selectors: They filter keys according to a set of values.
* Controllers: Monitor the state of the cluster. These are the types of controllers:
  * Replication controller
  * Daemon set controller
  * Job controller
* Replica set: Ensures that a set of replica pods is running at any given time.
* Deployments and services: Controller that transforms the current state into the intended state. They are used to:
  * Clean up older replica sets.
  * Roll out a replica set.
  * Declare the new state of pods.
  * Indicate that a rollout is stuck.
  * Apply multiple fixes to pods.
  * Facilitate more load.
* Services: An abstraction that defines a logical set of pods as well as a policy for accessing them. There are two ways of accessing a service:
  * DSN method: A name is given to the service when initialized.
  * Environment variable method: kubelet adds envionment variables for each active service.
  
* Services types:
  * ClusterIP: Default.
  * NodePort: Exposes a port.
  * LoadBalancer: Cloud provider load balancer is used.
  * ExternalName: Assigns a CNAME.

### Kubernetes best practises

* Update Kubernetes to the latest version
* Use Pod Security Policies
* Use Kubernetes namespaces
* Use Network Policies
* Configure the Kubernetes API server securely
* Maintain small container images

### Managing a Kubernetes cluster

Kubernetes provides a cli tool called kubectl that allows us to manage the cluster.

To initialize the cluster, run `sudo kubeadm init`.

To reset the cluster, run `sudo kubeadm reset`.

To list the nodes and their status, run `kubectl get nodes`.

To create a pod, run `kubectl run <podname> --image=<imagename:version>`.

To get information about a pod, run `kubectl describe pods <podname>`.

To delete a pod, run `kubectl delete pods <podname>`.