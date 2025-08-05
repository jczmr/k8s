Introduction to Kubernetes (LFS158)


Linux Command Line Interface (CLI)



container orchestrators

Learning Objectives
By the end of this chapter, you should be able to:

Define the concept of container orchestration.
Explain the benefits of using container orchestration.
Discuss different container orchestration options.
Discuss different container orchestration deployment options.


Development (Dev) Quality Assurance (QA) and Production (Prod) environments

Container orchestrators are tools which group systems together to form clusters where containers' deployment and management is automated 
at scale while meeting the requirements mentioned above. The clustered systems confer the advantages of distributed systems, 
such as increased performance, cost efficiency, reliability, workload distribution, and reduced latency.



managed Kubernetes as-a-Service (KaaS) solution
Google Kubernetes Engine (GKE)



"Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications".

Kubernetes Users
In less than a decade since its debut Kubernetes has become the platform of choice for many enterprises of various sizes to run their workloads. 
It is a solution for workload management in banking, education, finance and investments, gaming, information technology, media and streaming, 
online retail, ridesharing, telecommunications, nuclear research, and many other industries. 


Cloud Native Computing Foundation CNCF

Kubernetes and cloud native associates (KCNA), 
Kubernetes administrators (CKA), 
Kubernetes application developers (CKAD), 
Kubernetes security specialists (CKS)



Chapter Overview
In this chapter, we will explore the Kubernetes architecture, the components of a control plane node, the role of the worker nodes, 
the cluster state management with etcd and the network setup requirements. We will also learn about the Container Network Interface (CNI), as Kubernetes' network specification.



Worker Node Overview
A worker node provides a running environment for client applications. These applications are microservices running as application containers. In Kubernetes the application containers are encapsulated in Pods, controlled by the cluster control plane agents running on the control plane node. Pods are scheduled on worker nodes, where they find required compute, memory and storage resources to run, and networking to talk to each other and the outside world. A Pod is the smallest scheduling work unit in Kubernetes. It is a logical collection of one or more containers scheduled together, and the collection can be started, stopped, or rescheduled as a single unit of work.

Also, in a multi-worker Kubernetes cluster, the network traffic between client users and the containerized applications deployed in Pods is handled directly by the worker nodes, and is not routed through the control plane node.


A worker node has the following components: container runtime, node agent - kubelet, kubelet - CRI shims, proxy - kube-proxy, 
add-ons (for DNS, observability components such as dashboards, cluster-level monitoring and logging, and device plugins).


Minikube

Keep in mind that Minikube now supports all-in-one single-node and multi-node clusters. 


minikube install on WSL2

grep -E --color 'vmx|svm' /proc/cpuinfo

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

minikube start

minikube status

minikube stop


minikube delete


# Advanced Minikube Features(1)

minikube profile list


# Examples of more complex start commands that allow custom clusters to be created with Minikube.

minikube start --nodes=1 --kubernetes-version=v1.33.1 \
  --driver=docker --profile minipod

minikube start --nodes=2 --kubernetes-version=v1.33.1 --driver=docker --profile doubledocker

## provision different cluster of different sizes, driver or runtime  

minikube start --driver=docker -n 2 \
--container-runtime=cri-o \
--kubernetes-version=v1.33.1 -p minidock

## provision with runtime containerd
minikube start --driver=docker -n 3 \
--container-runtime=containerd \
--kubernetes-version=v1.33.1 --cni=calico


# The active marker indicates the target cluster profile of the minikube command line tool, also known as its context. The target cluster can be set to minibox with the following command:
minikube profile minibox

The target cluster can be set to the default minikube with one of the following commands:

minikube profile minikube

minikube profile default

# Using the Profile name
minikube stop -p minibox

minikube start -p minibox

# To display the version of the current Minikube installation:
minikube version



# A command that allows users to list the nodes of a cluster, add new control plane or worker nodes, delete existing cluster nodes, start or stop individual nodes of a cluster:

$ minikube node list

minikube 192.168.59.100

