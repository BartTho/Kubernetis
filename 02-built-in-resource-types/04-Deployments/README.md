# Kubernetes Deployments

## Deployment aanmaken en uitvoeren

- Het concept "rollouts" en "rollbacks"
- Dit gebruiken we voor toepassingen die lang actief moeten blijven

![Deployments.png](https://git.barttho.be/bart/kubernetis/-/blob/03c59b6c215f09b0c23eb37044ce2d216a8885b4/Afbeeldingen/Deployments.png)


- [ ] Namespace aanmaken
- [ ] Wacht 1 seconde zodat de namespace aangemaakt is
- [ ] Als standaard namespace instellen
- [ ] Deployment uitrollen eerst de minimale
- [ ] Deployment uitrollen daarna de betere
- [ ] We controleren dit

```
echo "Aanmaken namespace"
echo "++++++++++++++++++++++++++++++++++++++++++++++"
kubectl apply -f Namespace.yaml
sleep 1
echo "Stel 04--deployment in als standaard namespace"
echo "++++++++++++++++++++++++++++++++++++++++++++++"
kubens 04--deployment
sleep 1
echo "Deploy minimaal"
echo "++++++++++++++++++++++++++++++++++++++++++++++"
kubectl apply -f Deployment.nginx-minimaal.yaml
sleep 1
echo "Deploy beter"
echo "++++++++++++++++++++++++++++++++++++++++++++++"
kubectl apply -f Deployment.nginx-beter.yaml
echo "Toon info"
echo "++++++++++++++++++++++++++++++++++++++++++++++"
kubectl get all
```

namespace/04--deployment created  
Stel 04--deployment in als standaard namespace  
++++++++++++++++++++++++++++++++++++++++++++++  
✔ Active namespace is "04--deployment"  
Deploy minimaal  
++++++++++++++++++++++++++++++++++++++++++++++  
deployment.apps/nginx-minimaal created  
Deploy beter  
++++++++++++++++++++++++++++++++++++++++++++++  
deployment.apps/nginx-beter created  
Toon info  
++++++++++++++++++++++++++++++++++++++++++++++  
NAME                                  READY   STATUS              RESTARTS   AGE  
pod/nginx-beter-545b96d7c4-hcknq      0/1     ContainerCreating   0          0s  
pod/nginx-beter-545b96d7c4-pkdtb      0/1     ContainerCreating   0          0s  
pod/nginx-beter-545b96d7c4-sccxs      0/1     ContainerCreating   0          0s  
pod/nginx-minimaal-85bcb8bcc9-42gtc   0/1     ContainerCreating   0          1s  
pod/nginx-minimaal-85bcb8bcc9-4h6cl   0/1     ContainerCreating   0          1s  
pod/nginx-minimaal-85bcb8bcc9-6qcp9   0/1     ContainerCreating   0          1s  
  
NAME                             READY   UP-TO-DATE   AVAILABLE   AGE  
deployment.apps/nginx-beter      0/3     3            0           0s  
deployment.apps/nginx-minimaal   0/3     3            0           1s  
  
NAME                                        DESIRED   CURRENT   READY   AGE  
replicaset.apps/nginx-beter-545b96d7c4      3         3         0       0s  
replicaset.apps/nginx-minimaal-85bcb8bcc9   3         3         0       1s  
  
We zien dat kubernetes nu voor ons 2 replicasets heeft aangemaakt, één voor elke deployment  
Ook merken we op dat nog niet alle pods gestart zijn.  
  
- [ ] Wacht enkele seconden en voer het commando get pods opnieuw uit.  
```
kubectl get pods
``` 

NAME                              READY   STATUS    RESTARTS   AGE  
nginx-beter-545b96d7c4-hcknq      1/1     Running   0          13s  
nginx-beter-545b96d7c4-pkdtb      1/1     Running   0          13s  
nginx-beter-545b96d7c4-sccxs      1/1     Running   0          13s  
nginx-minimaal-85bcb8bcc9-42gtc   1/1     Running   0          14s  
nginx-minimaal-85bcb8bcc9-4h6cl   1/1     Running   0          14s  
nginx-minimaal-85bcb8bcc9-6qcp9   1/1     Running   0          14s  
  
Nu zijn alle pods actief  

## Deployments aanpassen
We voeren een "rollouts" en "rollbacks" uit.

- [ ] Pas het bestand Deployment.nginx-beter.yaml aan.
spec:  
  replicas: 1  

- [ ] Bewaar het bestand
- [ ] Voer onderstaande code up om de aanpassing door te voeren -> rollout
```
kubectl apply -f Deployment.nginx-beter.yaml
kubectl get pods
echo "++++++++++++++++++++++++++++++++++++++++++++++"
sleep 2
kubectl get pods
```

deployment.apps/nginx-beter configured  
NAME                              READY   STATUS        RESTARTS   AGE  
nginx-beter-545b96d7c4-hcknq      1/1     Terminating   0          3m38s  
nginx-beter-545b96d7c4-pkdtb      1/1     Running       0          3m38s  
nginx-beter-545b96d7c4-sccxs      1/1     Terminating   0          3m38s  
nginx-minimaal-85bcb8bcc9-42gtc   1/1     Running       0          3m39s  
nginx-minimaal-85bcb8bcc9-4h6cl   1/1     Running       0          3m39s  
nginx-minimaal-85bcb8bcc9-6qcp9   1/1     Running       0          3m39s  
++++++++++++++++++++++++++++++++++++++++++++++  
NAME                              READY   STATUS    RESTARTS   AGE  
nginx-beter-545b96d7c4-pkdtb      1/1     Running   0          3m39s  
nginx-minimaal-85bcb8bcc9-42gtc   1/1     Running   0          3m40s  
nginx-minimaal-85bcb8bcc9-4h6cl   1/1     Running   0          3m40s  
nginx-minimaal-85bcb8bcc9-6qcp9   1/1     Running   0          3m40s  

Nu is er nog maar 1 pod actief

- [ ] We hebben een nieuwe versie van onze minimale pod die we willen deployen
- [ ] Pas het bestand Deployment.nginx-minimaal.yaml aan
- [ ] Bewaar het bestand
- [ ] Voer onderstaande code up om de aanpassing door te voeren -> rollout

Deployment.nginx-minimaal.yaml
      containers:
        - name: nginx
          image: nginx:1.28.0

```
kubectl apply -f Deployment.nginx-minimaal.yaml
kubectl get pods
echo "++++++++++++++++++++++++++++++++++++++++++++++"
sleep 2
kubectl get pods
echo "++++++++++++++++++++++++++++++++++++++++++++++"
sleep 2
kubectl get pods
echo "++++++++++++++++++++++++++++++++++++++++++++++"
```

deployment.apps/nginx-minimaal configured  
NAME                              READY   STATUS              RESTARTS   AGE  
nginx-beter-545b96d7c4-pkdtb      1/1     Running             0          7m11s  
nginx-minimaal-7544f9f5b-fq7x5    0/1     ContainerCreating   0          0s  
nginx-minimaal-85bcb8bcc9-42gtc   1/1     Running             0          7m12s  
nginx-minimaal-85bcb8bcc9-4h6cl   1/1     Running             0          7m12s  
nginx-minimaal-85bcb8bcc9-6qcp9   1/1     Running             0          7m12s  
++++++++++++++++++++++++++++++++++++++++++++++  
NAME                              READY   STATUS              RESTARTS   AGE  
nginx-beter-545b96d7c4-pkdtb      1/1     Running             0          7m13s  
nginx-minimaal-7544f9f5b-5dtvq    0/1     ContainerCreating   0          1s  
nginx-minimaal-7544f9f5b-fq7x5    1/1     Running             0          2s  
nginx-minimaal-85bcb8bcc9-42gtc   1/1     Running             0          7m14s  
nginx-minimaal-85bcb8bcc9-4h6cl   1/1     Running             0          7m14s  
nginx-minimaal-85bcb8bcc9-6qcp9   0/1     Completed           0          7m14s  
++++++++++++++++++++++++++++++++++++++++++++++  
NAME                             READY   STATUS    RESTARTS   AGE  
nginx-beter-545b96d7c4-pkdtb     1/1     Running   0          7m23s  
nginx-minimaal-7544f9f5b-5dtvq   1/1     Running   0          11s  
nginx-minimaal-7544f9f5b-fq7x5   1/1     Running   0          12s  
nginx-minimaal-7544f9f5b-rm4h7   1/1     Running   0          9s  

  
"🚨 Wanneer we de naamruimte verwijderen, worden ook de bronnen erin recursief verwijderd! 🚨 "  
- [ ] Verwijder de Namespace

```
kubectl delete -f Namespace.yaml
```

namespace "04--deployment" deleted