## Kubernetes Service Account

To disable Automount secrets:

service-account.spec
pod.spec:
spec.serviceAccountName
spec.automountServiceAccountToken: false

OR 
ServiceAccount
spec.automountServiceAccountToken: false

Mounts directory with JWT token 

Links: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/


To communicate with API
kubectl run 
$ 

```commandline
printenv | grep KUBER 
--> kubeapiurl: 10.222.0.1
root@accessor:/# curl https://10.222.0.1:443 -k
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}root@accessor:/# exit
---

token: /run/secrets/kubernetes.io/serviceaccount/token

curl https://10.222.0.1:443 -k -H "Authorization: Bearer XXX"

{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:serviceaccount:ddubov:accessor\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403

```