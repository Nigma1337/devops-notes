There's a chance you have old, legacy systems, probably left over from before k8s.

We had servers which were unable to do TLS1.2+, so customers couldn't reach the webservers via their browsers. As the setups were rather complex, instead of messing around with setting up a new load balancer to terminate SSL, or upgrading the servers (god forbid), we set up proxies via our new k8s cluster, here's the YAML required:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: some-service
  namespace: web
spec:
  ports:
  - name: app
    port: <some-port>
    protocol: TCP
    targetPort: <some-port>
  clusterIP: None
  type: ClusterIP
---
apiVersion: v1
kind: Endpoints
metadata:
  name: some-service
  namespace: web
subsets:
- addresses:
  - ip: <some ip>
  ports:
  - name: app
    port: some-port
    protocol: TCP
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: external-services
  namespace: web
spec:
  rules:
  - host: <old domain>
    http: 
      paths:
      - backend:
        service:
          name: some-service
          port:
            number: some-port
      pathType: ImplementationSpecific
```

