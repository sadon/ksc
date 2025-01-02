# strace

example:

```commandline
[root@deckhouse-master-node-01 tmp]# strace -cw ls
mc-root  vmware-root_844-2688685076
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 23.12    0.000663          24        27           mmap
 15.11    0.000433          24        18           mprotect
 10.86    0.000311         311         1           execve
  8.92    0.000256          25        10           open
  8.71    0.000250          19        13           close
  7.21    0.000207          18        11           fstat
  5.97    0.000171          21         8           read
  3.00    0.000086          21         4         3 access
  2.87    0.000082          41         2           getdents
  2.14    0.000061          30         2           statfs
  1.90    0.000054          18         3           brk
  1.80    0.000052          25         2           munmap
  1.40    0.000040          20         2           ioctl
  1.22    0.000035          17         2           rt_sigaction
  0.90    0.000026          25         1           write
  0.85    0.000024          24         1           rt_sigprocmask
  0.72    0.000021          20         1           set_tid_address
  0.71    0.000020          20         1           openat
  0.71    0.000020          20         1           stat
  0.64    0.000018          18         1           getrlimit
  0.63    0.000018          18         1           arch_prctl
  0.59    0.000017          16         1           set_robust_list
------ ----------- ----------- --------- --------- ----------------
100.00    0.002867                   113         3 total

```


# /proc directory
```commandline
ab21045ce39d       etcd-deckhouse-master-node-01
[root@deckhouse-master-node-01 tmp]# ps aux | grep etcd
root       41767  0.0  0.0 112812   980 pts/1    S+   22:55   0:00 grep --color=auto etcd
root      152012 13.5  5.7 12336012 697292 ?     Ssl   2024 26359:38 etcd --advertise-cli...



 strace -cw -p 152012 -f

strace: Process 152490 detached
strace: Process 152491 detached
strace: Process 152492 detached
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 79.64   23.215043        3143      7384       746 futex
 15.74    4.587680         900      5094         3 epoll_pwait
  3.33    0.971452         136      7096           nanosleep
  0.55    0.159134        1007       158           fdatasync
  0.38    0.111809          68      1640           write
  0.24    0.069696          35      1987       951 read
  0.03    0.009793          36       272           pwrite64
  0.02    0.005346          35       151           lseek
  0.01    0.002923          38        76           tgkill
  0.01    0.002344          30        76         4 rt_sigreturn
  0.01    0.002309          32        70           sched_yield
  0.01    0.002295          30        76           getpid
  0.00    0.001346          24        56           fcntl
  0.00    0.001335          51        26           getrandom
  0.00    0.001296          29        44           setsockopt
  0.00    0.000915          30        30        14 epoll_ctl
  0.00    0.000696          31        22           close
  0.00    0.000680         679         1         1 restart_syscall
  0.00    0.000619         154         4         4 connect
  0.00    0.000505          36        14           openat
  0.00    0.000335          83         4           socket
  0.00    0.000250          20        12           fstat
  0.00    0.000247          30         8         4 accept4
  0.00    0.000244          30         8           getsockname
  0.00    0.000131          32         4           getsockopt
  0.00    0.000126          31         4           getpeername
  0.00    0.000101          25         4           getdents64
------ ----------- ----------- --------- --------- ----------------
100.00   29.148653                 24321      1727 total

```

## find open files

```commandline
cd /proc/152012/fd

[root@deckhouse-master-node-01 fd]# ls -lah ./ | grep db
lrwx------. 1 root root 64 Dec 30 10:23 10 -> /var/lib/etcd/member/snap/db

if etcd uncenryped:

kubectl -n ddubov create secret generic test-waekness4 --from-literal=sad=111222333444 # --dry-run=client -oyaml
tail 10 -n 5000 |strings | grep 111222333444
if etcd uncenryped: it will works 

 
```


