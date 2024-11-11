Chapter 14 on Container Probes from the *Certified Kubernetes Application Developer (CKAD) Study Guide*, which discusses different types of health checks for containers in Kubernetes.

### Working with Probes

Kubernetes provides three main probe types to monitor container health and take necessary actions if issues arise:

- **Readiness Probe**: Determines if an application is ready to handle traffic by checking for conditions like database connectivity. When the probe passes, the application can receive requests.
- **Liveness Probe**: Checks if the application is still functioning correctly. Kubernetes restarts the container if the probe fails, addressing issues such as deadlocks or memory leaks.
- **Startup Probe**: Waits for an application to complete its startup before enabling other probes. This probe is especially useful for applications with long initialization times, preventing premature health checks and restarts.

### Health Verification Methods

Each probe can use various methods to check container health:

1. **exec.command**: Executes a command inside the container, considering a zero exit code as success.
2. **httpGet**: Sends an HTTP GET request to an endpoint within the container, where responses in the 200–399 range are deemed successful.
3. **tcpSocket**: Opens a TCP socket on a specific port, and successful connections indicate a healthy state.
4. **gRPC**: Uses the gRPC Health Checking Protocol for applications that support it, ensuring the container can handle remote procedure calls (RPCs).

### Health Check Attributes

Each probe type allows configuration using attributes such as:

- **initialDelaySeconds**: Sets a delay before the first check.
- **periodSeconds**: Determines the interval between checks.
- **timeoutSeconds**: Sets the maximum wait time for each check.
- **successThreshold**: Specifies the number of consecutive successful checks required to consider the container healthy after a failure.
- **failureThreshold**: Defines the failure count before marking the container as unhealthy.

### The Readiness Probe

- Example:
  ```yaml
  readinessProbe:
    httpGet:
      path: /
      port: 8080
    initialDelaySeconds: 5
    periodSeconds: 10
  ```
  This readiness probe checks an HTTP endpoint at `/` on port 8080, waiting five seconds after container start and checking every 10 seconds afterward.

### The Liveness Probe

- Example:
  ```yaml
  livenessProbe:
    exec:
      command: ["cat", "/tmp/heartbeat.txt"]
    initialDelaySeconds: 15
    periodSeconds: 20
  ```
  This liveness probe checks if the file `/tmp/heartbeat.txt` exists, and if not, restarts the container, allowing detection of potential application failures.

### The Startup Probe

- Example:
  ```yaml
  startupProbe:
    tcpSocket:
      port: 80
    initialDelaySeconds: 10
    periodSeconds: 15
  ```
  A startup probe using TCP checks port 80 after a 10-second delay and rechecks every 15 seconds, allowing ample time for a slow-starting application to initialize before liveness checks begin.

### Summary and Exam Essentials

The chapter emphasizes the importance of configuring probes to improve application reliability and resilience. The readiness probe controls request handling, while liveness and startup probes ensure continued container operation.

### Sample Exercises

1. **Readiness Probe**: Configure a readiness probe to verify a web service at `/status` on port 8081.
2. **Startup Probe**: Add a startup probe to a container to check readiness on port 8080 after 30 seconds to allow a complex application time to start.

This chapter underscores effective probe configuration as a core competency in maintaining healthy applications in Kubernetes.