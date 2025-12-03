# Kubernetes kubectl Cheat Sheet

## Table of Contents
- [Cluster Information](#cluster-information)
- [Resource Management](#resource-management)
- [Pods](#pods)
- [Deployments](#deployments)
- [Services](#services)
- [ConfigMaps & Secrets](#configmaps--secrets)
- [Namespaces](#namespaces)
- [Logs & Debugging](#logs--debugging)
- [Scaling & Autoscaling](#scaling--autoscaling)
- [Rollouts](#rollouts)
- [RBAC](#rbac)
- [Storage](#storage)
- [Networking](#networking)
- [Troubleshooting](#troubleshooting)

---

## Cluster Information

```bash
# Cluster info
kubectl cluster-info
kubectl cluster-info dump

# Version
kubectl version
kubectl version --short

# Nodes
kubectl get nodes
kubectl get nodes -o wide
kubectl describe node <node-name>
kubectl top nodes

# API resources
kubectl api-resources
kubectl api-versions
kubectl explain <resource>
kubectl explain pod.spec.containers
```

---

## Resource Management

```bash
# Get resources
kubectl get all
kubectl get all -A  # All namespaces
kubectl get <resource>
kubectl get <resource> -o wide
kubectl get <resource> -o yaml
kubectl get <resource> -o json
kubectl get <resource> --watch

# Describe resources
kubectl describe <resource> <name>
kubectl describe pod <pod-name>

# Create resources
kubectl create -f <file.yaml>
kubectl apply -f <file.yaml>
kubectl apply -f <directory>/
kubectl apply -k <kustomize-dir>

# Update resources
kubectl apply -f <file.yaml>
kubectl replace -f <file.yaml>
kubectl patch <resource> <name> -p '<patch>'
kubectl edit <resource> <name>

# Delete resources
kubectl delete <resource> <name>
kubectl delete -f <file.yaml>
kubectl delete <resource> --all
kubectl delete <resource> -l <label>=<value>

# Diff before apply
kubectl diff -f <file.yaml>
```

---

## Pods

```bash
# List pods
kubectl get pods
kubectl get pods -A
kubectl get pods -n <namespace>
kubectl get pods -o wide
kubectl get pods --show-labels
kubectl get pods -l <label>=<value>
kubectl get pods --field-selector status.phase=Running

# Describe pod
kubectl describe pod <pod-name>

# Create pod
kubectl run <pod-name> --image=<image>
kubectl run <pod-name> --image=<image> --dry-run=client -o yaml > pod.yaml

# Delete pod
kubectl delete pod <pod-name>
kubectl delete pod <pod-name> --force --grace-period=0

# Execute commands
kubectl exec <pod-name> -- <command>
kubectl exec -it <pod-name> -- /bin/sh
kubectl exec -it <pod-name> -c <container-name> -- /bin/bash

# Copy files
kubectl cp <pod-name>:/path/to/file /local/path
kubectl cp /local/path <pod-name>:/path/to/file

# Port forwarding
kubectl port-forward <pod-name> <local-port>:<pod-port>
kubectl port-forward <pod-name> 8080:80

# Attach to pod
kubectl attach <pod-name> -it

# Top (resource usage)
kubectl top pod <pod-name>
kubectl top pods -A
```

---

## Deployments

```bash
# List deployments
kubectl get deployments
kubectl get deploy -A

# Create deployment
kubectl create deployment <name> --image=<image>
kubectl create deployment <name> --image=<image> --replicas=3
kubectl create deployment <name> --image=<image> --dry-run=client -o yaml > deploy.yaml

# Describe deployment
kubectl describe deployment <name>

# Update deployment
kubectl set image deployment/<name> <container>=<new-image>
kubectl set resources deployment/<name> -c=<container> --limits=cpu=200m,memory=512Mi

# Scale deployment
kubectl scale deployment/<name> --replicas=5

# Autoscale deployment
kubectl autoscale deployment/<name> --min=2 --max=10 --cpu-percent=80

# Delete deployment
kubectl delete deployment <name>

# Deployment status
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
kubectl rollout undo deployment/<name> --to-revision=2
kubectl rollout pause deployment/<name>
kubectl rollout resume deployment/<name>
kubectl rollout restart deployment/<name>
```

---

## Services

```bash
# List services
kubectl get services
kubectl get svc -A

# Create service
kubectl create service clusterip <name> --tcp=80:8080
kubectl expose deployment <name> --port=80 --target-port=8080
kubectl expose deployment <name> --type=LoadBalancer --port=80

# Describe service
kubectl describe service <name>

# Delete service
kubectl delete service <name>

# Get service endpoints
kubectl get endpoints <service-name>

# Service types
kubectl create service clusterip <name> --tcp=80:8080
kubectl create service nodeport <name> --tcp=80:8080
kubectl create service loadbalancer <name> --tcp=80:8080
```

---

## ConfigMaps & Secrets

```bash
# ConfigMaps
kubectl create configmap <name> --from-literal=key=value
kubectl create configmap <name> --from-file=<file>
kubectl create configmap <name> --from-file=<directory>
kubectl get configmaps
kubectl describe configmap <name>
kubectl edit configmap <name>
kubectl delete configmap <name>

# Secrets
kubectl create secret generic <name> --from-literal=key=value
kubectl create secret generic <name> --from-file=<file>
kubectl create secret docker-registry <name> --docker-server=<server> --docker-username=<user> --docker-password=<pass>
kubectl create secret tls <name> --cert=<cert-file> --key=<key-file>
kubectl get secrets
kubectl describe secret <name>
kubectl get secret <name> -o yaml
kubectl delete secret <name>

# Decode secret
kubectl get secret <name> -o jsonpath='{.data.key}' | base64 --decode
```

---

## Namespaces

```bash
# List namespaces
kubectl get namespaces
kubectl get ns

# Create namespace
kubectl create namespace <name>

# Delete namespace
kubectl delete namespace <name>

# Set default namespace
kubectl config set-context --current --namespace=<name>

# Get resources in namespace
kubectl get all -n <namespace>

# Get resources in all namespaces
kubectl get pods -A
kubectl get all -A
```

---

## Logs & Debugging

```bash
# View logs
kubectl logs <pod-name>
kubectl logs <pod-name> -c <container-name>
kubectl logs <pod-name> --previous
kubectl logs -f <pod-name>  # Follow logs
kubectl logs <pod-name> --tail=100
kubectl logs <pod-name> --since=1h
kubectl logs -l <label>=<value>

# Events
kubectl get events
kubectl get events -A
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get events --field-selector type=Warning

# Describe for debugging
kubectl describe pod <pod-name>
kubectl describe node <node-name>

# Resource usage
kubectl top nodes
kubectl top pods
kubectl top pods -A
kubectl top pod <pod-name> --containers

# Debug pod
kubectl debug <pod-name> -it --image=busybox
kubectl debug node/<node-name> -it --image=ubuntu

# Run temporary pod
kubectl run tmp --rm -it --image=busybox -- /bin/sh
kubectl run tmp --rm -it --image=nicolaka/netshoot -- /bin/bash
```

---

## Scaling & Autoscaling

```bash
# Manual scaling
kubectl scale deployment/<name> --replicas=5
kubectl scale statefulset/<name> --replicas=3
kubectl scale replicaset/<name> --replicas=2

# Horizontal Pod Autoscaler (HPA)
kubectl autoscale deployment/<name> --min=2 --max=10 --cpu-percent=80
kubectl get hpa
kubectl describe hpa <name>
kubectl delete hpa <name>

# Vertical Pod Autoscaler (VPA)
kubectl get vpa
kubectl describe vpa <name>
```

---

## Rollouts

```bash
# Rollout status
kubectl rollout status deployment/<name>
kubectl rollout status statefulset/<name>
kubectl rollout status daemonset/<name>

# Rollout history
kubectl rollout history deployment/<name>
kubectl rollout history deployment/<name> --revision=2

# Rollback
kubectl rollout undo deployment/<name>
kubectl rollout undo deployment/<name> --to-revision=2

# Pause/Resume
kubectl rollout pause deployment/<name>
kubectl rollout resume deployment/<name>

# Restart
kubectl rollout restart deployment/<name>
```

---

## RBAC

```bash
# Check permissions
kubectl auth can-i create deployments
kubectl auth can-i create deployments --namespace=production
kubectl auth can-i '*' '*'  # Check if cluster admin
kubectl auth can-i --list

# Check as another user
kubectl auth can-i create deployments --as=user@example.com
kubectl auth can-i create deployments --as=system:serviceaccount:default:myapp

# Roles and RoleBindings
kubectl get roles -A
kubectl get rolebindings -A
kubectl get clusterroles
kubectl get clusterrolebindings

# Describe RBAC
kubectl describe role <name>
kubectl describe rolebinding <name>
kubectl describe clusterrole <name>
kubectl describe clusterrolebinding <name>

# Create role
kubectl create role <name> --verb=get,list --resource=pods
kubectl create clusterrole <name> --verb=get,list --resource=pods

# Create rolebinding
kubectl create rolebinding <name> --role=<role> --user=<user>
kubectl create clusterrolebinding <name> --clusterrole=<role> --user=<user>

# Service accounts
kubectl get serviceaccounts
kubectl create serviceaccount <name>
kubectl describe serviceaccount <name>
```

---

## Storage

```bash
# PersistentVolumes
kubectl get pv
kubectl describe pv <name>
kubectl delete pv <name>

# PersistentVolumeClaims
kubectl get pvc
kubectl get pvc -A
kubectl describe pvc <name>
kubectl delete pvc <name>

# StorageClasses
kubectl get storageclass
kubectl get sc
kubectl describe sc <name>

# Volume Snapshots
kubectl get volumesnapshot
kubectl describe volumesnapshot <name>
```

---

## Networking

```bash
# Services
kubectl get services
kubectl get endpoints

# Ingress
kubectl get ingress
kubectl get ingress -A
kubectl describe ingress <name>

# Network Policies
kubectl get networkpolicies
kubectl get netpol -A
kubectl describe networkpolicy <name>

# DNS debugging
kubectl run tmp --rm -it --image=busybox -- nslookup <service-name>
kubectl run tmp --rm -it --image=nicolaka/netshoot -- nslookup <service-name>

# Test connectivity
kubectl run tmp --rm -it --image=nicolaka/netshoot -- curl <service-name>
kubectl run tmp --rm -it --image=nicolaka/netshoot -- ping <service-name>
```

---

## Troubleshooting

```bash
# Pod not starting
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get events --field-selector involvedObject.name=<pod-name>

# Image pull errors
kubectl describe pod <pod-name> | grep -A 10 Events
kubectl get events --field-selector reason=Failed

# Resource constraints
kubectl top nodes
kubectl top pods -A
kubectl describe node <node-name>

# Service not accessible
kubectl get endpoints <service-name>
kubectl describe service <service-name>
kubectl get pods --show-labels

# DNS issues
kubectl run tmp --rm -it --image=busybox -- nslookup kubernetes.default
kubectl get pods -n kube-system -l k8s-app=kube-dns

# Network issues
kubectl run tmp --rm -it --image=nicolaka/netshoot -- /bin/bash
# Then inside: ping, curl, nslookup, traceroute, etc.

# Check cluster health
kubectl get componentstatuses
kubectl get nodes
kubectl get pods -n kube-system

# Drain node (for maintenance)
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data
kubectl uncordon <node-name>

# Cordon node (prevent scheduling)
kubectl cordon <node-name>
kubectl uncordon <node-name>

# Taint node
kubectl taint nodes <node-name> key=value:NoSchedule
kubectl taint nodes <node-name> key=value:NoSchedule-  # Remove taint
```

---

## Useful Aliases

Add to your `~/.bashrc` or `~/.zshrc`:

```bash
alias k='kubectl'
alias kg='kubectl get'
alias kd='kubectl describe'
alias kdel='kubectl delete'
alias kl='kubectl logs'
alias kex='kubectl exec -it'
alias kaf='kubectl apply -f'
alias kdf='kubectl delete -f'
alias kgp='kubectl get pods'
alias kgd='kubectl get deployments'
alias kgs='kubectl get services'
alias kgn='kubectl get nodes'
alias kga='kubectl get all'
alias kgaa='kubectl get all -A'
alias kctx='kubectl config use-context'
alias kns='kubectl config set-context --current --namespace'

# With watch
alias kgpw='watch kubectl get pods'
alias kgdw='watch kubectl get deployments'
```

---

## Kubectl Configuration

```bash
# View config
kubectl config view
kubectl config view --minify

# Contexts
kubectl config get-contexts
kubectl config current-context
kubectl config use-context <context-name>

# Set namespace
kubectl config set-context --current --namespace=<namespace>

# Clusters
kubectl config get-clusters

# Users
kubectl config get-users

# Set credentials
kubectl config set-credentials <user> --token=<token>
kubectl config set-credentials <user> --client-certificate=<cert> --client-key=<key>
```

---

## JSON Path Queries

```bash
# Get specific field
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
kubectl get pods -o jsonpath='{.items[*].spec.containers[*].image}'

# Get node IPs
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}'

# Get pod IPs
kubectl get pods -o jsonpath='{.items[*].status.podIP}'

# Custom columns
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase,IP:.status.podIP
```

---

## Tips & Tricks

```bash
# Dry run (test without applying)
kubectl apply -f file.yaml --dry-run=client
kubectl apply -f file.yaml --dry-run=server

# Generate YAML
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > deployment.yaml
kubectl expose deployment nginx --port=80 --dry-run=client -o yaml > service.yaml

# Watch resources
kubectl get pods --watch
kubectl get pods -w

# Sort by creation time
kubectl get pods --sort-by=.metadata.creationTimestamp

# Filter by field
kubectl get pods --field-selector status.phase=Running
kubectl get pods --field-selector metadata.namespace=default

# Multiple resources
kubectl get pods,services,deployments

# Wait for condition
kubectl wait --for=condition=ready pod -l app=myapp --timeout=300s
kubectl wait --for=delete pod/<pod-name> --timeout=60s

# Explain resources
kubectl explain pod
kubectl explain pod.spec
kubectl explain pod.spec.containers

# Shell completion
source <(kubectl completion bash)  # Bash
source <(kubectl completion zsh)   # Zsh
```

---

**Pro Tip**: Use `kubectl` with `-o yaml` or `-o json` to see the full resource definition, which is great for learning and debugging!
