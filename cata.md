# Cata containers

- cata: emulate small VM
Qemu as default
  -Require  nested virtualization
https://github.com/kata-containers/kata-containers/blob/main/docs/how-to/run-kata-with-k8s.md

- installed as additonal kubelet arguments that directly communicate with containers/cri-o socket (like a proxy)

# gvizor
- gvisor emulate UserSpace 
- - runsc [independent kernel]



runtime class

create runtimeclass: as K8s resource
https://kubernetes.io/docs/concepts/containers/runtime-class/

Set it to the pod:
```commandline
spec:
  runtimeClassName: gvisor

```

install gvisor on working node
https://github.com/killer-sh/cks-course-environment/blob/master/course-content/microservice-vulnerabilities/container-runtimes/gvisor/install_gvisor.sh