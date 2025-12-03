# Kubernetes Glossary

A comprehensive glossary of Kubernetes terms and concepts used throughout this repository.

## Core Concepts

### **Cluster**
A set of nodes (machines) that run containerized applications managed by Kubernetes.

### **Node**
A worker machine in Kubernetes (physical or virtual). Each node contains the services necessary to run Pods.

### **Control Plane**
The container orchestration layer that manages the worker nodes and Pods in the cluster. Includes components like API server, scheduler, and controller manager.

### **Namespace**
Virtual clusters within a physical cluster. Used to divide cluster resources between multiple users or teams.

### **Label**
Key-value pairs attached to objects (like Pods) used for organizing and selecting subsets of objects.

### **Annotation**
Key-value pairs that can attach arbitrary metadata to objects. Not used for selection.

### **Selector**
Allows you to filter resources based on labels.

---

## Workloads

### **Pod**
The smallest deployable unit in Kubernetes. Contains one or more containers that share storage and network resources.

### **Deployment**
Manages a replicated application, providing declarative updates for Pods and ReplicaSets.

### **ReplicaSet**
Ensures a specified number of Pod replicas are running at any given time. Usually managed by Deployments.

### **StatefulSet**
Manages stateful applications, providing guarantees about ordering and uniqueness of Pods. Used for databases.

### **DaemonSet**
Ensures all (or some) nodes run a copy of a Pod. Used for node-level services like log collectors.

### **Job**
Creates one or more Pods and ensures a specified number successfully complete.

### **CronJob**
Creates Jobs on a time-based schedule (like cron in Unix/Linux).

---

## Networking

### **Service**
An abstract way to expose an application running on a set of Pods as a network service.

#### Service Types:
- **ClusterIP**: Exposes service on cluster-internal IP (default)
- **NodePort**: Exposes service on each node's IP at a static port
- **LoadBalancer**: Exposes service externally using cloud provider's load balancer
- **ExternalName**: Maps service to a DNS name

### **Ingress**
Manages external access to services, typically HTTP/HTTPS. Provides load balancing, SSL termination, and name-based virtual hosting.

### **Ingress Controller**
The actual implementation that fulfills the Ingress rules (e.g., NGINX, Traefik, HAProxy).

### **Network Policy**
Specification of how groups of Pods are allowed to communicate with each other and other network endpoints.

### **Endpoint**
Represents the network addresses of Pods that match a Service selector.

### **DNS**
Kubernetes provides DNS for services and Pods, allowing service discovery.

---

## Storage

### **Volume**
A directory accessible to containers in a Pod. Can persist beyond container restarts.

### **PersistentVolume (PV)**
A piece of storage in the cluster provisioned by an administrator or dynamically provisioned.

### **PersistentVolumeClaim (PVC)**
A request for storage by a user. Claims can request specific size and access modes.

### **StorageClass**
Provides a way to describe different "classes" of storage. Used for dynamic provisioning.

### **VolumeSnapshot**
A snapshot of a volume's contents at a particular point in time.

#### Volume Types:
- **emptyDir**: Temporary storage, deleted when Pod is removed
- **hostPath**: Mounts file/directory from host node
- **configMap**: Provides configuration data to Pods
- **secret**: Provides sensitive data to Pods
- **persistentVolumeClaim**: Mounts a PersistentVolume

---

## Configuration

### **ConfigMap**
Stores non-confidential configuration data in key-value pairs. Can be consumed as environment variables, command-line arguments, or configuration files.

### **Secret**
Stores sensitive information like passwords, OAuth tokens, and SSH keys. Similar to ConfigMap but specifically for confidential data.

#### Secret Types:
- **Opaque**: Arbitrary user-defined data (default)
- **kubernetes.io/service-account-token**: Service account token
- **kubernetes.io/dockercfg**: Docker registry credentials
- **kubernetes.io/tls**: TLS certificate and key

---

## Security

### **RBAC (Role-Based Access Control)**
Method of regulating access to resources based on roles of individual users.

### **Role**
Namespace-scoped set of permissions.

### **ClusterRole**
Cluster-wide set of permissions.

### **RoleBinding**
Grants permissions defined in a Role to a user or set of users within a namespace.

### **ClusterRoleBinding**
Grants permissions defined in a ClusterRole cluster-wide.

### **ServiceAccount**
Provides an identity for processes running in a Pod.

### **SecurityContext**
Defines privilege and access control settings for a Pod or Container.

### **PodSecurityPolicy (PSP)**
Cluster-level resource that controls security-sensitive aspects of Pod specification. (Deprecated in favor of Pod Security Standards)

### **Pod Security Standards**
Modern replacement for PSP, defining three policies: Privileged, Baseline, and Restricted.

### **NetworkPolicy**
Controls traffic flow at the IP address or port level.

---

## Scheduling

### **Scheduler**
Control plane component that assigns Pods to nodes based on resource requirements and constraints.

### **Node Affinity**
Constrains which nodes a Pod can be scheduled on based on node labels.

### **Pod Affinity/Anti-Affinity**
Constrains which nodes a Pod can be scheduled on based on labels of Pods already running on the node.

### **Taint**
Marks a node so that no Pods can schedule onto it unless they have a matching toleration.

### **Toleration**
Allows (but does not require) Pods to schedule onto nodes with matching taints.

### **PriorityClass**
Defines the priority of Pods. Higher priority Pods can preempt lower priority ones.

### **ResourceQuota**
Provides constraints that limit aggregate resource consumption per namespace.

### **LimitRange**
Defines min/max resource usage per Pod or Container in a namespace.

---

## Scaling & Performance

### **HorizontalPodAutoscaler (HPA)**
Automatically scales the number of Pods based on observed CPU/memory utilization or custom metrics.

