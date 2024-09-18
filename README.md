# Exploring GitHub Actions

- By Default actions are running on another machine that has no access to our github directory
- The pipe used to run a sequence of cmds ( | )
- In order to execute a shell script within your workflow, verify that the script have permission to be executed
- Using the github runner each job is run on a separate VM
    - A default behavior is that all jobs runs on parallel and do not depend one on another
    - Use **needs** to identify any jobs that must complete successfully before this job will run
![needs sequence](images/sequence_jobs.png)

- If needs is used and one job depends on another, if one fails the next one will be skipped

- By default Files are not shared between jobs
    - So we need yo use **artifacts** to upload a files from a job to another and than download it whare we need it. [Upload](https://github.com/marketplace/actions/upload-a-build-artifact)! [Download](https://github.com/marketplace/actions/download-a-build-artifact)!

- Artifact and log retention by default is 90days, can be changed (Settings -> Actions -> Retention days)

### We can define envirenment variables on three levels (Step level, Job level and Workflow level)

### We can trigger our workflow using many triggers for example : 
![workflow-dispatch](images/workflow_dispatch.png)

### Without concurrency if a workflow is stuck and we need to run another one than the first will always keep running -> We will loose time (2000 minutes free ...)

![concurrency](images/concurrency.png)

- If concurrency  = false workflow triggered after the other will wait for the first one in order to be completed:
![concurrencyFalse](images/concurrencyFalse.png)