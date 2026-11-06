# Namespaces

## Liste des commandes utilisées
kubectl get namespaces

kubectl apply -f ./TP/front/namespace.yml  
kubectl apply -f ./TP/middle/namespace.yml  
kubectl apply -f ./TP/database/namespace.yml  

kubectl describe namespace front  
kubectl describe namespace middle  
kubectl describe namespace database  

kubectl get namespaces

# NGINX

## Liste des commandes utilisées

kubectl get events --namespace=front  
kubectl get pods --namespace=front  

kubectl port-forward -n front svc/nginx 8080:8383  

### Quels sont les 2 commandes utilisées ?

kubectl get events --namespace=front  
kubectl get pods --namespace=front  

### A quoi correspond les pointillés violets ?
Cela correspond à un Node

### A quoi correspond les pointillés ? 
Cela correspond à un cluster

