If you run into the error, either dirctly though helm, or via some CD tools, look here

## Step 1
run `helm list --all-namespaces`, the deployment might've failed, in which case it'll show up here

## Step 2 (if not production)
If downtime isn't an issue, in an env like QA/development, do `helm uninstall -n <namespace> <release name>`, release name and namespace will be found above

## Step 2 (if production)
If in production, find the deployed chart from the command in step 1, and change the secret, via
`kubectl -n <namespace> patch secret <release-name>.<version> --type=merge -p '{"metadata":{"labels":{"status":"deployed"}}}'`
