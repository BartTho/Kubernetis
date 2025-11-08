# Service

- Services gebruiken we als interne load balancer tussen replica's
- Via pod labels weet kubernetes welke pod moet bedient worden
- types:
    - ClusterIP: Intern voor de Cluster
    - NodePort: Luisterd op elke Cluster node
    - LoadBalancer: Voor externe load balancers

![Deployments.png](https://git.barttho.be/bart/kubernetis/-/blob/03c59b6c215f09b0c23eb37044ce2d216a8885b4/Afbeeldingen/Deployments.png)

- [ ] We controleren dit


- [ ] Maak de namespace aan en maak het de standaard namespace
```
kubectl apply -f Namespace.yaml
kubens 05--service
```
namespace 05--service created
✔ Active namespace is "05--service"

- [ ] De implementatieconfiguratie toepassen, en controleren
```
kubectl apply -f Deployment.yaml
kubectl get pods
```

NAME                             READY   STATUS    RESTARTS   AGE
nginx-minimal-6b7d669b69-8c2qv   1/1     Running   0          17s
nginx-minimal-6b7d669b69-xmnzc   1/1     Running   0          17s
nginx-minimal-6b7d669b69-zkzh5   1/1     Running   0          18s

- [ ] De ClusterIP-service toepassen
- [ ] De NodePort-service toepassen
- [ ] De LoadBalancer-service toepassen
```
kubectl apply -f Service.nginx-clusterip.yaml
kubectl apply -f Service.nginx-nodeport.yaml
kubectl apply -f Service.nginx-loadbalancer.yaml
```

service/nginx-clusterip created
service/nginx-nodeport created
service/nginx-loadbalancer created

- [ ] Controleren
```
kubectl get Service
# ofwel
# kubectl get svc
```
NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
nginx-clusterip      ClusterIP      10.96.233.163   <none>        80/TCP         10m
nginx-loadbalancer   LoadBalancer   10.96.239.138   192.168.1.100 80:30755/TCP   10m
nginx-nodeport       NodePort       10.96.98.239    <none>        80:32082/TCP   10m

```
```
```
```
```
```
```

  06-delete-namespace:
    desc: "Delete the namespace to clean up"
    cmds:
      - cmd: gum style "🚨 Deleting the namespace recursively deletes the resources inside of it! 🚨 "
        silent: true
      - kubectl delete -f Namespace.yaml
