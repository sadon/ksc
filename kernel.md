
# Kernel hardering

Container isolation

- Namespaces
- CGroups


---
Main tools 
- appArmor
- seccomp

## AppArmor
 - FS
 - processes
 - Network

Create AppArmor's profile
 - modes:
 - - disable
 - - enable, but log it
 - - enable

```commandline
controlplane $ aa-status 
apparmor module is loaded.
37 profiles are loaded.
37 profiles are in enforce mode.
   /snap/snapd/23258/usr/lib/snapd/snap-confine
   /snap/snapd/23258/usr/lib/snapd/snap-confine//mount-namespace-capture-helper
   /usr/bin/man
   /usr/lib/NetworkManager/nm-dhcp-client.action
   /usr/lib/NetworkManager/nm-dhcp-helper
   /usr/lib/connman/scripts/dhclient-script
   /usr/lib/cups/backend/cups-pdf
   /usr/lib/lightdm/lightdm-guest-session
   /usr/lib/lightdm/lightdm-guest-session//chromium
```


```commandline
controlplane $ curl killer.sh
OK+++

controlplane $ apt-get install apparmor-utils

controlplane $ aa-genprof curl
F
Finished generating profile for /usr/bin/curl.
controlplane $ curl killer.sh
curl: (6) Could not resolve host: killer.sh

controlplane $ aa-status 
apparmor module is loaded.
38 profiles are loaded.
38 profiles are in enforce mode.
   /snap/snapd/23258/usr/lib/snapd/snap-confine
   /snap/snapd/23258/usr/lib/snapd/snap-confine//mount-namespace-capture-helper
   /usr/bin/curl
   
controlplane $ cat /etc/apparmor.d/usr.bin.curl 
# Last Modified: Sat Jan  4 20:35:50 2025
#include <tunables/global>

/usr/bin/curl {
  #include <abstractions/base>

  /usr/bin/curl mr,

}
controlplane $ aa-logprof
...

```


Example https://github.com/killer-sh/cks-course-environment/blob/master/course-content/system-hardening/kernel-hardening-tools/apparmor/profile-docker-nginx

Definitely used in exam
https://kubernetes.io/docs/tutorials/security/apparmor/#example


Typical usage:
```commandli ne
spec:
  securityContext:
    appArmorProfile:
      type: Localhost
      localhostProfile: k8s-apparmor-example-deny-write
```

eq. to:


`docker run --security-opt apparmor=docker-default containername`

apparmor have to be installed on the every node


## apparmor & K8s
used annotation to apply on the pods on nodes




# Seccomp

include in Linux kernel 2.7+

and can restrict access on certain syscals

Full OSI spec https://github.com/opencontainers/runtime-spec/blob/f329913/config-linux.md#seccomp

- don't allow to run `getPid` for example

BPF Filters

Profile example
https://github.com/killer-sh/cks-course-environment/blob/master/course-content/system-hardening/kernel-hardening-tools/seccomp/profile-docker-nginx.json
`docker run --security-opt seccomp=docker-nginx containername`

Use
https://kubernetes.io/docs/tutorials/security/seccomp/#create-a-pod-that-uses-the-container-runtime-default-seccomp-profile

````commandline
apiVersion: v1
kind: Pod
metadata:
  name: default-pod
  labels:
    app: default-pod
spec:
  securityContext: #add
    seccompProfile: #add
      type: RuntimeDefault #add
  containers:
  - name: test-container
    image: hashicorp/http-echo:1.0
    args:
    - "-text=just made some more syscalls!"
    securityContext:
      allowPrivilegeEscalation: false
````

Seccomp profiles stores as json files
https://kubernetes.io/docs/tutorials/security/seccomp/#download-profiles
it can be downloaded and installed 

```commandline
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
```

or set it up globally
