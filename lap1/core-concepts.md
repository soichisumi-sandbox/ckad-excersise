# Core Concepts (13%)

## ○ Create a namespace called 'mynamespace' and a pod with image nginx called nginx on this namespace

* k create ns mynamespace
* k run nginx --image nginx -n mynamespace 

## ○ Create the pod that was just described using YAML

* k apply -f nginx.yaml

## ○ Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output

* k run busybox --image busybox --command env
* k logs busybox

## ○ Create a busybox pod (using YAML) that runs the command "env". Run it and see the output

* k apply -f busybox.yaml

## ○ Get the YAML for a new namespace called 'myns' without creating it

* k create ns myns --dry-run -o yaml

## ○ Get the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it

* k create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run -o yaml

## ○ Get pods on all namespaces

* k get po -A

## ○ Create a pod with image nginx called nginx and allow traffic on port 80

* k run nginx --image nginx --port 80

## △ Change pod's image to nginx:1.7.1. Observe that the pod will be killed and recreated as soon as the image gets pulled

* k patch pod nginx -p '{"spec":{"containers": [{"name": "nginx", "image":"nginx:1.7.1"}]}}'~
* k set image pod/nginx nginx=nginx:1.7.1
* k get po --watch

!to see events: k describe po nginx

## △ Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'

* k get po nginx -o wide
* k run -i --tty busybox --image busybox -- sh

## ○ Get pod's YAML

* k get po nginx -o yaml

## ○ Get information about the pod, including details about potential issues (e.g. pod hasn't started)

* k describe po nginx

## ○ Get pod logs

* k logs nginx --all-containers

## ○ If pod crashed and restarted, get logs about the previous instance

* k logs -p nginx --all-containers

## ○ Execute a simple shell on the nginx pod

* k exec nginx -it -- sh

## ○ Create a busybox pod that echoes 'hello world' and then exits

* k run bb --image busybox --restart=Never -- echo "hello world"

## ○ Do the same, but have the pod deleted automatically when it's completed

* k run bb --image busybox --rm --restart=Never -it -- echo "hello world"

## ○ Create an nginx pod and set an env value as 'var1=val1'. Check the env value existence within the pod

* k run nx --image nginx --env="TESTENV=true"
* k exec nx -- env

