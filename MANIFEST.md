# Example Manifest

This document lists all available examples in the repository for quick reference.

## 01. Containers
- `health-probes.yaml`: Liveness, Readiness, and Startup probes
- `multi-container-pod.yaml`: Sidecar pattern implementation

## 02. Workloads
- `deployment/production-deployment.yaml`: Full deployment with HPA, PDB, and security context
- `statefulset/postgres-statefulset.yaml`: HA PostgreSQL with persistent storage
- `daemonset/daemonset-examples.yaml`: Fluentd, Node Exporter, Calico, Falco, NVIDIA plugin
- `cronjob/cronjob-examples.yaml`: Database backup, log cleanup, cert check, metrics aggregation
- `job/job-examples.yaml`: Simple job, indexed job (work queue), time-limited job

## 03. Services & Networking
- `service/service-examples.yaml`: ClusterIP, NodePort, LoadBalancer, ExternalName, Headless
- `ingress/ingress-nginx.yaml`: NGINX Ingress with TLS, rate limiting, CORS
- `network-policy/network-policies.yaml`: Zero-trust policies, tier isolation, egress control

## 04. Storage
- `persistent-volume/pv-examples.yaml`: Local, NFS, and HostPath PVs
- `storage-class/sc-examples.yaml`: StorageClasses for AWS, GCP, Azure, and Local
- `storage-examples.yaml`: Comprehensive guide to PV, PVC, Snapshots, and ConfigMap/Secret volumes

## 05. Configuration
- `configmap/configmap-examples.yaml`: Key-value, file embedding, JSON config
- `secrets/secret-examples.yaml`: Opaque, TLS, Docker Registry, StringData
- `hpa/hpa-examples.yaml`: CPU, Memory, Custom Metric, and Behavior control scaling

## 06. Security
- `rbac/rbac-examples.yaml`: Developer, CI/CD, Monitoring, and Admin roles
- `security-context/security-context-examples.yaml`: Pod and Container level security contexts, Seccomp
- `pod-security/pod-security-examples.yaml`: Pod Security Standards (Restricted, Baseline, Privileged)

## 07. Policies
- `resource-quota/resource-quota-examples.yaml`: Compute, Object count, and Storage quotas
- `limit-range/limit-range-examples.yaml`: Default limits, Min/Max constraints
- `pdb/pdb-examples.yaml`: MinAvailable and MaxUnavailable budgets

## 08. Scheduling
- `affinity/affinity-examples.yaml`: Node Affinity, Pod Affinity/Anti-Affinity
- `taints-tolerations/taints-tolerations-examples.yaml`: Taints, Tolerations, Eviction timeouts
- `priority/priority-examples.yaml`: PriorityClasses and usage

## 09. Cluster Admin
- `namespace/namespace-examples.yaml`: Namespaces with labels and security standards
- `service-account/service-account-examples.yaml`: Image pull secrets, Workload Identity

## 10. Real World Scenarios
- `microservices/complete-ecommerce-app.yaml`: Full stack e-commerce app
- `monitoring/prometheus-grafana-stack.yaml`: Complete monitoring stack
- `database/redis-cluster.yaml`: 6-node Redis Cluster with bootstrapping
