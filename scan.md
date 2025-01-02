# Scan for vulnerabilities

## tools:

- Clair
- Trivi


## Trivi
https://github.com/aquasecurity/trivy

```commandline
trivy image python:3.4-alpine
```

```commandline
 kubectl run trivy --image=aquasec/trivy --dry-run=client -o yaml > pod.yaml -- image python
```
is eq to:
`trivy image python`

````commandline
python (debian 12.8)
====================
Total: 1313 (UNKNOWN: 21, LOW: 552, MEDIUM: 626, HIGH: 108, CRITICAL: 6)

┌──────────────────────────────┬─────────────────────┬──────────┬──────────────┬──────────────────────────────┬───────────────┬──────────────────────────────────────────────────────────────┐
│           Library            │    Vulnerability    │ Severity │    Status    │      Installed Version       │ Fixed Version │                            Title                             │
├──────────────────────────────┼─────────────────────┼──────────┼──────────────┼──────────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ apt                          │ CVE-2011-3374       │ LOW      │ affected     │ 2.6.1                        │               │ It was found that apt-key in apt, all versions, do not       │
│                              │                     │          │              │                              │               │ correctly...                                                 │
│                              │                     │          │              │                              │               │ https://avd.aquasec.com/nvd/cve-2011-3374                    │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ bash                         │ TEMP-0841856-B18BAF │          │              │ 5.2.15-2+b7                  │               │ [Privilege escalation possible to other user than root]      │
│                              │                     │          │              │                              │               │ https://security-tracker.debian.org/tracker/TEMP-0841856-B1- │
│                              │                     │          │              │                              │               │ 8BAF                                                         │
├──────────────────────────────┼─────────────────────┤          │              ├──────────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ binutils                     │ CVE-2017-13716      │          │              │ 2.40-2                       │               │ binutils: Memory leak with the C++ symbol demangler routine  │
│                              │                     │          │              │                              │               │ in libiberty                                                 │
│                              │                     │          │              │                              │               │ https://avd.aquasec.com/nvd/cve-2017-13716                   │
│                              ├─────────────────────┤          │              │                              ├───────────────┼──────────────────────────────────────────────────────────────┤

````
