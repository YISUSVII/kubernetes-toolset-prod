# Kubernetes Toolset

## Prerequisites

Before using these examples, ensure you have:

1. **Kubernetes Cluster** (v1.24+)
   - Minikube (local development)
   - Kind (Kubernetes in Docker)
   - Cloud provider (GKE, EKS, AKS)
   - On-premise cluster

2. **kubectl CLI**
   ```bash
   # Check version
   kubectl version --client
   
   # Should be v1.24 or higher
   ```

3. **Cluster Access**
   ```bash
   # Verify cluster connection
   kubectl cluster-info
   kubectl get nodes
   ```

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/kubernetes-toolset-prod.git
cd kubernetes-toolset-prod
```

### 2. Choose Your Learning Path

#### **Beginner Path**
Start here if you're new to Kubernetes:

```bash
# 1. Understand containers
cd 01-containers
kubectl apply -f health-probes.yaml
kubectl get pods
kubectl describe pod webapp-with-probes

# 2. Deploy your first application
cd ../02-workloads/deployment
kubectl apply -f production-deployment.yaml
kubectl get deployments
kubectl get pods

# 3. Expose your application
cd ../../03-services-networking/service
kubectl apply -f service-types.yaml
kubectl get services
```

#### **Intermediate Path**
For those familiar with basics:

```bash
# 1. StatefulSets for databases
cd 02-workloads/statefulset
kubectl apply -f postgres-statefulset.yaml

# 2. Persistent storage
cd ../../04-storage
kubectl apply -f storage-examples.yaml

# 3. Security with RBAC
cd ../06-security/rbac
kubectl apply -f rbac-examples.yaml
```

#### **Advanced Path**
Production-ready configurations:

```bash
# 1. Complete microservices application
cd 10-real-world-scenarios/microservices
kubectl apply -f complete-ecommerce-app.yaml

# 2. Network policies
cd ../../03-services-networking/network-policy
kubectl apply -f network-policies.yaml

# 3. Monitoring stack
cd ../../10-real-world-scenarios/monitoring
kubectl apply -f prometheus-stack.yaml
```

## Common Workflows

### Deploy an Application

```bash
# 1. Create namespace
kubectl create namespace myapp

# 2. Apply configurations
kubectl apply -f configmap.yaml -n myapp
kubectl apply -f secrets.yaml -n myapp
kubectl apply -f deployment.yaml -n myapp
kubectl apply -f service.yaml -n myapp
kubectl apply -f ingress.yaml -n myapp

# 3. Verify deployment
kubectl get all -n myapp
kubectl rollout status deployment/myapp -n myapp
```

### Update an Application

```bash
# Method 1: Update image
kubectl set image deployment/myapp app=myapp:v2 -n myapp

# Method 2: Edit deployment
kubectl edit deployment myapp -n myapp

# Method 3: Apply updated YAML
kubectl apply -f deployment.yaml -n myapp

# Watch rollout
kubectl rollout status deployment/myapp -n myapp

# Rollback if needed
kubectl rollout undo deployment/myapp -n myapp
```

### Debug Issues

```bash
# Check pod status
kubectl get pods -n myapp
kubectl describe pod <pod-name> -n myapp

# View logs
kubectl logs <pod-name> -n myapp
kubectl logs <pod-name> -n myapp --previous  # Previous container
kubectl logs <pod-name> -c <container-name> -n myapp  # Specific container

# Execute commands in pod
kubectl exec -it <pod-name> -n myapp -- /bin/sh

# Check events
kubectl get events -n myapp --sort-by=.metadata.creationTimestamp

# Check resource usage
kubectl top pods -n myapp
kubectl top nodes
```

### Scale Applications

```bash
# Manual scaling
kubectl scale deployment/myapp --replicas=5 -n myapp

# Auto-scaling
kubectl autoscale deployment/myapp --min=2 --max=10 --cpu-percent=70 -n myapp

