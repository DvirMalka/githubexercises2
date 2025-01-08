
## Lab 3.3: Jobs and CronJobs

Goal: create a _Job_ and a _CronJob_

Duration: 0:15:00

### First Job

- Create a _Job_ from the following descriptor (source : `~/workspaces/Lab3/job--compute-pi--3.3.yml`):

```yaml
---
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    metadata:
      name: pi
    spec:
      containers:
        - name: pi
          image: perl:5.20-stretch
          command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
```

- Check the _Job_ logs once it is finished
- Show detailed information about the _Job_

### A bit of parallelism

- Make the changes so that the _Job_ is launched 5 times, with 2 occurrences in parallel

### CronJob (Optional)

- Create the _CronJob_ from the following descriptor (source : `~/workspaces/Lab3/cron--hello-from-k8s-cluster--3.3.yml`):

```yaml
---
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: hello
              image: debian:11-slim
              args:
                - bash
                - -c
                - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

- After creating the _CronJob_, retrieve its status with `kubectl get cronjob`
- Watch the first associated _Job_: `kubectl get jobs --watch`
- Retrieve the status of the _CronJob_ once the first _Job_ has finished
- Suspend the _CronJob_ to stop the execution of Jobs for the rest of the training

<div class="pb"></div>
