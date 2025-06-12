# Kubernetes

Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

---

## :pushpin: Table of Contents

1. [Acronyms](#1-acronyms)
2. [Accessing Kubernetes cluster](#2-accessing-kubernetes-cluster)


---

## 1. `Acronyms`

- Kubernetes as-a-Service (KaaS)
- Google Kubernetes Engine (GKE)

- Development (Dev)
- Quality Assurance (QA)
- Production (Prod)

- Linux Command Line Interface (CLI)

- Cloud Native Computing Foundation CNCF
- Kubernetes and cloud native associates (KCNA)
- Kubernetes administrators (CKA)
- Kubernetes application developers (CKAD)
- Kubernetes security specialists (CKS)

- Container Network Interface (CNI)

---

## 2. `Accessing Kubernetes cluster`

- Command Line Interface (CLI)
> kubectl is the Kubernetes Command Line Interface (CLI) client to manage cluster resources and applications. It is very flexible and easy to integrate with other systems, therefore it can be used standalone, or part of scripts and automation tools. Once all required credentials and cluster access points have been configured for kubectl, it can be used remotely from anywhere to access a cluster.

- Kubernetes Dashboard a Web-based User Interface (Web UI)

> Web-based User Interface (Web UI) to interact with a Kubernetes cluster to manage resources and containerized applications. While not as flexible as the kubectl CLI client tool, it is still a preferred tool to users who are not as proficient with the CLI.

- APIs
> The main component of the Kubernetes control plane is the API Server, responsible for exposing the Kubernetes APIs. The APIs allow operators and users to directly interact with the cluster. Using both CLI tools and the Dashboard UI, we can access the API server running on the control plane node to perform various operations to modify the cluster's state. The API Server is accessible through its endpoints by agents and users possessing the required credentials.

