# Core Concepts (13%)

## △ Change pod's image to nginx:1.7.1. Observe that the pod will be killed and recreated as soon as the image gets pulled

k get po -w
k describe pod xxx

## x Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'

wget -O- IP:port

### △ Create an nginx pod with a liveness probe that just runs the command 'ls'. Save its YAML in pod.yaml. Run it, check its probe status, delete it.
use describe to check
## Jobs
### △ Create a job with image perl that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"
ok
# Cron jobs

### △ Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from the Kubernetes cluster' to standard output
ok


memo:
* to see status, use describe. not get -o yaml
* do not use pod.spec.containers.ports.hostport
  * hostport determines port which is used for nodeport
* use restart=Never on kubectl run 
* use `kubectl create job pi --image=perl ...` to create job
* use `wget -O- IP:port` to show content of html to stdout
* expose can be used to create service for deployment
* to run pod with request/limit, use: `k run nginx --image=xx --restart=Never --requests='' --limits=''`
* to get secret value, decode by base64
* to use sa, autoMount = true is ok
* use `cut` for simple line string parsing
* 