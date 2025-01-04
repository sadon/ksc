# What can be

Containers
enforced:

- remove bash
- make FS read-only
- run as non root

---

Useful command for StartupProbe:
`chmod a-w -R /`


### SecurityContext
ReadOnly

spec.container.securityContext:
 readOnlyRootFileSystem: true

in additional better to configure
`emptyDir: {}`
to /some-logs-dir


RBAC - do not allow basic user to edit pod specs
