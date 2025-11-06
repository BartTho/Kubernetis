# kubernetis
## Opzetten en werken met kubernetis

## Getting started

To make it easy for you to get started with GitLab, here's a list of recommended next steps.

Already a pro? Just edit this README.md and make it your own. Want to make it easy? [Use the template at the bottom](#editing-this-readme)!

## Installatie
# Docker - Installatie

- [ ] # Add Docker's official GPG key:  
```
sudo apt-get update  
sudo apt-get install ca-certificates curl  
sudo install -m 0755 -d /etc/apt/keyrings  
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc  
sudo chmod a+r /etc/apt/keyrings/docker.asc
``` 

# Add the repository to Apt sources:  
 ```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
``` 

``` 
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin   docker-compose-plugin  
sudo systemctl status docker  
sudo systemctl start docker  
```
# Docker uitvoeren zonder sudo 

Voeg jou gebruiker toe aan de groep Docker:
```
sudo usermod -aG docker $USER
``` 

Log uit en terug in zodat je lid bent van de groep.
Tenslotte herstart het Docker proces.
``` 
sudo systemctl restart docker
``` 
# Docker Commando's
``` 
docker version
``` 

- [ ] https://docs.docker.com/engine/install/ubuntu/
```
sudo snap install act civo gh  
sudo snap install go --classic 
sudo snap install gum  --classic 
sudo snap install k9s 
sudo snap install kustomize 
```

- [ ] https://taskfile.dev/docs/installation
```
sudo snap install task --classic
```
##### git repo clonen
```
git clone http://192.168.3.112/bart/kubernetis
```
##### Aanpassen ~/.profile
      - [ ] chmod +x task.bash
      - [ ] nano ~/.profile
      - [ ] source $HOME/kubernetis/task.bash
      - [ ] export PATH=$PATH:$(go env GOPATH)/bin

##### Regeleindes goed zetten
      - [ ] sed -i 's/\r//' task.bash

- [ ] https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management 
```
sudo apt-get install -y kubectx nodejs procps git-all 
```
##### Alias toevoegen aan .bashrc
- [ ] nano ~/.bashrc   
```
alias k=kubectl
alias t=task
alias tl='task --list-all'
```
##### kind installeren
```
go install sigs.k8s.io/kind@v0.24.0 
```
- [ ] tasks
```
task kind:01-generate-config
task kind:02-create-cluster
```

## Werken met de kluster
```
kubectl get nodes
```
NAME                 STATUS   ROLES           AGE     VERSION  
kind-control-plane   Ready    control-plane   3m      v1.31.0  
kind-worker          Ready    <none>          2m44s   v1.31.0  
kind-worker2         Ready    <none>          2m44s   v1.31.0  
```
kubectl get pods -A
```
NAMESPACE            NAME                                         READY   STATUS    RESTARTS   AGE  
kube-system          coredns-6f6b679f8f-r8v5f                     1/1     Running   0          4m11s  
kube-system          coredns-6f6b679f8f-rw8k6                     1/1     Running   0          4m11s  
kube-system          etcd-kind-control-plane                      1/1     Running   0          4m19s  
kube-system          kindnet-g5vf4                                1/1     Running   0          4m5s  
kube-system          kindnet-jctms                                1/1     Running   0          4m5s  
kube-system          kindnet-lzs7b                                1/1     Running   0          4m12s  
kube-system          kube-apiserver-kind-control-plane            1/1     Running   0          4m19s  
kube-system          kube-controller-manager-kind-control-plane   1/1     Running   0          4m19s 
kube-system          kube-proxy-66vdx                             1/1     Running   0          4m5s  
kube-system          kube-proxy-jr85h                             1/1     Running   0          4m5s  
kube-system          kube-proxy-vpj6w                             1/1     Running   0          4m12s  
kube-system          kube-scheduler-kind-control-plane            1/1     Running   0          4m19s  
local-path-storage   local-path-provisioner-57c5987fd4-gfs5d      1/1     Running   0          4m11s  

