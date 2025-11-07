- We gaan replicas maken
- We doen dit door gebruik te maken van yaml-bestanden
- Labels vormen de link tussen de Replicaset en de pod

![ReplicaSet.png](https://git.barttho.be/bart/kubernetis/-/blob/34428fae981d0732766d65f696559294cb8222b4/Afbeeldingen/ReplicaSet.png)

- [ ] Namespace aanmaken
- [ ] Als standaard namespace instellen
- [ ] Replicas aanmaken, Deze replica zal 3 pods starten
- [ ] We controleren dit

kubens ${NAMESPACE} 
```
kubectl apply -f Namespace.yaml
kubens ${NAMESPACE}
kubectl apply -f ReplicaSet.nginx-minimal.yaml
kubectl get pods
```
NAME                  READY   STATUS    RESTARTS   AGE
nginx-minimal-7mst7   1/1     Running   0          2m35s
nginx-minimal-k8vlc   1/1     Running   0          2m35s
nginx-minimal-pv74t   1/1     Running   0          2m35s

```
kubectl get replicasets
```
NAME            DESIRED   CURRENT   READY   AGE
nginx-minimal   3         3         3       3m27s

```
kubectl get rs
```
NAME            DESIRED   CURRENT   READY   AGE
nginx-minimal   3         3         3       3m33s

"kubectl get rs" is synoniem van "kubectl get replicasets"

Wat gebeurt er als een pod nu stop (crached)?
Laten we dit symuleren door 1 pod te stoppen

- [ ] Stop een pod (gebruik de naam van de pod)
ik stop de pod met de naam "nginx-minimal-7mst7" zie hierboven get pods
kubectl delete pod NAAM_VAN_DE_POD

```
kubectl delete pod nginx-minimal-7mst7
kubectl get pods
 ```
pod "nginx-minimal-7mst7" deleted from default namespace
NAME                  READY   STATUS    RESTARTS   AGE
nginx-minimal-gn6p5   1/1     Running   0          55s
nginx-minimal-k8vlc   1/1     Running   0          27m
nginx-minimal-pv74t   1/1     Running   0          27m

We zien dat de replicator een nieuwe pod (nginx-minimal-gn6p5) heeft gestart

