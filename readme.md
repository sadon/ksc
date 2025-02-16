
# KSC preparaion


- [Kubernetes Service Account](./sa.md)
- [API request](./allow.md)
- [Secrets](./secret.md)
- [cata-containers](./cata.md)
- [mTLS](./mtls.md)
- [OPA](./opa.md)
- [footprint](./footprint.md)
- [kubesec](./kubesec.md)
- [scanning](./scan.md)
- [supply-chain](./supply-chain.md)
- [runtime-analyse](./runtime-analyse.md)
- [immutability](./immutability.md)
- [auditing](./auditing.md)
- [kernel-hardering](./kernel.md)
- [reduce-attack-surface](./reduce-attack-surface.md)

Simulator
killer.sh 
https://killercoda.com/killer-shell-cks/scenario/apiserver-crash


Page for exam
- [change encrypt](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/#write-an-encryption-configuration-file)
- - Don't forget mount /volume + VolumeMount 
- - use `echo -n` **-n** to generate valid secret key
- - check errors by 'crictl ps' + api server manifest
- - reencrypt `kubectl get secrets -n your-namespace -o json | kubectl replace -f -`
- [admission-controller](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#imagepolicywebhook)
- - config files are personal and mounts _inside_ container
- - Special config
```commandline
...
kind: AdmissionConfiguration
...
```
- - Custom format: [Config](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#imagereview-config-file-format), 
especially server: 
```commandline
 server: https://images.example.com/policy # URL of remote service to query. Must use 'https'.
```