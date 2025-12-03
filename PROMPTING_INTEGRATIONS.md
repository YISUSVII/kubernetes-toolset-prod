# Prompting Integrations

This document contains a collection of prompts you can use with an AI agent to generate specific Kubernetes configurations based on the templates provided in this repository.

## How to Use
Copy the **Prompt** text and paste it into your chat with the AI agent. The agent will use the referenced files from this repository to generate the customized YAML you need.

---

## 1. CronJob Running a Python Script
**Goal**: Schedule a Python script to run periodically.
**Prompt**:
> Using the `02-workloads/cronjob/cronjob-examples.yaml` file as a reference, create a Kubernetes CronJob named `daily-report-generator`. It should run a Python script `generate_report.py` every day at midnight (0 0 * * *). Use the image `python:3.9-slim` and ensure it restarts on failure.

## 2. Java Backend Service
**Goal**: Deploy a Java Spring Boot application with a Service.
**Prompt**:
> Based on `02-workloads/deployment/production-deployment.yaml` and `03-services-networking/service/service-examples.yaml`, create a Deployment and a ClusterIP Service for a Java Spring Boot application. Name the deployment `java-backend`, use image `my-java-app:v1`, expose port 8080, and include liveness and readiness probes pointing to `/actuator/health`.

## 3. Next.js Frontend App
**Goal**: Deploy a Next.js application exposed to the internet.
**Prompt**:
> Create a deployment for a Next.js application using `02-workloads/deployment/production-deployment.yaml` as a base. Name it `nextjs-frontend`, use image `my-nextjs-app:latest`, and expose port 3000. Also, generate an Ingress resource based on `03-services-networking/ingress/ingress-nginx.yaml` to expose it at `example.com`.

## 4. Redis with Password Authentication
**Goal**: Deploy Redis with a password stored in a Secret.
**Prompt**:
> I need a Redis deployment. Use `05-configuration/secrets/secret-examples.yaml` to create a Secret for the Redis password. Then, using `02-workloads/deployment/basic-deployment.yaml`, create a Redis deployment that mounts this secret and sets the `REDIS_PASSWORD` environment variable.

## 5. PostgreSQL with Persistent Storage
**Goal**: Deploy a stateful PostgreSQL database.
**Prompt**:
> Using `02-workloads/statefulset/postgres-statefulset.yaml` and `04-storage/persistent-volume/pv-examples.yaml`, generate a StatefulSet for PostgreSQL 14. It should request a 10Gi PersistentVolumeClaim and use a ConfigMap for `POSTGRES_DB` and `POSTGRES_USER` configuration.

## 6. Secure Ingress with TLS
**Goal**: Expose a service with HTTPS using Let's Encrypt.
**Prompt**:
> Generate an Ingress resource based on `03-services-networking/ingress/ingress-nginx.yaml`. It should route traffic for `secure-api.com` to a service named `api-service` on port 80. Include TLS configuration referencing a secret named `api-tls-cert` and add annotations for cert-manager.

## 7. Auto-scaling Microservice
**Goal**: Configure HPA for a high-traffic service.
**Prompt**:
> I have a deployment named `payment-service`. Using `05-configuration/hpa/hpa-examples.yaml`, create a HorizontalPodAutoscaler that scales this deployment between 3 and 15 replicas based on 70% CPU utilization and 80% Memory utilization.

## 8. Logging DaemonSet
**Goal**: Deploy a log collector on every node.
**Prompt**:
> Create a DaemonSet for Fluentd using `02-workloads/daemonset/daemonset-examples.yaml` as a template. It needs to mount the host path `/var/log` to `/var/log` in the container to collect node logs.

## 9. Database Migration Job
**Goal**: Run a one-off schema migration script.
**Prompt**:
> Using `02-workloads/job/job-examples.yaml`, create a Job named `db-migration-v2`. It should run the command `npm run migrate` using the image `my-node-app:v2`. Ensure the `restartPolicy` is set to `Never` and it has a backoff limit of 4.

## 10. CI/CD Pipeline Permissions (RBAC)
**Goal**: Create a ServiceAccount for a CI/CD tool with specific permissions.
**Prompt**:
> I need to set up a CI/CD user. Using `06-security/rbac/rbac-examples.yaml`, create a ServiceAccount named `cicd-bot` and a RoleBinding that grants it the `edit` ClusterRole within the `staging` namespace only.

## 11. Multi-container Pod (Sidecar)
**Goal**: Add a proxy sidecar to an application.
**Prompt**:
> Take the `01-containers/multi-container-pod.yaml` example and adapt it. Create a Pod named `web-with-proxy` that has a main container running `nginx` and a sidecar container running `envoy`. The sidecar should share the same network namespace.

## 12. Database Network Isolation
**Goal**: Restrict access to a database to only specific pods.
**Prompt**:
> Using `03-services-networking/network-policy/network-policies.yaml`, create a NetworkPolicy named `db-access-policy`. It should allow ingress traffic to pods labeled `app: postgres` ONLY from pods labeled `app: backend`. Deny all other traffic.

## 13. ConfigMap for Environment Variables
**Goal**: Externalize application configuration.
**Prompt**:
> Create a ConfigMap based on `05-configuration/configmap/configmap-examples.yaml` named `app-config`. It should contain keys for `API_URL`, `FEATURE_FLAG_ENABLED`, and `LOG_LEVEL`. Then show me how to inject these as environment variables into a Deployment based on `02-workloads/deployment/basic-deployment.yaml`.


## 14. Feel free to contribute with more prompts for your prod examples

