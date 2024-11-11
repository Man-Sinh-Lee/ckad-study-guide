Chapter 16 on CustomResourceDefinitions (CRDs) from the *Certified Kubernetes Application Developer (CKAD) Study Guide*.

### Working with CRDs

1. **Example CRD**:
   - A CustomResourceDefinition (CRD) extends Kubernetes by allowing custom API primitives. For example, the CRD for a `SmokeTest` custom resource might specify attributes like `service`, `path`, `timeout`, and `retries`. This schema registers a new resource type within Kubernetes:
     ```yaml
     apiVersion: apiextensions.k8s.io/v1
     kind: CustomResourceDefinition
     metadata:
       name: smoketests.stable.bmuschko.com
     spec:
       group: stable.bmuschko.com
       versions:
         - name: v1
           served: true
           storage: true
           schema:
             openAPIV3Schema:
               type: object
               properties:
                 spec:
                   type: object
                   properties:
                     service:
                       type: string
                     path:
                       type: string
                     timeout:
                       type: integer
                     retries:
                       type: integer
       scope: Namespaced
       names:
         plural: smoketests
         singular: smoketest
         kind: SmokeTest
         shortNames:
         - st
     ```

2. **Implementing a CRD Schema**:
   - CRDs use an OpenAPI v3 specification to define attributes for custom resources, which is essential for validation. The example above creates a CRD for `SmokeTest`, where each property in the `spec` section has a specific data type and configuration for validationnstantiating an Object for the CRD**:
   - After defining a CRD, you can instantiate objects of the custom type by specifying the desired values:
     ```yaml
     apiVersion: stable.bmuschko.com/v1
     kind: SmokeTest
     metadata:
       name: backend-smoke-test
     spec:
       service: backend
       path: /health
       timeout: 600
       retries: 3
     ```
     - This YAML manifest creates a `SmokeTest` object, which checks the health of the `backend` service endpoint.

4. **Discovering CRDs**:
   - Discovering available CRDs in a cluster can be done with:
     ```bash
     kubectl get crds
     ```
   - Specific CRD schemas can be listed with:
     ```bash
     kubectl api-resources --api-group=stable.bmuschko.com
     ```
   - This returns the custom resources available in the specified API group 【6†sourcng a Controller**:
   - CRDs typically rely on controllers to manage their lifecycle. A controller for `SmokeTest` could, for example, make HTTP calls to a service endpoint and analyze the response, acting based on the result (e.g., updating a dashboard). This process, part of the "operator pattern," involves implementing a controller that regularly checks and reconciles the state of custom objects within Kubernetes  .

### Summary

es CRDs as a way to extend Kubernetes by defining new resources. While creating a CRD schema allows for defining custom resources, controllers are needed to manage these resources and respond to their states actively. 

### Exam Essentials

- **Understanding CRD Basics**: Familiarize yourself with CRD schemas and the purpose of controllers in managing CRDs.
- **Commands**: Know how to list, create, and inspect CRDs using `kubectl` commands.

### Sample Exercises

1. **Creating a CRD**: Define a simple CRD schema and instantiate an object with it.
2. **Interacting with a CRD Object**: Implement and interact with a CRD object using `kubectl get` and `kubectl delete`.

This chapter is crucial for extending Kubernetes functionalities to meet specific application requirements.