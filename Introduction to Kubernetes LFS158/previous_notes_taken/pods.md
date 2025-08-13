Pods are ephemeral in nature, and they do not have the capability to self-heal themselves. That is the reason they are used with controllers, or operators (controllers/operators are used interchangeably), which handle Pods' replication, fault tolerance, self-healing, etc. Examples of controllers are Deployments, ReplicaSets, DaemonSets, Jobs, etc. When an operator is used to manage an application, the Pod's specification is nested in the controller's definition using the Pod Template.

Below is an example of a stand-alone Pod object's definition manifest in YAML format, without an operator. This represents the declarative method to define an object, and can serve as a template for a much more complex Pod definition manifest if desired:

apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    run: nginx-pod
spec:
  containers:
  - name: nginx-pod
    image: nginx:1.22.1
    ports:
    - containerPort: 80

The apiVersion field must specify v1 for the Pod object definition. The second required field is kind specifying the Pod object type. The third required field metadata, holds the object's name and optional labels and annotations. The fourth required field spec marks the beginning of the block defining the desired state of the Pod object - also named the PodSpec. Our Pod creates a single container running the nginx:1.22.1 image pulled from a container image registry, in this case from Docker Hub. The containerPort field specifies the container port to be exposed by Kubernetes resources for inter-application access or external client access - to be explored in the Services chapter. The contents of spec are evaluated for scheduling purposes, then the kubelet of the selected node becomes responsible for running the container image with the help of the container runtime of the node. The Pod's name and labels are used for workload accounting purposes.

The above definition manifest, if stored by a def-pod.yaml file, is loaded into the cluster to run the desired Pod and its associated container image. While create is exemplified below, advanced Kubernetes practitioners may opt to use apply instead:

$ kubectl create -f def-pod.yaml

Writing up definition manifests, especially complex ones, may prove to be quite time consuming and troublesome because YAML is extremely sensitive to indentation. When eventually editing such definition manifests keep in mind that each indent is two blank spaces wide, and TAB should be omitted.

Imperatively, we can simply run the Pod defined above without the definition manifest as such:

$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80

However, when in need of a starter definition manifest, knowing how to generate one can be a life-saver. The imperative command with additional key flags such as dry-run and the yaml output, can generate the definition template instead of running the Pod, while the template is then stored in the nginx-pod.yaml file. The following is a multi-line command that should be selected in its entirety for copy/paste (including the backslash character "\"):

$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \
--dry-run=client -o yaml > nginx-pod.yaml

The command above generates a definition manifest in YAML, but we can generate a JSON definition file just as easily with:

$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \
--dry-run=client -o json > nginx-pod.json

Both the YAML and JSON definition files can serve as templates or can be loaded into the cluster respectively as such:

$ kubectl create -f nginx-pod.yaml
$ kubectl create -f nginx-pod.json

Before advancing to more complex application deployment and management methods, become familiar with Pod operations with additional commands such as:

$ kubectl apply -f nginx-pod.yaml
$ kubectl get pods
$ kubectl get pods -o wide
$ kubectl get pod nginx-pod -o yaml
$ kubectl get pod nginx-pod -o json
$ kubectl describe pod nginx-pod
$ kubectl delete pod nginx-pod



ReplicaSets can be used independently as Pod controllers but they only offer a limited set of features. A set of complementary features are provided by Deployments, the recommended controllers for the orchestration of Pods. Deployments manage the creation, deletion, and updates of Pods. A Deployment automatically creates a ReplicaSet, which then creates a Pod. There is no need to manage ReplicaSets and Pods separately, the Deployment will manage them on our behalf.

