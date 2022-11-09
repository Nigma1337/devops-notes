Here are a couple of tools I use daily for managing kubernetes.

### [kubectx + kubens](https://github.com/ahmetb/kubectx)

This package installs these tools:
- kubectx
This lets you change between contexts (or clusters as one might call them), via either doing kubectx and selecting the context or kubectx NAME

- kubens
This lets you change namespaces within a context, same syntax as above

- kubeon + kubeoff
These let you toggle the extra info in ps1, pass -g if you want it to persist

### [stern](https://github.com/wercker/stern)

Easily tail multiple pods, what's not to like?
matches regex, pog

### [krew](https://krew.sigs.k8s.io/)

Plugin manager for kubectl, i've used it to install minio and rabbitmq operators

### [helm](https://helm.sh/)

Helm charts are a super easy way to distribute/download full scale deployments (with services n such often), and are easily customizable via passing a values.yml file