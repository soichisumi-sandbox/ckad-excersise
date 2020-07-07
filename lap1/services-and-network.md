# Services and Networking (13%)

### o Create a pod with image nginx called nginx and expose its port 80

* k run nginx --image nginx --port=80 --expose

### o Confirm that ClusterIP has been created. Also check endpoints

* k get svc,endpoints

### o Get service's ClusterIP, create a temp busybox pod and 'hit' that IP with wget

* k run busybox --image busybox --restart=Never -it --rm --command -- sh
* k run busybox --image busybox --restart=Never --rm --command --attach -- wget http://10.98.147.158

### o Convert the ClusterIP to NodePort for the same service and find the NodePort port. Hit service using Node's IP. Delete the service and the pod at the end.

* k edit svc nginx
* curl http://192.168.64.2:32507

### o Create a deployment called foo using image 'dgkanatsios/simpleapp' (a simple server that returns hostname) and 3 replicas. Label it as 'app=foo'. Declare that containers in this pod will accept traffic on port 8080 (do NOT create a service yet)

* k apply -f deploy.yaml

### o Get the pod IPs. Create a temp busybox pod and trying hitting them on port 8080

* k get pod -l app=foo -o wide
* k run bb --image busybox -it --rm --command -- sh
* wget http://172.17.0.8

### o Create a service that exposes the deployment on port 6262. Verify its existence, check the endpoints

* k apply -f service.yaml
* k get svc,endpoints

### o Create a temp busybox pod and connect via wget to foo service. Verify that each time there's a different hostname returned. Delete deployment and services to cleanup the cluster

* k run bb --image busybox -it --rm --command -- sh
* wget -O indexN 10.99.253.70:6262
* cat indexN
* k delete deploy app
* k delete svc my-service

### o Create an nginx deployment of 2 replicas, expose it via a ClusterIP service on port 80. Create a NetworkPolicy so that only pods with labels 'access: granted' can access the deployment and apply it

* k apply -f networkpolicy.yaml