# Check HPA status
kubectl get hpa -n myapp
```

## Testing Examples

### Test a Simple Deployment

```bash
# Apply the example
kubectl apply -f 02-workloads/deployment/production-deployment.yaml

# Wait for pods to be ready
kubectl wait --for=condition=ready pod -l app=webapp -n production --timeout=300s

# Check status
kubectl get pods -n production -l app=webapp
kubectl get deployment webapp-production -n production

# Test the service
kubectl port-forward -n production svc/webapp-service 8080:80

# In another terminal
curl http://localhost:8080

# Cleanup
kubectl delete -f 02-workloads/deployment/production-deployment.yaml
```

### Test Storage

```bash
# Create storage class and PVC
kubectl apply -f 04-storage/storage-examples.yaml

# Check PVC status
kubectl get pvc -n production

# Create pod using PVC
kubectl apply -f 04-storage/pod-with-pvc.yaml

# Verify volume is mounted
kubectl exec -it <pod-name> -n production -- df -h

# Cleanup
kubectl delete -f 04-storage/pod-with-pvc.yaml
kubectl delete -f 04-storage/storage-examples.yaml
```

## Environment Setup

### Local Development (Minikube)

```bash
# Start Minikube
minikube start --cpus=4 --memory=8192 --disk-size=50g

# Enable addons
minikube addons enable ingress
minikube addons enable metrics-server
minikube addons enable dashboard

# Access dashboard
minikube dashboard
```

### Local Development (Kind)

```bash
# Create cluster
cat <<EOF | kind create cluster --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
- role: worker
- role: worker
EOF

# Install ingress controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Useful Commands Cheat Sheet

### Cluster Information
```bash
kubectl cluster-info
kubectl get nodes
kubectl get namespaces
kubectl api-resources
kubectl api-versions
```

### Resource Management
```bash
# Get resources
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get all -A  # All resources in all namespaces

# Describe resources
kubectl describe pod <name>
kubectl describe deployment <name>

# Delete resources
kubectl delete pod <name>
kubectl delete deployment <name>
kubectl delete -f file.yaml
```

### Logs and Debugging
```bash
kubectl logs <pod-name>
kubectl logs -f <pod-name>  # Follow logs
kubectl logs <pod-name> --previous
kubectl exec -it <pod-name> -- /bin/sh
kubectl port-forward <pod-name> 8080:80
kubectl top pods
kubectl top nodes
```

### Configuration
```bash
kubectl apply -f file.yaml
kubectl create -f file.yaml
kubectl replace -f file.yaml
kubectl delete -f file.yaml
kubectl diff -f file.yaml
```

### Context and Namespace
```bash
kubectl config get-contexts
kubectl config use-context <context-name>
kubectl config set-context --current --namespace=<namespace>
```

## Next Steps

1. **Explore Examples**: Browse through each category and try the examples
2. **Modify and Experiment**: Change values and see what happens
3. **Read Documentation**: Each directory has detailed README files
4. **Build Your Own**: Use these as templates for your applications
5. **Contribute**: Share your own examples and improvements

## Getting Help

- **Documentation**: Check README files in each directory
- **Kubernetes Docs**: https://kubernetes.io/docs/
- **kubectl Cheat Sheet**: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
- **Community**: Join Kubernetes Slack or forums

## Common Issues

### Pods Not Starting
```bash
# Check pod status
kubectl describe pod <pod-name>

# Common causes:
# - Image pull errors (check image name and registry credentials)
# - Resource constraints (check node resources)
# - Configuration errors (check ConfigMaps and Secrets)
```

### Service Not Accessible
```bash
# Check service endpoints
kubectl get endpoints <service-name>

# Verify pod labels match service selector
kubectl get pods --show-labels
kubectl describe service <service-name>
```

### Storage Issues
```bash
# Check PVC status
kubectl get pvc

# Check PV status
kubectl get pv

# Describe for details
kubectl describe pvc <pvc-name>
```

---

**Happy Learning!**

Remember: The best way to learn Kubernetes is by doing. Don't be afraid to break things in your development environment!
