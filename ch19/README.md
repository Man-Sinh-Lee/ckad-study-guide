Chapter 19 in the CKAD guide covers "ConfigMaps and Secrets" in Kubernetes, providing methods to manage configuration data and sensitive information separately from code. Here’s a detailed summary:

### 1. **Working with ConfigMaps**:
   - **Creating a ConfigMap**: 
     ConfigMaps store plain-text configuration data, such as connection strings or environment settings, which can be used across multiple pods. ConfigMaps can be created with:
     - *Literal key-value pairs*:
       ```bash
       kubectl create configmap app-config --from-literal=key1=value1 --from-literal=key2=value2
       ```
     - *Environment files*:
       ```bash
       kubectl create configmap app-config --from-env-file=config.env
       ```
     - *Files or directories* with structured data such as JSON:
       ```bash
       kubectl create configmap app-config --from-file=config.json
       ```

   - **Consuming a ConfigMap as Environment Variables**:
     ConfigMaps can inject key-value pairs into containers as environment variables:
     ```yaml
     envFrom:
       - configMapRef:
           name: app-config
     ```
   
   - **Mounting a ConfigMap as a Volume**:
     ConfigMaps can also be mounted as files within a container, allowing applications to read configuration files directly:
     ```yaml
     volumeMounts:
       - name: config-volume
         mountPath: /etc/config
     volumes:
       - name: config-volume
         configMap:
           name: app-config
     ```

### 2. **Working with Secrets**:
   - **Creating a Secret**:
     Secrets are similar to ConfigMaps but are designed for sensitive information like passwords. Values are Base64-encoded for obfuscation:
     - Creating a secret from literals:
       ```bash
       kubectl create secret generic db-creds --from-literal=username=myuser --from-literal=password=mypassword
       ```
     - Using specialized types (e.g., `docker-registry`, `tls`):
       ```bash
       kubectl create secret docker-registry my-registry-creds --docker-username=user --docker-password=password
       ```
     
   - **Consuming a Secret as Environment Variables**:
     Secrets can also be used as environment variables, enabling a more secure way to handle sensitive data:
     ```yaml
     env:
       - name: DB_USERNAME
         valueFrom:
           secretKeyRef:
             name: db-creds
             key: username
       - name: DB_PASSWORD
         valueFrom:
           secretKeyRef:
             name: db-creds
             key: password
     ```
   
   - **Mounting a Secret as a Volume**:
     Similar to ConfigMaps, secrets can be mounted as files within a container:
     ```yaml
     volumeMounts:
       - name: secret-volume
         mountPath: /var/secret
         readOnly: true
     volumes:
       - name: secret-volume
         secret:
           secretName: db-creds
     ```

### 3. **Summary**:
   - ConfigMaps and Secrets allow separating configuration and sensitive data from application code. ConfigMaps handle plain-text data, while Secrets manage sensitive data, with values stored in Base64 encoding. Both can be injected into containers as environment variables or mounted as volumes.

### 4. **Exam Essentials**:
   - Know how to create, view, and manage ConfigMaps and Secrets, both imperatively and declaratively.
   - Understand how to inject and mount configuration data in pods, which is essential for the CKAD exam.

### 5. **Sample Exercises**:
   - Exercises include creating ConfigMaps and Secrets, consuming them in pods as environment variables or volumes, and inspecting the data from within containers.

This chapter provides essential tools for securely managing application configurations in Kubernetes