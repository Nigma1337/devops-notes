If you're getting a message mentioning a missing csi driver, you can check which drivers there are on your nodes via 

```bash
kubectl get nodes -o jsonpath="{range .items[*]}{.metadata.name}{\"\t\"}{@.metadata.annotations.csi\.volume\.kubernetes\.io/nodeid}{\"\n\"}{end}"
```
To just get the ones missing your driver, do

```bash
kubectl get nodes -o jsonpath="{range .items[*]}{.metadata.name}{\"\t\"}{@.metadata.annotations.csi\.volume\.kubernetes\.io/nodeid}{\"\n\"}{end}" | grep -v <driver name> | awk '{print $1}'
```

Now we need to find the pods responsible for registering this driver. They're usually, in my expierence, deployed as Daemonsets with CSI in their name
```bash
NODES=$(kubectl get nodes -o jsonpath="{range .items[*]}{.metadata.name}{\"\t\"}{@.metadata.annotations.csi\.volume\.kubernetes\.io/nodeid}{\"\n\"}{end}"| awk '{print $1}')
for node in $NODES; do kubectl get pods --all-namespaces -o jsonpath="{range .items[*]}{.metadata.namespace}{\"\t\"}{.metadata.name}{\"\n\"}{end}" --field-selector spec.nodeName=$node | grep csi; done
```
Some pod which is alike to the driver should show up, lets say csi-cephfsplugin was bullying me, now i'd run this command, putting in the namespace i got above
```bash
export NAMESPACE="<namespace here>"
for node in $NODES; do kubectl get pods -n $NAMESPACE -o jsonpath="{range .items[*]}{.metadata.name}{\"\n\"}{end}" --field-selector spec.nodeName=$node | grep csi-cephfsplugin | xargs kubectl delete -n $NAMESPACE ; done
```