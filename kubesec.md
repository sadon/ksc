# kubesec.io

https://github.com/controlplaneio/kubesec

`kubectl run pod --image=nginx --dry-run=client -oyaml > pod.yaml`

Example usage:
```commandline
$ docker run -i kubesec/kubesec:v2 scan /dev/stdin < pod.yaml
```
Will give suggest


## Conftest

https://github.com/killer-sh/cks-course-environment/tree/master/course-content/supply-chain-security/static-analysis/conftest/kubernetes


```
docker run --rm -v $(pwd):/project openpolicyagent/conftest test deploy.yaml
```

Check REGO language
```commandline
# from https://www.conftest.dev
package main

deny[msg] {
  input.kind = "Deployment"
  not input.spec.template.spec.securityContext.runAsNonRoot = true
  msg = "Containers must not run as root"
}

deny[msg] {
  input.kind = "Deployment"
  not input.spec.selector.matchLabels.app
  msg = "Containers must provide app label for pod selectors"
}
```


## Dockerfile
https://github.com/killer-sh/cks-course-environment/blob/master/course-content/supply-chain-security/static-analysis/conftest/docker/policy/commands.rego
Example 
```commandline
# from https://www.conftest.dev
package main

denylist = [
  "ubuntu"
]

deny[msg] {
  input[i].Cmd == "from"
  val := input[i].Value
  contains(val[i], denylist[_])

  msg = sprintf("unallowed image found %s", [val])
}
```

Command section
```commandline
# from https://www.conftest.dev

package commands

denylist = [
  "apk",
  "apt",
  "pip",
  "curl",
  "wget",
]

deny[msg] {
  input[i].Cmd == "run"
  val := input[i].Value
  contains(val[_], denylist[_])

  msg = sprintf("unallowed commands found %s", [val])
}
```