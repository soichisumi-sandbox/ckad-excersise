# multi-container-pods

## ○ Create a Pod with two containers, both with image busybox and command "echo hello; sleep 3600". Connect to the second container and run 'ls'

* k apply -f mc.yaml
* k exec mc -it -c mc-2 -- ls