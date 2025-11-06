- [ ] Overzicht van alle kubectl commando's
https://kubernetes.io/docs/reference/kubectl/

#### kubectl explain ?
- [ ] met explane kunen we uitleg vragen van objecten
```
kubectl explain namespace
```
KIND:       Namespace
VERSION:    v1

DESCRIPTION:
    Namespace provides a scope for Names. Use of multiple namespaces is
    optional.
    
FIELDS:
  apiVersion    <string>
    APIVersion defines the versioned schema of this representation of an object.
    Servers should convert recognized schemas to the latest internal value, and
...

- [ ] toon alle bestaande namespace
```
kubectl get namespaces
```
NAME                 STATUS   AGE
default              Active   89m
kube-node-lease      Active   89m
kube-public          Active   89m
kube-system          Active   89m
local-path-storage   Active   89m

### Het namespace cli commando 
We kunnen een namespace maken via het cli commando 
kubectl create namespace namespace-cli
Maar beter is van een namespace te maken via een yaml-bestand.

- [ ] Maak bestand aan Namespace.yaml
```
nano Namespace.yaml
```
Voor waarde van de namespace naam:  
- kleine letters (geen hoofdletters)  
- een minteken is toegelaten  
- start en eindigd op een letter  
- 'mijn-name',  of '123-abc', de validatie regel is '[a-z0-9]([-a-z0-9]*[a-z0-9])  

- [ ] Toevoegen aan Namespace.yaml
```
apiVersion: v1
kind: Namespace
metadata:
  name: mijn_namespace
```
- [ ] Namespace Toevoegen aan cluster
```
kubectl apply -f Namespace.yaml
```




