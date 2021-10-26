Instead of passing -n NAMESPACE whenever you run kubectl, set a default namespace via


`kubectl config set-context --current --namespace=NAMESPACE`


this sets `contexts.context.namespace` to our namespace, which will then use it as a default.
