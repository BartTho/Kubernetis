# Service

- Services gebruiken we als interne load balancer tussen replica's
- Via pod labels weet kubernetes welke pod moet bedient worden
- types:
    - ClusterIP: Intern voor de Cluster
    - NodePort: Luisterd op elke Cluster node
    - LoadBalancer: Voor externe load balancers

![Deployments.png](https://git.barttho.be/bart/kubernetis/-/blob/03c59b6c215f09b0c23eb37044ce2d216a8885b4/Afbeeldingen/Deployments.png)

- [ ] We controleren dit

```
