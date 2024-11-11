Here’s a summary of Chapter 12 on Helm from the *Certified Kubernetes Application Developer (CKAD) Study Guide*, detailing how to manage existing Helm charts.

### Managing an Existing Chart

1. **Identifying a Chart**:
   - Helm charts are templates for Kubernetes applications and can be located on *Artifact Hub* or other repositories. To find a chart for a tool like Jenkins, search using keywords, and inspect available versions and templates for configuration options.

2. **Adding a Chart Repository**:
   - Add repositories with `helm repo add` using a repository name and URL. For instance, adding the Jenkins repository:
     ```bash
     helm repo add jenkinsci https://charts.jenkins.io/
     ```
   - List all added repositories using `helm repo list`.

3. **Searching for a Chart in a Repository**:
   - Use `helm search repo <repository-name>` to explore available charts within a repository. Use `--versions` to list different versions of a chart.

4. **Installing a Chart**:
   - Install a chart by specifying its repository and version:
     ```bash
     helm install my-jenkins jenkinsci/jenkins --version 4.6.4
     ```
   - The command downloads the chart and creates the specified Kubernetes objects (Pods, Services, etc.) in the default namespace or a specified namespace with `-n <namespace>`d Charts**:
   - Display all installed Helm charts with `helm list`, using `--all-namespaces` to see charts across namespaces. This command outputs the chart’s namespace, version, and deployment status.

6. **Upgrading an Installed Chart**:
   - Upgrade a chart to a newer version using:
     ```bash
     helm upgrade my-jenkins jenkinsci/jenkins --version 4.6.5
     ```
   - If needed, customize the upgrade using `--set` or `--values` flags to override default configurations【6:0†source】【15:0†sourcehart**:
   - Remove an installed chart with `helm uninstall`, which deletes all associated Kubernetes objects:
     ```bash
     helm uninstall my-jenkins
     ```
   - The uninstallation may take a few seconds as Kubernetes applies the workload grace period .

### Summary

Helm isool for managing Kubernetes applications. Key workflows include locating a chart, adding the repository, and installing, listing, upgrading, and uninstalling charts. The `helm install`, `helm upgrade`, and `helm uninstall` commands are essential for chart lifecycle management, and `--set` or `--values` allow for custom configuration.

### Exam Essentials

- **Artifact Hub Familiarity**: While the exam may not involve browsing Artifact Hub, understanding Helm’s repository-based management of charts is essential.
- **Command Proficiency**: Be prepared to add repositories, install, upgrade, and uninstall charts, with confidence in using both `--set` and `--values` for customization.

### Sample Exercises

1. **Repository Addition**: Add a Prometheus Helm chart repository and install the latest version of `kube-prometheus-stack`.
2. **Upgrade Practice**: Upgrade an installed Helm chart to a specified version, customizing specific values during the process.

This chapter provides foundational skills for Helm usage, allowing efficient management of reusable application configurations【11:0†source】  .