$ minikube node list -p minibox

minibox   192.168.59.101
minibox-m02   192.168.59.102
minibox-m03   192.168.59.103

To display the cluster control plane node's IP address, or another node's IP with the --node or -n flags:

$ minikube ip

192.168.59.100

$ minikube -p minibox ip

192.168.59.101

$ minikube -p minibox ip -n minibox-m02

192.168.59.102

When a cluster configuration is no longer of use, the cluster's profile can be deleted. It is also a profile aware command - it deletes the default minikube cluster if no profile is specified, or a custom cluster if its profile is specified:

$ minikube delete

🔥  Deleting "minikube" in virtualbox ...
💀  Removed all traces of the "minikube" cluster.

$ minikube delete -p minibox

🔥  Deleting "minibox" in virtualbox ...
🔥  Deleting "minibox-m02" in virtualbox ...
🔥  Deleting "minibox-m03" in virtualbox ...
💀  Removed all traces of the "minibox" cluster.


# Chapter Overview
In this chapter, we will learn about different methods of accessing a Kubernetes cluster.

We can use a variety of external clients or custom scripts to access our cluster for administration purposes. We will explore kubectl as a CLI tool to access the Minikube Kubernetes cluster, the Kubernetes Dashboard as a web-based user interface to interact with the cluster, and the curl command with proper credentials to access the cluster via APIs.


# kubectl

kubectl config view

kubectl cluster-info


minikube kubectl -- get namespaces

minikube kubectl -- get nodes

minikube kubectl -- get pods

minikube kubectl -- get services

kubectl version --client --output=yaml


kubectl get namespaces
kubectl get nodes
kubectl get pods
kubectl get services


# Kubernetes Dashboard

minikube addons list
minikube addons enable metrics-server
minikube addons enable dashboard
minikube addons list
minikube dashboard 


> APIs with 'kubectl proxy'
## Issuing the kubectl proxy command, kubectl authenticates with the API server on the control plane node and makes services available on the default proxy port 8001.

kubectl proxy

> APIs with Authentication
When not using the kubectl proxy, we need to authenticate to the API Server when sending API requests. We can authenticate by providing a Bearer Token when issuing a curl command, or by providing a set of keys and certificates.

A Bearer Token is an access token that can be generated by the authentication server (the API Server on the control plane node) at the client's request. Using that token, the client can securely communicate with the Kubernetes API Server without providing additional authentication details, and then, access resources. The token may need to be provided again for subsequent resource access requests. 

Let's create an access token for the default ServiceAccount, and grant special permission to access the root directory of the API (special permission that was not necessary when the kubectl proxy was used earlier). The special permission will be set through a Role Based Access Control (RBAC) policy. The policy is the clusterrole defined below, which is granted through the clusterrolebinding definition (RBAC, clusterroles, and clusterrolebindings will be discussed in a later chapter). The special permission is only needed to access the root directory of the API, but not needed to access /api, /apis, or other subdirectories:

$ export TOKEN=$(kubectl create token default)

