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
