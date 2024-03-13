# Lab 04 - backup & restore ETCD

## Documentation

* https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/ (search: etcdctl)

## Step 1 - Retrieving value from ETCD

```
ETCDCTL_API=3 etcdctl get cluster.name \
--endpoints=https://10.0.1.101:2379 \
--cacert=/home/cloud_user/etcd-certs/etcd-ca.pem \
--cert=/home/cloud_user/etcd-certs/etcd-server.crt \
--key=/home/cloud_user/etcd-certs/etcd-server.key
```

## Step 2 - Backup ETCD

```
ETCDCTL_API=3 etcdctl snapshot save etcd_backup.db \
--endpoints=https://10.0.1.101:2379 \
--cacert=/home/cloud_user/etcd-certs/etcd-ca.pem \
--cert=/home/cloud_user/etcd-certs/etcd-server.crt \
--key=/home/cloud_user/etcd-certs/etcd-server.key
```

## Step 3 - Verify ETCD backup

```
ETCDCTL_API=3 etcdctl --write-out=table snapshot status etcd_backup.db
```

## Step 4 - Remove ETCD data

```
sudo systemctl stop etcd
sudo rm -rf /var/lib/etcd/
```

## Step 5 - Restore ETCD backup

```
ETCDCTL_API=3 sudo etcdctl --data-dir /var/lib/etcd/ snapshot restore etcd_backup.db
```

```
sudo chown -R etcd:etcd /var/lib/etcd/
```

```
sudo systemctl start etcd
```

## Step 6 - Retrieving value from ETCD

```
ETCDCTL_API=3 etcdctl get cluster.name \
--endpoints=https://10.0.1.101:2379 \
--cacert=/home/cloud_user/etcd-certs/etcd-ca.pem \
--cert=/home/cloud_user/etcd-certs/etcd-server.crt \
--key=/home/cloud_user/etcd-certs/etcd-server.key
```