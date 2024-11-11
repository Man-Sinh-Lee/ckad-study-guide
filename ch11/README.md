Chapter 11 on Deployment Strategies from the *Certified Kubernetes Application Developer (CKAD) Study Guide*.

### Rolling Deployment Strategy

- **Implementation**: Rolling updates are the default deployment strategy in Kubernetes, gradually replacing old Pods with new ones. Configured with `RollingUpdate` strategy, this approach allows the number of Pods to be controlled with:
  - `maxUnavailable`: Maximum Pods that can be unavailable during updates.
  - `maxSurge`: Maximum additional Pods allowed above the desired count during updates.
  - Example YAML:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    spec:
      strategy:
        type: RollingUpdate
        rollingUpdate:
          maxUnavailable: 1
          maxSurge: 2
    ```

- **Use Cases and Trade-Offs**: Ideal for zero-downtime updates, it’s useful for production where gradual transitions minimize risk. However, rolling updates may extend update times depending on the number of replicas and can create mixed versions during the transitionxed Deployment Strategy

- **Implementation**: The fixed (or recreate) strategy involves stopping all old version Pods before starting new ones, creating a “break and replace” model. This is done by setting the strategy type to `Recreate`.
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  spec:
    strategy:
      type: Recreate
  ```

- **Use Cases and Trade-Offs**: Suitable for development environments or cases where downtime is acceptable. It can cause service interruptions, so it’s less suited for production unless downtime can be managed effectively .

### Beployment Strategy

- **Implementation**: Blue-Green deployments operate two environments in parallel (e.g., blue for the old version, green for the new version). The traffic is routed to the green (new) version once it is ready.
  - Requires two separate Deployments (one for each version), and traffic routing can be controlled by updating Service labels.
  - Example YAML for blue deployment:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: app-blue
    spec:
      replicas: 4
      template:
        metadata:
          labels:
            app: blue
    ```

- **Use Cases and Trade-Offs**: Provides a rollback option by retaining the blue environment. However, it requires double resources (for blue and green environments), making it resource-intensive .

### Canary Depltegy

- **Implementation**: The canary strategy gradually introduces the new version to a small subset of users before rolling out more widely. A small number of replicas for the new version is created alongside the existing version.
  - To implement, create a separate Deployment with fewer replicas for the canary version, sharing the same Service label as the main Deployment.
  - Example of reducing the number of replicas for the canary version:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    spec:
      replicas: 1
      template:
        metadata:
          labels:
            app: canary
    ```

- **Use Cases and Trade-Offs**: Canary deployments are ideal for testing features or changes with limited users before full deployment. They consume fewer resources than blue-green but can increase complexity, especially in managing traffic proportions .

### Summary

Each deploymgy has unique use cases, balancing between resources, risks, and complexity. Kubernetes provides native support for rolling and fixed strategies, while blue-green and canary approaches require additional configuration.

### Exam Essentials

- Understand how to configure and implement each deployment strategy, especially using Kubernetes primitives like Deployments and Services.
- Be prepared to adjust strategies depending on requirements, downtime tolerance, and the need for rapid rollbacks.

### Sample Exercises

1. **Rolling Update Exercise**: Create a rolling update with 3 replicas and set `maxSurge` to 50%.
2. **Canary Deployment**: Implement a canary deployment with 10% of traffic going to the new version.

This chapter emphasizes choosing the right strategy based on the environment and requirements for service continuity and deployment control.