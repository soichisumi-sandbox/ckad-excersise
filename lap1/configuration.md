# Configuration

## ConfigMap

### o Create a configmap named config with values foo=lala,foo2=lolo

* k create configmap --from-literal foo=lala --from-literal foo2=lolo

### o Display its values

* k get cm -o yaml

### o Create and display a configmap from a file

* k create cm --from-file=./configmap.txt config
* k get cm config -o yaml

### o Create the file with

* echo -e "foo3=lili\nfoo4=lele" > config2.txt

### o Create and display a configmap from a .env file

* k create cm --from-env-file=./config2.txt config2
* k get cm config2 -o yaml

### o Create the file with the command

* echo -e "var1=val1\n# this is a comment\n\nvar2=val2\n#anothercomment" > config.env

### o Create and display a configmap from a file, giving the key 'special'

* echo -e "var3=val3\nvar4=val4" > config4.txt
* k create cm --from-file=special=./configmap.txt config3
* k get cm config3 -o yaml

### o Create a configMap called 'options' with the value var5=val5. Create a new nginx pod that loads the value from variable 'var5' in an env variable called 'option'

* k create cm --from-literal var5=val5 options
* k apply -f pod-env-from-cm.yaml

### o Create a configMap 'anotherone' with values 'var6=val6', 'var7=val7'. Load this configMap as env variables into a new nginx pod

* k create cm --from-literal var6=val6 --from-literal var7=val7 anotherone
* k apply -f pod-env-from-cm2.yaml

### o Create a configMap 'cmvolume' with values 'var8=val8', 'var9=val9'. Load this as a volume inside an nginx pod on path '/etc/lala'. Create the pod and 'ls' into the '/etc/lala' directory.

* k create cm cmvolume --from-literal var8=val8 --from-literal var9=val9\
* k apply -f pod-env-from-cm3.yaml
* k exec nginx3 -- ls /etc/lala

## SecurityContext

### o create the YAML for an nginx pod that runs with the user ID 101. No need to create the pod

* cat pod-secctx.yaml

### o Create the YAML for an nginx pod that has the capabilities "NET_ADMIN", "SYS_TIME" added on its single container

* cat pod-secctx2.yaml

## Requests and limits

### o Create an nginx pod with requests cpu=100m,memory=256Mi and limits cpu=200m,memory=512Mi

* k apply -f pod-quota.yaml

## Secrets

### o Create a secret called mysecret with the values password=mypass

* kubectl create secret generic mysecret --from-literal=password=mypass

### o Create a secret called mysecret2 that gets key/value from a file `echo -n admin > username`

* k create secret generic mysecret2 --from-file=./username

### o Get the value of mysecret2

* k get secret mysecret2 -o yaml

### o Create an nginx pod that mounts the secret mysecret2 in a volume on path /etc/foo

* k apply -f pod-volume-secret.yaml

### o Delete the pod you just created and mount the variable 'username' from secret mysecret2 onto a new nginx pod in env variable called 'USERNAME'

* k apply -f pod-volume-secret2.yaml

## ServiceAccounts

### o See all the service accounts of the cluster in all namespaces

* k get sa -A

### o Create a new serviceaccount called 'myuser'

* k create sa myuser

### △ Create an nginx pod that uses 'myuser' as a service account

* k apply -f pod-sa.yaml