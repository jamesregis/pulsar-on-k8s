# Install pulsar and pulsar-manager on Kubernetes

All resources to deploy Apache Pulsar and Pulsar manager on Kubernetes

## Install pulsar

To install pulsar you should :

###  Create a namespace

`kubectl create namespace sn-pulsar`


### Apply all pulsar install resources in the namespace

`kubectl apply pulsar/*.yml -n sn-pulsar`

###  Check that everything is up and running

```bash
[root@k8s-node01 streamnative]# kubectl get pods -n sn-pulsar
NAME                                      READY   STATUS      RESTARTS   AGE
bookie-bk-0                               1/1     Running     0          47h
bookie-bk-1                               1/1     Running     0          47h
bookie-bk-2                               1/1     Running     0          46h
bookie-bk-auto-recovery-0                 1/1     Running     0          32d
broker-broker-0                           1/1     Running     2          47h
broker-broker-1                           1/1     Running     2          47h
propxy-proxy-0                            1/1     Running     0          32d
zookeeper-zk-0                            1/1     Running     0          47h
zookeeper-zk-1                            1/1     Running     0          2d
zookeeper-zk-2                            1/1     Running     0          33d\
```

## Install pulsar-mamager

### Change user/password for PostgreSQL
Don't forget to modify the POSGRESQL (user/password) in the following files:

`vim pulsar-manager/pulsar-manager-application-properties-secret.yml`

`vim pulsar-manager/pulsar-manager-secret.yml` <-- password and username should be `base64` encoded string.

### Create postgres secret

`kubectl apply -f pulsar-manager-postgres-secret -n sn-pulsar`

### Create postgresql database

`kubectl apply -f pulsar-manager/postgres*.yml -n sn-pulsar`

### Initialize the database

`kubectl apply pulsar-manager/initialize-pulsar-manager-db.yml -n sn-pulsar`

### Install pulsar-manager

`kubectl apply pulsar-manager/pulsar-manager*.yml -n sn-pulsar`
###  Check that everything is up and running
```bash
[root@k8s-node01 streamnative]# kubectl get pods -n sn-pulsar
NAME                                      READY   STATUS      RESTARTS   AGE
bookie-bk-0                               1/1     Running     0          47h
bookie-bk-1                               1/1     Running     0          47h
bookie-bk-2                               1/1     Running     0          46h
bookie-bk-auto-recovery-0                 1/1     Running     0          32d
broker-broker-0                           1/1     Running     2          47h
broker-broker-1                           1/1     Running     2          47h
postgres-pulsar-manager-d7949b8b9-cb7zq   1/1     Running     0          14d
propxy-proxy-0                            1/1     Running     0          32d
pulsar-db-init-6wdxm                      0/1     Completed   0          14d
pulsar-manager-76d6f7c8cf-wld9b           1/1     Running     0          4h22m
zookeeper-zk-0                            1/1     Running     0          47h
zookeeper-zk-1                            1/1     Running     0          2d
zookeeper-zk-2                            1/1     Running     0          33d
```