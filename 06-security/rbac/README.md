# RBAC (Role-Based Access Control)

## Overview

RBAC regulates access to Kubernetes resources based on roles assigned to users, groups, or service accounts.

## Core Components

### 1. **Role / ClusterRole**
- **Role**: Namespace-scoped permissions
- **ClusterRole**: Cluster-wide permissions

### 2. **RoleBinding / ClusterRoleBinding**
- **RoleBinding**: Grants Role permissions in a namespace
- **ClusterRoleBinding**: Grants ClusterRole permissions cluster-wide

### 3. **ServiceAccount**
- Identity for pods
- Used by applications running in cluster

## Permission Model

```
Subject (Who) → Binding → Role (What) → Resources (Where)
```

### Subjects
- **User**: Human users
- **Group**: Collection of users
- **ServiceAccount**: Pod identity

### Verbs (Actions)
- `get`, `list`, `watch` - Read operations
- `create` - Create resources
- `update`, `patch` - Modify resources
- `delete`, `deletecollection` - Remove resources
- `*` - All verbs (use sparingly!)

## Best Practices

- **Principle of Least Privilege**
- Grant minimum permissions needed
- Avoid cluster-admin unless necessary

- **Use ServiceAccounts for Pods**
- Don't use default service account
- Create specific accounts per application

- **Namespace Isolation**
- Use Roles instead of ClusterRoles when possible
- Separate environments with namespaces

- **Regular Audits**
- Review permissions periodically
- Remove unused accounts and bindings

- **Avoid Wildcards**
- Be specific with resources and verbs
- Wildcards (`*`) are security risks

## Common Patterns

### Developer Access
- Read-only to production
- Full access to development namespace

### CI/CD Pipeline
- Create/update deployments
- Read secrets (specific ones)
- No delete permissions

### Monitoring
- Read-only cluster-wide
- Access to metrics endpoints

### Application
- Read ConfigMaps/Secrets
- Update own resources
- No access to other namespaces

## Examples Included

1. `developer-role.yaml` - Developer access patterns
2. `cicd-role.yaml` - CI/CD pipeline permissions
3. `monitoring-role.yaml` - Monitoring system access
4. `app-service-account.yaml` - Application service accounts
5. `security-audit.yaml` - Audit and compliance roles

## Troubleshooting

### Check permissions
```bash
# Check if you can perform action
kubectl auth can-i create deployments

# Check for specific user
kubectl auth can-i create deployments --as=user@example.com

# Check in namespace
kubectl auth can-i create deployments --namespace=production

# List all permissions for service account
kubectl auth can-i --list --as=system:serviceaccount:default:myapp
```

### Debug RBAC
```bash
# View roles
kubectl get roles -A
kubectl get clusterroles

# View bindings
kubectl get rolebindings -A
kubectl get clusterrolebindings

# Describe for details
kubectl describe role developer -n development
kubectl describe rolebinding developer-binding -n development
```

## Security Considerations

**Dangerous Permissions**
- `*` on verbs or resources
- `escalate` and `bind` on roles
- `impersonate` on users/groups
- `create` on pods (can mount any secret)
- `get/list` on secrets cluster-wide

**Privilege Escalation Risks**
- Creating pods with privileged service accounts
- Binding roles you don't have
- Impersonating users with more permissions
