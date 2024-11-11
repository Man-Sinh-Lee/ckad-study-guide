Chapter 10 on Deployments from the *Certified Kubernetes Application Developer (CKAD) Study Guide*:

### Working with Deployments

1. **Creating Deployments**:
   - A Deployment manages a group of replicated Pods, supporting updates and scaling automatically. You can create a Deployment using:
     ```bash
     kubectl create deployment app-cache --image=memcached:1.6.8 --replicas=4
     ```
     - Alternatively, a YAML manifest can define the Deployment declaratively:
       ```yaml
       apiVersion: apps/v1
       kind: Deployment
       metadata:
         name: app-cache
       spec:
         replicas: 4
         selector:
           matchLabels:
             app: app-cache
         template:
           metadata:
             labels:
               app: app-cache
           spec:
             containers:
             - name: memcached
               image: memcached:1.6.8
       ```

2. **Listing Deployments and Their Pods**:
   - `kubectl get deployments` lists Deployments, showing columns such as `READY`, `UP-TO-DATE`, and `AVAILABLE`. Each Deployment is associated with a ReplicaSet that manages the Pods, identified by labels and selectorsndering Deployment Details**:
   - `kubectl describe deployment <deployment-name>` provides detailed information, including Pod templates, ReplicaSet references, and rolling update strategies.

4. **Deleting a Deployment**:
   - Use `kubectl delete deployment <deployment-name>` to remove a Deployment and its managed Pods.

### Performing Rolling Updates and Rollbacks

1. **Updating a Deployment’s Pod Template**:
   - Deployments support various ways to update Pods, including `kubectl apply`, `kubectl edit`, `kubectl set image`, and `kubectl patch`. For example:
     ```bash
     kubectl set image deployment app-cache memcached=memcached:1.6.10
     ```

2. **Rolling Out a New Revision**:
   - Deployments implement a rolling update by default, updating Pods gradually. You can monitor the status with `kubectl rollout status <deployment-name>`.

3. **Adding a Change Cause for a Revision**:
   - The change cause can be annotated to track the reason for each revision, which is visible in the rollout history:
     ```bash
     kubectl annotate deployment app-cache kubernetes.io/change-cause="Updated to v1.6.10"
     ```

4. **Rolling Back to a Previous Revision**:
   - Rollbacks are possible using:
     ```bash
     kubectl rollout undo deployment <deployment-name> --to-revision=<revision-number>
     ```

### Scaling Workloads

1. **Manually Scaling a Deployment**:
   - Scale manually with `kubectl scale`:
     ```bash
     kubectl scale deployment app-cache --replicas=6
     ```

2. **Autoscaling a Deployment**:
   - Horizontal Pod Autoscaler (HPA) dynamically adjusts the replica count based on resource usage. Define an HPA using `kubectl autoscale deployment` or YAML:
     ```yaml
     apiVersion: autoscaling/v2
     kind: HorizontalPodAutoscaler
     metadata:
       name: app-cache
     spec:
       scaleTargetRef:
         apiVersion: apps/v1
         kind: Deployment
         name: app-cache
       minReplicas: 3
       maxReplicas: 5
       metrics:
       - type: Resource
         resource:
           name: cpu
           target:
             type: Utilization
             averageUtilization: 80
       - type: Resource
         resource:
           name: memory
           target:
             type: AverageValue
             averageValue: 500Mi
     ```

3. **Listing and Inspecting Horizontal Pod Autoscalers**:
   - `kubectl get hpa` lists HPAs, and `kubectl describe hpa <hpa-name>` provides HPA details.

### Summary and Exam Essentials

- **Summary**: Deployments offer a robust way to manage replicated Pods, supporting rolling updates, rollbacks, and scaling operations.
- **Exam Essentials**: Understand creating, updating, and scaling Deployments. Familiarize with HPA setup and metrics-based scaling.

### Sample Exercises

1. **Deployment Exercise**: Create a Deployment with 3 replicas of `nginx:1.23.0`, label it, and verify it’s running.
2. **HPA Setup**: Configure an HPA for a Deployment, setting thresholds for CPU and memory utilization.