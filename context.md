#SecurityContext 

pod.spec.SecurityContext 
OR
pod.spec.containers.SecurityContext 


https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
```commandline
securityContext
  runAsUser: 1000
  runAsGroup: 1000
  fsGroup: 2000
```


```commandline
securityContext
  runAsNonRoot: true
 ```

## As root
eq = docker run --privileged (uid=0)
Allow to change host params, like a hostname etc
```commandline
securityContext
  priviliged: true
 ```
## AllowPriviledgeEscalation: false
AllowPriviledgeEscalation: true #default

