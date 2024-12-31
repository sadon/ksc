# Open Policy Agent (OPA)

https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/

Why?
 Ex. 
 - has the pod REQUIRED labels?
 - namesapaces must hava label **y**


_Rego_ language

Good examples here: https://github.com/BouweCeunen/gatekeeper-policies
Playground: https://play.openpolicyagent.org/


JSON/YAML support

Admission Controller 

Ex. OPA Gatekeeper:

CRD:

```commandline
[root@deckhouse-master-node-01 ~]# kubectl api-resources | grep templates.gatekeeper.sh/v1
constrainttemplates                                       templates.gatekeeper.sh/v1                 false        ConstraintTemplate

K8srequiredlabels                                          constraints.gatekeeper.sh/v1beta1          false        D8RequiredLabels

#deckhouse
d8requiredlabels                                          constraints.gatekeeper.sh/v1beta1          false        D8RequiredLabels

```


Examples: 
- alwaysPull https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#alwayspullimages
- Default Storage Class https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#defaultstorageclass 

## Types:

- Validatation
- Mutating

## Deny all example
https://github.com/killer-sh/cks-course-environment/tree/master/course-content/opa/deny-all

1. Ctrate template with REGO language
```commandline
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8salwaysdeny
spec:
  crd:
    spec:
      names:
        kind: K8sAlwaysDeny
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          properties:
            message:
              type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8salwaysdeny

        violation[{"msg": msg}] {
          1 > 0
          msg := input.parameters.message
        }
```

2. Create CRD with targets:
```commandline
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAlwaysDeny
metadata:
  name: pod-always-deny
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
  parameters:
    message: "ACCESS DENIED!"
```
3. Check
```
k run pod --image=nginx
FAILED >>
```

4. Detailed
````commandline
kubectl describe K8sAlwaysDeny pod-always-deny
````

Check STATUS fields (13 Violations) include explanation
 
---

## Require labels
Example 
```commandline
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
spec:
  crd:
    spec:
      names:
        kind: K8sRequiredLabels
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          properties:
            labels:
              type: array
              items: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8srequiredlabels

        violation[{"msg": msg, "details": {"missing_labels": missing}}] {
          provided := {label | input.review.object.metadata.labels[label]}
          required := {label | label := input.parameters.labels[_]}
          missing := required - provided
          count(missing) > 0
          msg := sprintf("you must provide labels: %v", [missing])
        }
```