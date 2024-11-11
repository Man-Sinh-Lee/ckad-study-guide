Chapter 6 from the *Certified Kubernetes Application Developer (CKAD) Study Guide*, covering the use of Jobs and CronJobs.

### Working with Jobs

- **Creating and Inspecting Jobs**: Jobs are used for one-time tasks that need to complete successfully at least once. They can be created imperatively with:
  ```bash
  kubectl create job counter --image=nginx:1.25.1 -- /bin/sh -c 'counter=0; while [ $counter -lt 3 ]; do counter=$((counter+1)); echo "$counter"; sleep 3; done;'
  ```
  This command creates a job that increments a counter and logs the values.

- **Job Operation Types**:
  - **Non-Parallel Job**: A single Pod completes the task once (`completions: 1`, `parallelism: 1`).
  - **Parallel Fixed-Completion Job**: Multiple Pods execute a set number of tasks, configured with `spec.completions` and `spec.parallelism`.
  - **Worker Queue Job**: Multiple Pods handle tasks until one successfully completes, using `parallelism` without `completions`.

- **Restart Behavior**: 
  - The `backoffLimit` attribute specifies retry limits (default 6 retries) before marking the job as failed.
  - **Restart Policies**: `OnFailure` restarts the same Pod container on failure, while `Never` initiates a new Pod on failure.

### Working with CronJobs

- **Creating and Inspecting CronJobs**:
  - CronJobs schedule Jobs at specified times using cron expressions (e.g., `* * * * *` for every minute).
  - Example command:
    ```bash
    kubectl create cronjob current-date --schedule="* * * * *" --image=nginx:1.25.1 -- /bin/sh -c 'echo "Current date: $(date)"'
    ```
    This schedules a task to print the date every minute.

- **Configuring Retained Job History**:
  - CronJobs retain job history for troubleshooting by default, keeping the last 3 successful jobs and 1 failed job.
  - Configurable attributes: `successfulJobsHistoryLimit` and `failedJobsHistoryLimit` can be adjusted for retention (e.g., retain the last 5 successes and 3 failures).

### Summary and Exam Essentials

- **Summary**: Jobs handle finite, one-time tasks, while CronJobs allow scheduling periodic tasks. Understanding how to create, inspect, and manage both Jobs and CronJobs is crucial for batch processing and periodic tasks.
- **Exam Essentials**: Familiarity with different Job configurations, setting parallelism, retention of job histories, and adjusting restart policies are key skills. Practice creating both Jobs and CronJobs and understanding configurations for successful exam preparation.

### Sample Exercises

1. **Job Creation**: Create a Job that generates a hash with `alpine` image, running in parallel across two Pods with 5 completions.
2. **CronJob Creation**: Schedule a `curl` command to execute every two minutes, reconfigure to retain seven past executions, and prevent new execution if the current run is still active.