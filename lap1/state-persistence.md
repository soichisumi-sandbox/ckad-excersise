# State Persistence (8%)

## Define volumes

### o Create busybox pod with two containers, each one will have the image busybox and will run the 'sleep 3600' command. Make both containers mount an emptyDir at '/etc/foo'. Connect to the second busybox, write the first column of '/etc/passwd' file to '/etc/foo/passwd'. Connect to the first busybox and write '/etc/foo/passwd' file to standard output. Delete pod.

* k apply -f sp-pod.yaml
* k exec -it -c bb2 pod1 -- sh
  * echo -n "root\ndaemon..." > /etc/foo/passwd
* k exec -it -c bb1 pod1 -- sh
  * cat /etc/foo/passwd
* k delete pod pod1

### o Create a PersistentVolume of 10Gi, called 'myvolume'. Make it have accessMode of 'ReadWriteOnce' and 'ReadWriteMany', storageClassName 'normal', mounted on hostPath '/etc/foo'. Save it on pv.yaml, add it to the cluster. Show the PersistentVolumes that exist on the cluster

* k apply -f pv.yaml
* k get pv -A

### o Create a PersistentVolumeClaim for this storage class, called mypvc, a request of 4Gi and an accessMode of ReadWriteOnce, with the storageClassName of normal, and save it on pvc.yaml. Create it on the cluster. Show the PersistentVolumeClaims of the cluster. Show the PersistentVolumes of the cluster

* k apply -f pvc.yaml
* k get pvc -A

### o Create a busybox pod with command 'sleep 3600', save it on pod.yaml. Mount the PersistentVolumeClaim to '/etc/foo'. Connect to the 'busybox' pod, and copy the '/etc/passwd' file to '/etc/foo/passwd'

* k apply -f sp-pod2.yaml
* k exec -it busybox -- sh
* cp /etc/passwd /etc/foo/passwd

### o Create a second pod which is identical with the one you just created (you can easily do it by changing the 'name' property on pod.yaml). Connect to it and verify that '/etc/foo' contains the 'passwd' file. Delete pods to cleanup

* k apply -f sp-pod3.yaml
* k exec -it busybox -- sh
* cat /etc/foo/passwd

### o Create a busybox pod with 'sleep 3600' as arguments. Copy '/etc/passwd' from the pod to your local folder

* k run busybox3 --image busybox --command -- sh -c sleep 3600
* k cp busybox3:/etc/passwd /Users/s-sumi/Desktop/passwd
