# Observability

## Liveness and readiness probes

### Create an nginx pod with a liveness probe that just runs the command 'ls'. Save its YAML in pod.yaml. Run it, check its probe status, delete it.

* k apply -f pod.yaml
* k get nginx -o yaml
* k delete pod nginx

### ○ Modify the pod.yaml file so that liveness probe starts kicking in after 5 seconds whereas the interval between probes would be 5 seconds. Run it, check the probe, delete it.

* k apply -f pod2.yaml
* k get nginx2 -o yaml
* k delete pod nginx2

### △ Create an nginx pod (that includes port 80) with an HTTP readinessProbe on path '/' on port 80. Again, run it, check the readinessProbe, delete it.

* k apply -f pod3.yaml
* k get po nginx3 -o yaml
* k delete pod nginx3

## Logging

### ○ Create a busybox pod that runs 'i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 1; done'. Check its logs

* k run busybox --image busybox --restart=Never --command -- sh -c 'i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 1; done'

## Debugging

### ○ Create a busybox pod that runs 'ls /notexist'. Determine if there's an error (of course there is), see it. In the end, delete the pod

* k run busybox2 --image busybox --restart=Never --command -- ls /notexist
* k get po busybox2 -o yaml

### o Create a busybox pod that runs 'notexist'. Determine if there's an error (of course there is), see it. In the end, delete the pod forcefully with a 0 grace period

* k run busybox2 --image busybox --restart=Never --command -- notexist
* k delete pod busybox2 --force --grace-period=0

### o Get CPU/memory utilization for nodes (metrics-server must be running)

* k top nodes