### **VerticalPodAutoscaler (VPA)**
Automatically adjusts CPU and memory requests/limits for containers.

### **Cluster Autoscaler**
Automatically adjusts the size of the cluster based on resource requirements.

### **PodDisruptionBudget (PDB)**
Limits the number of Pods that can be down simultaneously during voluntary disruptions.

---

## Probes & Health Checks

### **Liveness Probe**
Indicates whether the container is running. If fails, kubelet kills the container.

### **Readiness Probe**
Indicates whether the container is ready to serve requests. If fails, endpoints controller removes Pod from service endpoints.

### **Startup Probe**
Indicates whether the application has started. Disables liveness and readiness checks until it succeeds.

#### Probe Types:
- **exec**: Executes a command inside container
- **httpGet**: Performs HTTP GET request
- **tcpSocket**: Checks if TCP port is open
- **grpc**: Performs gRPC health check

---

## Resource Management

### **Resource Request**
Minimum amount of CPU/memory guaranteed to a container.

### **Resource Limit**
Maximum amount of CPU/memory a container can use.

### **QoS (Quality of Service) Classes**
- **Guaranteed**: Requests = Limits for all containers
- **Burstable**: At least one container has request or limit
- **BestEffort**: No requests or limits set

### **CPU**
Measured in cores. Can use fractional values (e.g., 0.5 = 500m = half a core).

### **Memory**
Measured in bytes. Can use suffixes: Ki, Mi, Gi, Ti, Pi, Ei (binary) or K, M, G, T, P, E (decimal).

---

## Advanced Concepts

### **Operator**
Method of packaging, deploying, and managing a Kubernetes application using custom resources.

### **CustomResourceDefinition (CRD)**
Extends Kubernetes API to create custom resources.

### **Admission Controller**
Piece of code that intercepts requests to the Kubernetes API before object persistence.

### **Webhook**
HTTP callback that receives admission requests and can mutate or validate objects.

### **Finalizer**
Key that tells Kubernetes to wait until specific conditions are met before fully deleting a resource.

### **OwnerReference**
Establishes parent-child relationships between resources for garbage collection.

---

## Deployment Strategies

### **Rolling Update**
Gradually replaces old Pods with new ones. Default strategy for Deployments.

### **Recreate**
Terminates all old Pods before creating new ones. Causes downtime.

### **Blue-Green Deployment**
Two identical environments; switch traffic from blue (old) to green (new) instantly.

### **Canary Deployment**
Gradually roll out changes to a small subset of users before full deployment.

### **A/B Testing**
Run two versions simultaneously and route traffic based on specific criteria.

---

## Observability

### **Metrics**
Numerical data about system performance (CPU, memory, request rate, etc.).

### **Logs**
Text records of events that happened in the system.

### **Traces**
Records of the path of requests through distributed systems.

### **Prometheus**
Open-source monitoring and alerting toolkit commonly used with Kubernetes.

### **Grafana**
Open-source analytics and visualization platform.

### **Fluentd/Fluent Bit**
Log collectors and processors.

### **Jaeger/Zipkin**
Distributed tracing systems.

---

## Container Runtime

### **Container Runtime**
Software responsible for running containers (e.g., containerd, CRI-O, Docker).

### **CRI (Container Runtime Interface)**
Plugin interface enabling kubelet to use different container runtimes.

### **OCI (Open Container Initiative)**
Industry standards for container formats and runtimes.

---

## GitOps & CI/CD

### **GitOps**
Operational model where Git is the single source of truth for declarative infrastructure and applications.

### **ArgoCD**
Declarative GitOps continuous delivery tool for Kubernetes.

### **Flux**
GitOps toolkit for keeping Kubernetes clusters in sync with configuration sources.

### **Helm**
Package manager for Kubernetes. Packages are called "charts."

### **Kustomize**
Tool for customizing Kubernetes configurations without templates.

---

## Abbreviations

- **K8s**: Kubernetes (K + 8 letters + s)
- **YAML**: YAML Ain't Markup Language
- **API**: Application Programming Interface
- **RBAC**: Role-Based Access Control
- **CNI**: Container Network Interface
- **CSI**: Container Storage Interface
- **CRI**: Container Runtime Interface
- **HPA**: HorizontalPodAutoscaler
- **VPA**: VerticalPodAutoscaler
- **PV**: PersistentVolume
- **PVC**: PersistentVolumeClaim
- **PDB**: PodDisruptionBudget
- **PSP**: PodSecurityPolicy
- **CRD**: CustomResourceDefinition
- **ETCD**: Distributed key-value store (control plane data)
- **LB**: Load Balancer
- **SVC**: Service

---

## Common Patterns

### **Sidecar Pattern**
Additional container in a Pod that enhances or extends the main container (e.g., log shipper, proxy).

### **Ambassador Pattern**
Sidecar that proxies network connections for the main container.

### **Adapter Pattern**
Sidecar that transforms output of the main container to match expected format.

### **Init Container Pattern**
Containers that run before app containers start, used for setup tasks.

---

## Best Practices Terms

### **Immutable Infrastructure**
Infrastructure that is never modified after deployment. Changes require new deployment.

### **Declarative Configuration**
Describe desired state; system figures out how to achieve it.

### **Imperative Configuration**
Specify exact commands to achieve desired state.

### **Infrastructure as Code (IaC)**
Managing infrastructure through code and version control.

### **Twelve-Factor App**
Methodology for building modern, scalable applications.

### **Zero-Downtime Deployment**
Deploying updates without service interruption.

### **Self-Healing**
System automatically detects and recovers from failures.

---

**Note**: This glossary covers terms used in this repository. For complete Kubernetes documentation, visit [kubernetes.io](https://kubernetes.io/docs/).