## Check host's secrets
```commandline
[root@deckhouse-master-node-01 ddubovitskiy]# crictl ps | grep apac
b7ee09d37a99e       4ce47c750a586       About a minute ago   Running             apache                                    0                   23c7b30de0df8       apache




[root@deckhouse-master-node-01 ddubovitskiy]# cat ./apache.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: apache
  name: apache
  namespace: ddubov
spec:
  nodeName: deckhouse-worker-node-1
  containers:
  - image: httpd
    name: apache
    resources: {}
    command: ['/bin/sh', '-c', 'sleep 1d']
    env:
     - name: SECRET
       value: "436457657"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}


Worker Node
[root@deckhouse-worker-node-1 ~]# pstree -p
[root@deckhouse-worker-node-1 ~]# pstree -p | grep sleep
           |                         |-sh(701309)---sleep(701321)



[root@deckhouse-worker-node-1 ~]# cat /proc/701309/environ
PATH=/usr/local/apache2/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/binHOSTNAME=apacheHTTPD_PREFIX=/usr/local/apache2HTTPD_VERSION=2.4.62HTTPD_SHA256=674188e7bf44ced82da8db522da946849e22080d73d16c93f7f4df89e25729ecHTTPD_PATCHES=SECRET=436457657TEST_PVC_RESTORE_PORT_444_TCP_PROTO=tcpTEST_PVC_RESTORE_PORT_444_TCP_ADDR=10.222.214.106PLANETAPLF_433_SERVICE_HOST=10.222.16.118PLANETAPLF_433_PORT=tcp://10.222.16.118:80MY_NGINX_SERVICE_3_PORT_80_TCP_PROTO=tcpKUBERNETES_SERVICE_PORT_HTTPS=443KUBERNETES_PORT=tcp://10.222.0.1:443TEST_PVC_RESTORE_PORT_444_TCP_PORT=444KUBERNETES_SERVICE_HOST=10.222.0.1KUBERNETES_PORT_443_TCP_PROTO=tcpTEST_PVC_RESTORE_SERVICE_HOST=10.222.214.106PLANETAPLF_433_PORT_80_TCP_PORT=80KUBERNETES_SERVICE_PORT=443GUESTBOOK_UI_PORT_80_TCP_ADDR=10.222.201.193PLANETAPLF_433_PORT_80_TCP_PROTO=tcpMY_NGINX_SERVICE_3_SERVICE_PORT=80MY_NGINX_SERVICE_3_PORT=tcp://10.222.56.174:80MY_NGINX_SERVICE_3_PORT_80_TCP_PORT=80KUBERNETES_PORT_443_TCP_ADDR=10.222.0.1GUESTBOOK_UI_PORT_80_TCP_PROTO=tcpGUESTBOOK_UI_PORT_80_TCP_PORT=80PLANETAPLF_433_SERVICE_PORT=80PLANETAPLF_433_PORT_80_TCP=tcp://10.222.16.118:80KUBERNETES_PORT_443_TCP=tcp://10.222.0.1:443KUBERNETES_PORT_443_TCP_PORT=443TEST_PVC_RESTORE_SERVICE_PORT=444GUESTBOOK_UI_PORT=tcp://10.222.201.193:80MY_NGINX_SERVICE_3_SERVICE_HOST=10.222.56.174TEST_PVC_RESTORE_PORT=tcp://10.222.214.106:444TEST_PVC_RESTORE_PORT_444_TCP=tcp://10.222.214.106:444GUESTBOOK_UI_SERVICE_HOST=10.222.201.193GUESTBOOK_UI_SERVICE_PORT=80GUESTBOOK_UI_PORT_80_TCP=tcp://10.222.201.193:80PLANETAPLF_433_PORT_80_TCP_ADDR=10.222.16.118MY_NGINX_SERVICE_3_PORT_80_TCP=tcp://10.222.56.174:80MY_NGINX_SERVICE_3_PORT_80_TCP_ADDR=10.222.56.174HOME=/root

[root@deckhouse-worker-node-1 ~]# cat /proc/701309/environ  | strings | grep 436457657
SECRET=436457657
[root@deckhouse-worker-node-1 ~]#

...


```


## Falco


