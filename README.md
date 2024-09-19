# Comprehensive Guide to GitHub Actions

## Basic Concepts

1. **Execution Environment**
   - By default, actions run on separate machines without direct access to your GitHub repository.
   - Each job in a workflow runs on a separate virtual machine (VM).

2. **Job Execution**
   - Default behavior: All jobs run in parallel.
   - To create dependencies between jobs, use the `needs` keyword.

3. **File Sharing Between Jobs**
   - Files are not automatically shared between jobs.
   - Use artifacts to transfer files between jobs.

4. **Shell Commands**
   - Use the pipe (`|`) to run a sequence of commands.
   - Ensure shell scripts have execution permissions before running them in your workflow.

## Workflow Configuration

### Job Dependencies
- Use `needs` to specify jobs that must complete successfully before another job runs.
- If a job fails, dependent jobs will be skipped.

![Job Sequence with 'needs'](images/sequence_jobs.png)

### Artifacts
- Use artifacts to share files between jobs.
- [Upload Artifact Action](https://github.com/marketplace/actions/upload-a-build-artifact)
- [Download Artifact Action](https://github.com/marketplace/actions/download-a-build-artifact)

### Retention Policies
- Default artifact and log retention: 90 days
- Can be changed in Settings -> Actions -> Retention days

### Environment Variables
Define environment variables at three levels:
1. Workflow level
2. Job level
3. Step level

### Workflow Triggers
Multiple triggers available, including:
- `workflow_dispatch` for manual triggering

![Workflow Dispatch Trigger](images/workflow_dispatch.png)

### Concurrency
- Without concurrency, multiple workflow runs can occur simultaneously.
- With `concurrency: false`, subsequent triggers wait for the current run to complete.

![Concurrency False](images/concurrencyFalse.png)

**Important**: Consider adding `timeout-minutes` to prevent infinite runs and manage resources effectively.

## Matrix Strategy

- Default behavior: If a job fails, other matrix jobs stop running.
- You can exclude specific combinations or entire keys from the matrix.

### Exclusion Examples

1. Excluding a specific combination:
```yaml
matrix:
  os: [ubuntu-latest, macos-12, windows-latest]
  py-version: [3.7, 3.8, 3.9]
  exclude:
    - os: macos-12
      py-version: 3.9
```

2. Excluding all combinations with specific values:
```yaml
matrix:
  os: [ubuntu-latest, macos-12, windows-latest]
  py-version: [3.7, 3.8, 3.9]
  exclude:
    - os: macos-12
    - py-version: 3.9
```

### Additional Matrix Options
- Control the number of parallel jobs
- Continue on failure option

## Best Practices

1. **Resource Management**: Use `timeout-minutes` to prevent long-running jobs from consuming excessive resources.
2. **Artifact Management**: Regularly clean up unnecessary artifacts to save storage.
3. **Concurrency Control**: Use concurrency settings to manage workflow execution and prevent resource conflicts.
4. **Matrix Optimization**: Carefully plan your matrix strategy to balance thoroughness with efficiency.
5. **Environment Variables**: Use appropriate scoping for environment variables to maintain security and clarity.

Remember to consult the [official GitHub Actions documentation](https://docs.github.com/en/actions) for the most up-to-date information and advanced features.
