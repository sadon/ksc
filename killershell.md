# Difficult  questions

### falco
falco -U | grep httpd

catch containers based on rules


### get secret without permissions:
 - check mounted secrets in running pods
 - check created serviceaccounts with prepared tokens
 - - creat api requests to read the secret(s) using SA credentials

### check KILL syscalls
- crictl collect id of conatainers
crictl inspect
- - find pid on the host machine
- - `strace -p <host-proc-id>`
```commandline
ssh cks7262-node1
```

### Secrets store 
 - do not store  secret in logs etc
 - do not store logs inside containers layers


### Cillium policy
example see: `egress` vs `egressDeny` field

```commandline
apiVersion: "cilium.io/v2"
kind: CiliumNetworkPolicy
metadata:
  name: policy1
  namespace: my-tead
spec:
  endpointSelector:
    matchLabels:
      type: mes
  egressDeny:
  - toEndpoints:
    - matchLabels:
        type: data
```

Examples L3

https://docs.cilium.io/en/stable/security/policy/language/#limit-icmp-icmpv6-types

#### enable cillium mutual authentification 
- https://docs.cilium.io/en/stable/network/servicemesh/mutual-authentication/mutual-authentication-example/#enforce-mutual-authentication

add to egress/ingress field
````commandline
authentication:
    mode: "required"
````


## kubelet config
additional store
During cluster creation and upgrade, kubeadm writes its KubeletConfiguration in a ConfigMap called kubelet-config in the kube-system namespace.

```commandline
kubectl edit cm -n kube-system kubelet-config
```

