Renovate supports updating Helm Chart references and git references in vendir.yml via the [vendir](https://carvel.dev/vendir/) tool. Renovate requires the presence of a [vendir lock file](https://carvel.dev/vendir/docs/v0.40.x/vendir-lock-spec/) which is generated by vendir and should be stored in source code.

### Helm Charts

It supports both https and oci helm chart repositories.

```yaml title="Example helm chart vendir.yml"
apiVersion: vendir.k14s.io/v1alpha1
kind: Config

# one or more directories to manage with vendir
directories:
  - # path is relative to `vendir` CLI working directory
    path: config/_ytt_lib
    contents:
      path: github.com/cloudfoundry/cf-k8s-networking
      helmChart:
        # chart name (required)
        name: stable/redis
        # use specific chart version (string; optional)
        version: '1.2.1'
        # specifies Helm repository to fetch from (optional)
        repository:
          # repository url; supports exprimental oci helm fetch via
          # oci:// scheme (required)
          url: https://...
        # specify helm binary version to use;
        # '3' means binary 'helm3' needs to be on the path (optional)
        helmVersion: '3'
```

### Registry Aliases

#### OCI

Aliases for OCI registries are supported via the dockerfile/docker manager

### Git

Renovates supporting explicit refs in for git references in vendir.yml

```yaml title="Example git vendir.yml"
apiVersion: vendir.k14s.io/v1alpha1
kind: Config

# one or more directories to manage with vendir
directories:
  - path: config/_ytt_lib
    contents:
      path: github.com/cloudfoundry/cf-k8s-networking
      git:
        # http or ssh urls are supported (required)
        url: https://github.com/cloudfoundry/cf-k8s-networking
        # branch, tag, commit; origin is the name of the remote (required)
        # optional if refSelection is specified (available in v0.11.0+)
        ref: origin/master
        # depth of commits to fetch; 0 (default) means everything (optional; v0.29.0+)
        depth: 1
        ...
```