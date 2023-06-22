	
####  Creating and deploy App on Azure platform (AKS)
az login

####  Resource Group

az acr create --name exploredocker7 --resource-group explore-docker-aks-rg --sku basic --admin-enabled

#### Local

change tags on docker images
docker tag webapp:latest exploredocker7.azurecr.io/webapp:v1

exploredocker7.azurecr.io/webapi
exploredocker7.azurecr.io/webapp


#### login to ACR

az acr login --name exploredocker    

docker push exploredocker7.azurecr.io/webapi:v1

--------------------------------------------------

az acr repository list --name <registry-name> --output table
az acr repository list --name exploredocker7 --output table

     
 #### Result 
  webapi
  webapp


#### Create AKS

az aks create --resource-group explore-docker-aks-rg --name explore-docker-aks --node-vm-size Standard_B2s --node-count 1 --generate-ssh-keys --location germanywestcentral


az aks create --resource-group explore-docker-aks-rg --name explore-docker-aks --node-vm-size Standard_B2s --node-count 1 --generate-ssh-keys --location westeurope

--work
az aks create --resource-group explore-docker-aks-rg --name explore-docker-aks --node-vm-size Standard_DS2_v2 --node-count 1 --generate-ssh-keys --location norwayeast



------------------------------------------------
kubectl get nodes
NAME                                STATUS   ROLES   AGE   VERSION
aks-nodepool1-25510838-vmss000000   Ready    agent   11m   v1.22.6

-------------------------------------------------------------
az aks get-credentials --resource-group explore-docker-aks-rg --name explore-docker-aks

az aks update --name explore-docker-aks --resource-group explore-docker-aks-rg --attach-acr exploredocker7

------------------------------------------------------------


kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard

az aks browse --resource-group explore-docker-aks-rg --name explore-docker-aks


kubectl get all -n kube-systems

kubectl cluster-info

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml

 kubectl get pods --all-namespaces
kubernetes-dashboard   dashboard-metrics-scraper-c45b7869d-kb8gw   0/1     ContainerCreating   0          3s
kubernetes-dashboard   kubernetes-dashboard-764b4dd7-xlkbn         0/1     ContainerCreating   0          3s

------------------------------------------------------------------
 kubectl get serviceaccounts --all-namespaces

 kubectl get ns

------------------------------------------------------------------
 kubectl describe serviceaccount kubernetes-dashboard -n kubernetes-dashboard

Mountable secrets:   kubernetes-dashboard-token-49qzl
Tokens:              kubernetes-dashboard-token-49qzl

kubectl describe secret kubernetes-dashboard-token-49qzl -n kubernetes-dashboard

token:      xxxxx your tocken

-------------------------------------------------------------------

 kubectl apply -f RBAC-kubernetes-dashboard.yml


kubectl create clusterrolebinding cluster-admin --clusterrole=cluster-admin --user=user1 --user=user2 --group=group1
Create a cluster role binding for a particular cluster role.

-------------------------------------------------------------------

     kubectl get deployments
     NAME     READY   UP-TO-DATE   AVAILABLE   AGE			
     webapi   1/1     1            1           5d23h
     webapp   1/1     1            1           5d23h
-------------------------------------------------------------------

    kubectl get services
    NAME         TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)        AGE								
    kubernetes   ClusterIP      10.0.0.1       <none>         443/TCP        5d23h
    webapi       ClusterIP      10.0.184.71    <none>         80/TCP         5d23h
    webapp       LoadBalancer   10.0.202.208   20.251.3.178   80:30155/TCP   5d23h
-------------------------------------------------------------------
kubectl get pods

kubectl get service -o wide

kubectl delete svc

kubectl delete --all services --namespace=*here-you-enter-namespace

kubectl delete deployment deployment-name


kubectl get events --all-namespaces  | grep -i $podname

kubectl logs commands-depl-686f57dcb-hl7vz

kubectl logs -f deploy/ -n

kubectl describe pod commands-depl-686f57dcb-hl7vz

kubectl describe pod commands-depl-686f57dcb-hl7vz | grep -A20 Events

----------------------------------------------------------------------

kubectl get deployments

kubectl rollout restart deployment  mssql-depl//platforms-depl

----------------------------------------------------------------------
az account list

az login

az account show

az logout

az aks stop  --name explore-docker-aks --resource-group explore-docker-aks-rg

az aks start --name explore-docker-aks --resource-group explore-docker-aks-rg

----------------------------------------------------------------------

kubectl config get-contexts

| CURRENT   NAME     | CLUSTER            | AUTHINFO        NAMESPACE                            |
|--------------------|--------------------|------------------------------------------------------|
| docker-desktop     | docker-desktop     | docker-desktop                                       |
| explore-docker-aks | explore-docker-aks | clusterUser_explore-docker-aks-rg_explore-docker-aks |
| minikube           | minikube           | default                                              |

 kubectl config use-context docker-desktop

 kubectl config view

----------------------------------------------------------------------
 kubectl logs commands-depl-66f7fd8767-7knvl

 kubectl create secret generic mssql --from-literal=SA_PASSWORD="pa55w0rd!"

----------------------------------------------------------------------

kubectl get pvc -A

xx.xxx.xx.xx,1433

---
#### Setting of dashboard


To deploy Dashboard, execute following command:
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

kubectl proxy


1. Create The Dashboard Service Account

kubectl create serviceaccount kubernetes-dashboard-admin-sa -n kube-system

2. Bind The Service Account To The Cluster-Admin Role

kubectl create clusterrolebinding kubernetes-dashboard-admin-sa --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard-admin-sa

3.List Secretes

kubectl get secrets -n kube-system

//if you using kubernetes 1.23 or above please use following to get secret
kubectl -n kube-system create token kubernetes-dashboard-admin-sa

4.Get The Token From Secret
kubectl describe secret kubernetes-dashboard-admin-sa-token-lj8cc -n kube-system
