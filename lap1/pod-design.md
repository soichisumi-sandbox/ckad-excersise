# pod-design

## Labels and annotations

### o Create 3 pods with names nginx1,nginx2,nginx3. All of them should have the label app=v1

* k run nginx1 --image nginx -l app=v1 --restart=Never
* k run nginx2 --image nginx -l app=v1 --restart=Never
* k run nginx3 --image nginx -l app=v1 --restart=Never

### o Show all labels of the pods

* k get po --show-labels

### o Change the labels of pod 'nginx2' to be app=v2

* k label po nginx2 app=v2 --overwrite

### o Get the label 'app' for the pods

* k get po -l app --show-labels

### o Get only the 'app=v2' pods

* k get po -l app=v2 --show-labels

### o Remove the 'app' label from the pods we created before

* k label --overwrite pods nginx1 nginx2 nginx3 app-

### o Create a pod that will be deployed to a Node that has the label 'accelerator=nvidia-tesla-p100'

* k apply -f bbnode.yaml

### o Annotate pods nginx1, nginx2, nginx3 with "description='my description'" value

* k annotate pods nginx1 nginx2 nginx3 description='my description'

### o Check the annotations for pod nginx1

* k get pod nginx1 -o yaml

### o Remove the annotations for these three pods

* k annotate pods nginx1 nginx2 nginx3 --overwrite description-

### o Remove these pods to have a clean state in your cluster

* k delete pod --all


## Deployments

### o Create a deployment with image nginx:1.7.8, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)

* k apply -f deploy-1.yaml

### o View the YAML of this deployment

* k get deploy nginx -o yaml

### o View the YAML of the replica set that was created by this deployment

* k get replicaset -l pod-template-hash=5b6f47948 -o yaml

### o Get the YAML for one of the pods

* k get po nginx-5b6f47948-5t6fc -o yaml

### o Check how the deployment rollout is going

* k rollout status -w deployment/nginx

### o Update the nginx image to nginx:1.7.9

* k set image deployment nginx nginx=1.7.9

### o Check the rollout history and confirm that the replicas are OK

* k rollout history deployment/nginx
* k get deploy nginx -o wide

### o Undo the latest rollout and verify that new pods have the old image (nginx:1.7.8)

* kubectl rollout undo deployment/frontend 

### o Do an on purpose update of the deployment with a wrong image nginx:1.91

* k set image deployment nginx nginx=nginx:1.91 

### o Verify that something's wrong with the rollout

* k describe  pod nginx-7789688b8f-5gznj

### o Return the deployment to the second revision (number 2) and verify the image is nginx:1.7.9

* kubectl rollout undo deployment/nginx
* k get deploy nginx -o yaml

### o Check the details of the fourth revision (number 4)

* k rollout history deployment/nginx --revision=3

### o Scale the deployment to 5 replicas

* k scale --replicas 5 deploy/nginx 

### o Autoscale the deployment, pods between 5 and 10, targetting CPU utilization at 80%

* k autoscale deploy nginx --min=5 --max=10 --cpu-percent=80

### o Pause the rollout of the deployment

* k rollout pause deployment/nginx 

### o Update the image to nginx:1.9.1 and check that there's nothing going on, since we paused the rollout

* k set image deploy nginx nginx=nginx:1.9.1
* k rollout history deployment/nginx 

### o Resume the rollout and check that the nginx:1.9.1 image has been applied

* k rollout resume deploy/nginx

### o Delete the deployment and the horizontal pod autoscaler you created

* k delete deploy nginx
* k delete horizontalpodautoscaler nginx


## Jobs
### △ Create a job with image perl that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"

* k apply -f job1.yaml

### o Wait till it's done, get the output

* k get job -w

### o Create a job with the image busybox that executes the command 'echo hello;sleep 30;echo world'

* k apply -f job2.yaml

### o Follow the logs for the pod (you'll wait for 30 seconds)

* k logs job2-xxxx

### o See the status of the job, describe it and see the logs

* k describe job job2
* k logs job2-xxx

### o Delete the job

* k delete job job2

### o Create a job but ensure that it will be automatically terminated by kubernetes if it takes more than 30 seconds to execute

* k apply -f job3.yaml

### o Create the same job, make it run 5 times, one after the other. Verify its status and delete it

* k apply -f job4.yaml
* k get job -w
* k delete job job4

### o Create the same job, but make it run 5 parallel times

* k apply -f job5.yaml


# Cron jobs

### △ Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from the Kubernetes cluster' to standard output

* k apply -f cronjob.yaml

### o See its logs and delete it

* k logs job-xxx -w

### o Create a cron job with image busybox that runs every minutes and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it takes more than 17 seconds to execute.

* k apply -f cronjob2.yaml

