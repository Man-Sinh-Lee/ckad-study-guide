Chapter 13 on API Deprecations from the *Certified Kubernetes Application Developer (CKAD) Study Guide*, covering essential practices for managing deprecated APIs in Kubernetes.

### Understanding the Deprecation Policy
- Kubernetes releases multiple versions annually, often introducing new APIs, features, and security updates. A version may also deprecate certain APIs, meaning they are scheduled for future removal or replacement.
- The **Deprecation Policy** ensures that deprecated APIs remain available for a minimum of three releases before removal, allowing time to update applications and manifests.
- Deprecation notices display during the use of deprecated APIs, warning users to update their configurations.

### Listing Available API Versions
- Use `kubectl api-versions` to list available API versions for all Kubernetes resources. This command displays available API groups in the format `group/version`, such as `apps/v1` or `autoscaling/v2`.
  ```bash
  kubectl api-versions
  ```
- Kubernetes documentation should be consulted to verify the deprecation status of specific API versions and plan upgrades.

### Handling Deprecation Warnings
- Deprecation warnings appear when applying YAML files using deprecated APIs. For instance, creating a HorizontalPodAutoscaler with `autoscaling/v2beta2` will generate a warning, suggesting migration to `autoscaling/v2`:
  ```yaml
  apiVersion: autoscaling/v2beta2
  kind: HorizontalPodAutoscaler
  metadata:
    name: php-apache
  spec:
    scaleTargetRef:
      apiVersion: apps/v1
      kind: Deployment
      name: php-apache
    minReplicas: 1
    maxReplicas: 10
  ```
  ```bash
  kubectl apply -f hpa.yaml
  ```
  Warning output suggests the use of `autoscaling/v2` starting from Kubernetes 1.26.

### Handling a Removed or Replaced API
- For APIs fully removed in newer Kubernetes versions, `kubectl apply` returns an error rather than a warning. For example, the API version `rbac.authorization.k8s.io/v1beta1` for ClusterRole has been removed in Kubernetes 1.22, replaced by `rbac.authorization.k8s.io/v1`.
  - Example:
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1beta1
    kind: ClusterRole
    metadata:
      name: pod-reader
    rules:
    - apiGroups: [""]
      resources: ["pods"]
      verbs: ["get", "watch", "list"]
    ```
    Attempting to apply this manifest in Kubernetes 1.22+ results in an error. Updating `apiVersion` to `rbac.authorization.k8s.io/v1` resolves the issue.

### Summary and Exam Essentials
- **Summary**: Developers must be prepared to handle deprecation warnings and consult migration guides when upgrading Kubernetes versions. Understanding deprecated and replaced APIs ensures stability and reduces issues during updates.
- **Exam Essentials**: Familiarize yourself with tools like `kubectl api-versions` and the Kubernetes Deprecated API Migration Guide to address deprecated APIs in exam scenarios.

### Sample Exercises
1. **API Migration**: Update an older YAML manifest using deprecated APIs to the latest supported versions and validate compatibility.
2. **Error Resolution**: Diagnose and correct a manifest that fails due to an obsolete API version by identifying and updating the version.