Chapter 15 on Troubleshooting Pods and Containers from the *Certified Kubernetes Application Developer (CKAD) Study Guide*.

### Troubleshooting Pods

1. **Retrieving High-Level Information**:
   - Use `kubectl get pods` to check Pod readiness, status, and restarts. The `STATUS` column displays issues like `ErrImagePull`, indicating a problem pulling the image, or `CrashLoopBackOff`, showing repeated container crashes. For instance:
     ```bash
     kubectl get pods
     ```
   - In-depth information can be gathered with `kubectl describe pod <pod-name>` to view events and issues like missing Secrets or failed mountsspecting Events**:
   - Events provide insights into why Pods may be failing. Command:
     ```bash
     kubectl get events
     ```
   - Use this to identify configuration or permission issues, such as when a Pod fails to access a Secret .

3. **Uorwarding**:
   - For troubleshooting or testing a Pod, port-forwarding allows local access to an application inside a Pod:
     ```bash
     kubectl port-forward <pod-name> 8080:80
     ```
   - This connects local port 8080 to the Pod’s port 80, enabling direct requests to the application .

### Troubleshooters

1. **Inspecting Logs**:
   - Logs are essential for identifying application errors. Use `kubectl logs <pod-name>` to view logs, and `-f` to stream logs in real-time:
     ```bash
     kubectl logs <pod-name>
     ```
   - For restarts, `--previous` retrieves logs from the previous container instance .

2. **Opening an Interactiv   - For deeper investigation, open an interactive shell within the container using:
     ```bash
     kubectl exec -it <pod-name> -- /bin/sh
     ```
   - This helps check file directories or environment variables .

3. **Interacting with a Distroless C
   - Distroless containers lack shell utilities, limiting interactive troubleshooting. Kubernetes provides ephemeral containers, allowing injection of debugging tools without restarting the main container, using:
     ```bash
     kubectl alpha debug <pod-name> --image=busybox
     ```

### Inspecting Resource Metrics

- Kubernetes metrics server gathers CPU and memory data. Using `kubectl top`, monitor Pods and nodes:
  ```bash
  kubectl top pod <pod-name>
  ```
- This helps assess if a Pod is resource-starved or misconfigured .

### Summary and Exam Essentials

This chapter understanding `kubectl` troubleshooting commands for Pods and containers, focusing on log analysis, resource metrics, and using ephemeral containers.

### Sample Exercises

1. **Event Inspection**: Diagnose a Pod with a missing ConfigMap or Secret by examining the `kubectl describe pod` output.
2. **Metrics Analysis**: Use `kubectl top` to evaluate resource usage for Pods and make recommendations for resource adjustments .