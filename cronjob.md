
A job can be scheduled to run multiple times, using a cron job.

A cron job builds on a regular job by allowing you to specify how the job should be run. Cron jobs are part of the Kubernetes API, which can be managed with oc commands like other object types.

Cron jobs are useful for creating periodic and recurring tasks, like running backups or sending emails. Cron jobs can also schedule individual tasks for a specific time, such as if you want to schedule a job for a low activity period. A cron job creates a Job object based on the timezone configured on the control plane node that runs the cronjob controller.


```
cronjob.yaml:
=============
apiVersion: batch/v1
kind: CronJob
metadata:
  name: python-script-cron
  namespace: vijay
spec:
  schedule: "* * * * *"  #Runs every minute
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: python-script
            image: ubuntu:latest
            imagePullPolicy: Always
            command: ["/bin/bash", "-c"]
            args: ["echo 'cron tab'"]
          restartPolicy: OnFailure
```

```

oc apply -f cronjob.yaml  

oc get cronjob -n vijay

oc get pods 

oc logs <pod-name>

oc get jobs

oc describe job <job-name>

```



deleting crob job:
==================
```
oc delete cronjob python-script-cron -n zfw-jenkins-zfab-app

oc get jobs -n vijay
```


CronJob History Limits: 
- Ensure that the successfulJobsHistoryLimit and failedJobsHistoryLimit are set in your cronjob configuration. When a cronjob is deleted, these limits determine how many successful and failed jobs are retained:

```
cronjob.yaml:
-------------
apiVersion: batch/v1
kind: CronJob
metadata:
  name: python-script-cron
  namespace: vijay
spec:
  schedule: "* * * * *"  # Runs every minute
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: python-script
            image: ubuntu:latest
            imagePullPolicy: Always
            command: ["/bin/bash", "-c"]
            args: |
              echo 'cron tab'
              echo 'additional argument'
              echo 'another argument'
          restartPolicy: OnFailure
```
