## Kubernetes Architecture

### Nodes
Think of a node as the foundational building block of a Kubernetes cluster. Whether physical or virtual, a node is essentially a machine where Kubernetes is installed. Formerly referred to as “minions,” nodes serve as the execution environment for containers managed by Kubernetes. If a node hosting your application fails, so does your application. Hence, it’s imperative to have multiple nodes for redundancy and scalability.

### Cluster
A cluster is a collection of nodes organized and managed by Kubernetes. By grouping nodes together within a cluster, even if one node experiences failure, your application remains accessible through other nodes. Moreover, distributing workloads across multiple nodes ensures efficient resource utilization and fault tolerance.

### Master
The master node, another node in the cluster with Kubernetes installed, takes charge of managing the cluster. It acts as the orchestrator, overseeing node status, workload distribution, and container orchestration. The master node is crucial for maintaining the stability and performance of the Kubernetes environment.

Components
Upon installing Kubernetes, several essential components come into play:

API Server: 
Serving as the interface for Kubernetes services, the API server facilitates communication between users, management tools, and the cluster itself. All interactions with the Kubernetes cluster occur through the API server.

etcd Key Store:
This distributed and reliable key-value store serves as the backbone for storing crucial cluster data. From node configurations to cluster membership information, etcd ensures consistent and synchronized data across the cluster.

Kubelet Service: 
Running on each node, the Kubelet service acts as an agent responsible for managing containers’ lifecycle and ensuring their proper execution as instructed by the master node.

Container Runtime: 
The underlying software responsible for executing containers on nodes. While Docker is a popular choice, Kubernetes supports other runtimes like rkt or CRI-O, providing flexibility and compatibility with various containerization solutions.

Controllers: 
Serving as the brains behind Kubernetes orchestration, controllers monitor the cluster’s state and respond to changes. They’re tasked with maintaining the desired state of resources, ensuring high availability and fault tolerance.

Schedulers: 
Responsible for workload distribution across nodes, schedulers optimize resource utilization by assigning containers to appropriate nodes based on available resources and constraints.

Distribution Across Servers
In a Kubernetes cluster, components are distributed strategically across master and worker nodes. The master node hosts essential components like the API server, etcd key store, control manager, and scheduler. Conversely, worker nodes, where containers reside, feature the Kubelet agent responsible for communication with the master and container runtime for executing containers.

kubectl Command Line Utility
To interact with the Kubernetes cluster effectively, administrators utilize the kubectl command-line utility:

kubectl run: 
Deploy applications within the cluster.
kubectl cluster-info: 
Obtain comprehensive information about the cluster.
kubectl get nodes: 
List all nodes comprising the cluster, providing insights into its composition and health.

Understanding Kubernetes architecture and leveraging tools like kubectl empowers users to efficiently manage and orchestrate containerized applications within a distributed environment.

## kubectl how to install on Windows

### Install kubectl binary with curl on Windows 

Download the latest 1.29 patch release: kubectl 1.29.1.
```
https://dl.k8s.io/release/v1.29.1/bin/windows/amd64/kubectl.exe
```

Or if you have curl installed, use this command:
```
curl.exe -LO "https://dl.k8s.io/release/v1.29.1/bin/windows/amd64/kubectl.exe"
```  

Append or prepend the kubectl binary folder to your PATH environment variable.

Test to ensure the version of kubectl is the same as downloaded:
```
kubectl version --client
```

## minikube how to install on Windows

What you’ll need:<br>
Docker Desktop installed<br>
2 CPUs or more<br>
2GB of free memory<br>
20GB of free disk space<br>
Internet connection<br>
Container or virtual machine manager, such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation<br>

Download and run the installer for the latest release. Or if using PowerShell, use this command:
```
https://storage.googleapis.com/minikube/releases/latest/minikube-installer.exe
```
```
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```

Add the minikube.exe binary to your PATH. Make sure to run PowerShell as Administrator.
```
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)
}
``` 

Start your cluster:
```
minikube start
```

Validate cluster:
```
kubectl get namespace
```
```
PS C:\Program Files\Kubernetes\Minikube> kubectl get namespace
NAME              STATUS   AGE
default           Active   12m
kube-node-lease   Active   12m
kube-public       Active   12m
kube-system       Active   12m
```

## Kubectl Command Cheatsheet

reference:
```
https://www.bluematador.com/learn/kubectl-cheatsheet
```

Creating Objects
```
kubectl create -f ./file.yml                   # create resource(s) in a json or yaml file

kubectl create -f ./file1.yml -f ./file2.yaml  # create resource(s) in a json or yaml file

kubectl create -f ./dir                        # create resources in all .json, .yml, and .yaml files in dir

# Create from a URL

kubectl create -f http://www.fpaste.org/279276/48569091/raw/

# Create multiple YAML objects from stdin
cat <<EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "1000000"
---
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep-less
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "1000"
EOF

# Create a secret with several keys
cat <<EOF | kubectl create -f -
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  password: $(echo "s33msi4" | base64)
  username: $(echo "jane" | base64)
EOF
```
Viewing, Finding Resources
```
 Columnar output
kubectl get services                          # List all services in the namespace
kubectl get pods --all-namespaces             # List all pods in all namespaces
kubectl get pods -o wide                      # List all pods in the namespace, with more details
kubectl get rc <rc-name>                      # List a particular replication controller
kubectl get replicationcontroller <rc-name>   # List a particular RC

# Verbose output
kubectl describe nodes <node-name>
kubectl describe pods <pod-name>
kubectl describe pods/<pod-name>              # Equivalent to previous
kubectl describe pods <rc-name>               # Lists pods created by <rc-name> using common prefix

# List Services Sorted by Name
kubectl get services --sort-by=.metadata.name

# List pods Sorted by Restart Count
kubectl get pods --sort-by=.status.containerStatuses[0].restartCount

# Get the version label of all pods with label app=cassandra
kubectl get pods --selector=app=cassandra rc -o 'jsonpath={.items[*].metadata.labels.version}'

# Get ExternalIPs of all nodes
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=ExternalIP)].address}'

# List Names of Pods that belong to Particular RC
# "jq" command useful for transformations that are too complex for jsonpath
sel=$(./kubectl get rc <rc-name> --output=json | jq -j '.spec.selector | to_entries | .[] | "\(.key)=\(.value),"')
sel=${sel%?} # Remove trailing comma
pods=$(kubectl get pods --selector=$sel --output=jsonpath={.items..metadata.name})`

# Check which nodes are ready
kubectl get nodes -o jsonpath='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}'| tr ';' "\n"  | grep "Ready=True" 
```

Modifying and Deleting Resources
```
kubectl logs <pod-name>         # dump pod logs (stdout)
kubectl logs -f <pod-name>      # stream pod logs (stdout) until canceled (ctrl-c) or timeout

kubectl run -i --tty busybox --image=busybox -- sh      # Run pod as interactive shell
kubectl attach <podname> -i                             # Attach to Running Container
kubectl port-forward <podname> <local-and-remote-port>  # Forward port of Pod to your local machine
kubectl port-forward <servicename> <port>               # Forward port to service
kubectl exec <pod-name> -- ls /                         # Run command in existing pod (1 container case) 
kubectl exec <pod-name> -c <container-name> -- ls /     # Run command in existing pod (multi-container case) 
```


## How to install ingress on minikube
```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.44.0/deploy/static/provider/cloud/deploy.yaml
```

refrence link:
```
https://minikube.sigs.k8s.io/docs/handbook/accessing/
```



