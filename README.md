Minimal repro for [docker/for-mac#7706](https://github.com/docker/for-mac/issues/7706)
Reproduced on Docker for Mac 4.41.2 (191736)
Kubeadm Cluster version: 1.32.2
Kind Cluster version: 1.32.2

## Run it
```
DATA_PATH=$(pwd)/data envsubst < repro.yml | kubectl apply -f -
```

## with kubeadm
```
$ DATA_PATH=$(pwd)/data envsubst < repro.yml | kubectl apply -f -
namespace/repro created
deployment.apps/nginx-deployment created

$ kubectl -n repro get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-7665d59fc-mvffg   1/1     Running   0          8s

$ kubectl -n repro exec -it pod/nginx-deployment-7665d59fc-mvffg -- bash
root@nginx-deployment-7665d59fc-mvffg:/# ls /data
kind.txt  kubeadm.txt
root@nginx-deployment-7665d59fc-mvffg:/#
```

### With kind
```
$ DATA_PATH=$(pwd)/data envsubst < repro.yml | kubectl apply -f -
namespace/repro created
deployment.apps/nginx-deployment created

$ kubectl -n repro get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-7665d59fc-l8lhl   1/1     Running   0          8s

$ kubectl -n repro exec -it pod/nginx-deployment-7665d59fc-l8lhl -- bash
root@nginx-deployment-7665d59fc-l8lhl:/# ls /data
root@nginx-deployment-7665d59fc-l8lhl:/#
```