$ kubectl create clusterrole api-access-root --verb=get --non-resource-url=/*

$ kubectl create clusterrolebinding api-access-root --clusterrole api-access-root --serviceaccount=default:default

Retrieve the API Server endpoint:

$ export APISERVER=$(kubectl config view | grep https | cut -f 2- -d ":" | tr -d " ")

Confirm that the APISERVER stored the same IP as the Kubernetes control plane IP by issuing the following two commands and comparing their outputs:

$ echo $APISERVER

htt‌ps://192.168.99.100:8443

$ kubectl cluster-info

Kubernetes control plane is running at htt‌ps://192.168.99.100:8443 ...

Access the API Server using the curl command, as shown below:

$ curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure

{
 "paths": [
   "/api",
   "/api/v1",
   "/apis",
   "/apis/apps",
   ......
   ......
   "/logs",
   "/metrics",
   "/openapi/v2",
   "/version"
 ]
}

We can run additional curl commands to retrieve details about specific API groups as follows. These commands should work even without the special permission defined above and granted to the default ServiceAccount associated with the access token: 

$ curl $APISERVER/api/v1 --header "Authorization: Bearer $TOKEN" --insecure

$ curl $APISERVER/apis/apps/v1 --header "Authorization: Bearer $TOKEN" --insecure

$ curl $APISERVER/healthz --header "Authorization: Bearer $TOKEN" --insecure

$ curl $APISERVER/metrics --header "Authorization: Bearer $TOKEN" --insecure

Instead of the access token, we can extract the client certificate, client key, and certificate authority data from the .kube/config file. Once extracted, they can be encoded and then passed with a curl command for authentication. The new curl command would look similar to the example below. Keep in mind, however, that the example command below would only work with the base 64 encoded client certificate, key and certificate authority data, and it is provided only for illustrative purposes.

$ curl $APISERVER --cert encoded-cert --key encoded-key --cacert encoded-ca


> Chapter Overview
In this chapter, we will explore the Kubernetes object model and describe some of its fundamental building blocks, such as Nodes, Namespaces, Pods, ReplicaSets, Deployments, DaemonSets, etc. We will also discuss the essential role of Labels and Selectors in a microservices-driven architecture as they logically group decoupled objects together.

Nodes
- Based on their predetermined functions, there are two distinct types of nodes - control plane and worker.

Namespaces
Pods
- A Pod is the smallest Kubernetes workload object. It is the unit of deployment in Kubernetes, which represents a single instance of the application. A Pod is a logical collection of one or more containers, enclosing and isolating them to ensure that they:

Are scheduled together on the same host with the Pod.
Share the same network namespace, meaning that they share a single IP address originally assigned to the Pod.
Have access to mount the same external storage (volumes) and other common dependencies.

ReplicaSets

Deployments

- Deployment objects provide declarative updates to Pods and ReplicaSets. The DeploymentController is part of the control plane node's controller manager, and as a controller it also ensures that the current state always matches the desired state of our running containerized application. It allows for seamless application updates and rollbacks, known as the default RollingUpdate strategy, through rollouts and rollbacks, and it directly manages its ReplicaSets for application scaling. It also supports a disruptive, less popular update strategy, known as Recreate.

DaemonSets

- DaemonSets are operators designed to manage node agents. They resemble ReplicaSet and Deployment operators when managing multiple Pod replicas and application updates, but the DaemonSets present a distinct feature that enforces a single Pod replica to be placed per Node, on all the Nodes or on a select subset of Nodes. In contrast, the ReplicaSet and Deployment operators by default have no control over the scheduling and placement of multiple Pod replicas on the same Node.

DaemonSet operators are commonly used in cases when we need to collect monitoring data from all Nodes, or to run storage, networking, or proxy daemons on all Nodes, to ensure that we have a specific type of Pod running on all Nodes at all times. They are critical API resources in multi-node Kubernetes clusters. The kube-proxy agent running as a Pod on every single node in the cluster, or the Calico or Cilium networking node agent implementing the Pod networking across all nodes of the cluster, are examples of applications managed by DaemonSet operators.

Whenever a Node is added to the cluster, a Pod from a given DaemonSet is automatically placed on it. Although it ensures an automated process, the DaemonSet's Pods are placed on all cluster's Nodes by the controller itself, and not with the help of the default Scheduler. When any one Node crashes or it is removed from the cluster, the respective DaemonSet operated Pods are garbage collected. If a DaemonSet is deleted, all Pod replicas it created are deleted as well.

The placement of DaemonSet Pods is still governed by scheduling properties which may limit its Pods to be placed only on a subset of the cluster's Nodes.  This can be achieved with the help of Pod scheduling properties such as nodeSelectors, node affinity rules, taints and tolerations. This ensures that Pods of a DaemonSet are placed only on specific Nodes, such as workers if desired. However, the default Scheduler can take over the scheduling process if a corresponding feature is enabled, accepting again node affinity rules.


