## Secrets

all stores in etcd


mounts as 
- env variable
- as fsmount

Let's check

```commandline
crictl get pods
crictl inspect 21d895598fb9f

[root@deckhouse-master-node-01 ~]# crictl inspect 21d895598fb9f | grep pid
    "pid": 151464,
            "type": "pid",
            "path": "/proc/151154/ns/pid"

[root@deckhouse-master-node-01 ~]#ps aux | grep 
 tree /proc/151464/root/
 ...somethere is secret store...
```

# Watch etcd params
```commandline
 cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep etcd                                   - --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
    - --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
    - --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
      --etcd-servers=https://127.0.0.1:2379,https://10.28.4.211:2379,https://10.28.4.212:2379,https://10.28.4.213:2379


[root@deckhouse-master-node-01 root]# ETCDCTL_API=3 etcdctl --cert=/etc/kubernetes/pki/apiserver-etcd-client.crt --cacert=/etc/kubernetes/pki/etcd/ca.crt --key=/etc/kubernetes/pki/apiserver-etcd-client.key endpoint health
127.0.0.1:2379 is healthy: successfully committed proposal: took = 1.77573ms

```


# Store safely:

https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

api-server flag `--encryption-provider-config`
`kind: EncryptionConfigureation`

--order is make sense: 


Try to read through etcd-client

