# MYSQL

## Liste de commandes utilisées
kubectl apply -f TP/database/mysql-configmap.yml -n database  
kubectl apply -f TP/database/mysql-secret.yml -n database  
kubectl apply -f TP/database/mysql-statefulset.yml -n database  

kubectl get pods -n database  
kubectl exec -n database -it mysql-statefulset-0 -- env  

# WORDPRESS

## Liste de commandes utilisées
kubectl apply -f TP/middle/wordpress-pv.yml -n middle  
kubectl apply -f TP/middle/wordpress-pvc.yml -n middle  
kubectl apply -f TP/database/mysql-secret.yml -n middle  
kubectl apply -f TP/database/mysql-configmap.yml -n middle  
kubectl apply -f TP/middle/wordpress-deployment.yml -n middle  
kubectl apply -f TP/middle/wordpress-service.yml -n middle  

# NGINX

## Liste de commades utilisées
kubectl apply -f TP/front/nginx-cm.yml -n front
kubectl apply -f TP/front/nginx-deployment.yml -n front
kubectl apply -f TP/front/nginx-service.yml -n front
kubectl port-forward -n front svc/nginx 8080:8383  