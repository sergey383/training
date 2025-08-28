## How to install adminer 

for reference: https://www.adminer.org/

Create Namespace: 
```
kubectl apply -f ns.yaml
```

To install adminer:
```
kubectl apply -f adminer-deployment.yaml
```
```
kubectl apply -f adminer-service.yaml
```

```
kubectl port-forward svc/adminer 8080:8080 -n adminer
```

adminer UI:

```
http://localhost:8080
```


# Connect to DB service :

PostgreSQL:
```
postgres.web-app.svc.cluster.local
```

pattern:
service_name.namespace.svc.cluster.local







