//dashboard

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
