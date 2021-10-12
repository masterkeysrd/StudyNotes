# Kubernetes #

Is open-source container orchestrator developed and maintained by Google.

## Architecture of a Kubernetes cluster ##

### Master Node ###

The master node is responsible for the overall management of a
kubernetes cluster. This is brake in tree components that take care of Comunication, 
Scheduling and Controllers these are:

* The API Server
* Scheduler
* Controller Manager

#### The API Server ####

The Kube API Server allows you to interact with the Kubernetes API. It's the frontend
of the Kubernetes control plane.

#### Scheduler ####

The Scheduler watches created pods, who do not have a Node design yet, and designs the
Pod to run on specific Node.

#### Controller Manager ####

The controller manager runs controller. These are background threads that run tasks in
a cluster. The controller have a bunch of different roles, but that's all compiled into
a single binary. The roles include:

* **The Node Controller**: Responsible for the worker states.
* **The Replication Contoller**: Responsible for maintaining the the correct number of
Pods for the replicated controllers.
* **The Endpoint Controller**: Responsible of join services and Pods together.
* **Service account and token controller**: Responsible of handling access management.

### Other components ###

#### etcd ####

Is a simple distributed key value stored. Kubernetes uses etcd as its database and
store all cluster data here. Some of the inforation that might be stored is job
scheduling info, Pods details, stage information, etc.

#### `kubectl` ####

We interact with kubernetes master node using the `kubectl` application, which is the
command like interface for kubernetes.

`kubectl` have a config file called `kubeconfig`. This file has server information, as
well as authentication information to access the API Server.

### Worker Nodes ###

The worker nodes are the nodes where your applications operate. The worker nodes communicate
back with the master node. The communication in the worker node is handle by `kubelet` process.

#### Node ####

The node server as a worker machine in a K8s cluster. One important thing to note is that
the node can be a physical computer or a virtual machine.

**Node requirements:**

* A `kubelet` running
* Container tooling like Docker
* A `kube-proxy` process running
* A `supervisord` process running

#### `kubelet` ####

Its a agent that communicates withe the API Server to see if Pods have been designed to the Nodes,
executes Pod containers via the **container engine**, mounts and run Pod Volume, and Secrets. And
finally, is aware of Pod of Node states and response back to the master node.

> Note: If the kubelet isn't working correctly on the worker node, you're going to have issues.

#### Container Native Platform ####

Kubernetes is a container orchestrator, so we need an container native platform running in the
nodes. This where Docker comes in and work together with the `kubelet` to run container on the
node.

#### `kube-proxy` ####

This process is the Network Proxy and load balancer for the service, on a single worker node.
It handle the network routing for TCP and UDP packets, and perform connection forwarding.

Having a docker daemon allows you to run containers. Containers of an application are tightly
coupled together in a Pod.

#### Pods ####

A pod is the smallest unit that can be scheduled as a deployment in Kubernetes. This group of
containers share storage, Linux Namespace, IP Addresess, amongst other things. They're also call
located and share resources that are always scheduled together. Once Pods have been deployed, and
are running, the `kubelet` process communicates with the pods to check on state and health, and
the `kube-proxy` routes any packets to the pods from other resources that might be wanting to
communicate with them.

**A Pod contains the next:**

* A docker application container(s)
* Storage resources
* Unique network IP
* Options that govern how the container(s) should run

**Pods are**

* Ephemeral, disposable
* Never self-heal, and not restarted by the scheduler by ifself
* Never create pods just by themselves
* Always use higher-level constructs (like a deployment)

**Pod states**

* **Pending**: means that the pod has been accepted by the Kubernetes system, but a container
has not been created yet. 
* **Running**: meas that the pod has been scheduled on a node, and all its containers are
created, and a least one container is in a running state.
* **Succeeded**: means that all of the containers in the pod have exited with an exit stat
of zero, which indicates successful execution, and will no be restarted.
* **Failed**: means all the containers in the pod have exited and a least one container
has failed and return a non-zero exit status.
* **CrashLoopBackOff**: its where a container fails to start, for some reason, and then
kubernetes tries over, and over, and over again to restart the pod.

> Note: A Pod represents one running process on your cluster. We can have several containers
> running in a pod but it's still represents one single unit of deployment, a single instance
> of an application in k8s that's tightly cupled and share resources.

> Note: Workers node can be expose to internet via load balancer. and traffic comming into the
> nodes is also handle by the `kube-proxy`, which is how and end-user ends up talking to a
> kubernetes application.


## K8s Constructs ##

### Controllers ##

**Benefits of Controllers**

* Application reliability
* Scaling
* Load balancing

#### ReplicaSet ####

Ensures that a specified number of replicas for a pod are running at all times.

#### Deployment ####

A Deployment controller provides declarative updates for pods and ReplicaSets.

#### Replication Controller ####

Early implementation of deployments and ReplicaSets.

> Note: Use deployments and ReplicaSets instead.

#### DaemonSets ####

DaemonSets ensure that all nodes run a copy of a specifc pod.

As nodes are added or removed from the cluster, a DaemonSet will add or remove the required pods.

#### Jobs ####

Supervisor process for pods carrying out batch jobs processes to completion

#### Services ####

Allow the communication between one set of deployments with another. This also add network
connectivity to one or more pods in your cluster. When you create a service it's designed
a unique IP address that never changes through the lifetime of the service.

**Kind of Services**

* **Internal**: IP is only reachable within the cluster
* **External**: endpoint available through node ip: port(called NodePort)
* **Load balancer**: exposes application to the internet with a load balancer (available with
a cloud provider)