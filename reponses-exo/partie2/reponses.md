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

# VERIFICATIONS

## Les différentes ressources sont déployées dans le bon namespace ? Quelle commande utilisez-vous pour vérifier cela ?

Pour afficher les ressources de tous les namespaces et vérifier leur namespace : kubectl get all -A

Pour vérifier également les ConfigMaps, Secrets et PersistentVolumeClaims : kubectl get configmap,secret,pvc -A

## Les ressources MySQL ConfigMap et MySQL sont à déployer au sein des namespaces middle et database, pourquoi ?

Le StatefulSet MySQL, son Service, son Secret et son ConfigMap sont déployés dans database. WordPress est déployé dans middle et se connecte à MySQL via le nom DNS du Service : mysql-clusterip.database.svc.cluster.local

Les ConfigMaps et Secrets sont des ressources limitées à leur namespace. Le Deployment WordPress ne peut donc pas référencer directement le ConfigMap ou le Secret situés dans database. Une copie du ConfigMap et du Secret doit être créée dans middle pour fournir à WordPress ses variables d'environnement.

Le StatefulSet MySQL, lui, ne doit être déployé que dans database : il ne faut pas créer une deuxième instance MySQL dans middle.

## Les différentes pods sont-ils correctement démarrés ? Quelle commande utilisez-vous pour vérifier cela ?

Pour vérifier que les pods sont démarrés et prêts :  
kubectl get pods -A  
kubectl get pods -n database  
kubectl get pods -n middle  
kubectl get pods -n front  

La colonne READY doit indiquer le nombre de conteneurs prêts et la colonne STATUS doit généralement être à Running.

## Lorsqu'un pod est en erreur comment accédez-vous aux events du namespace et aux logs du pod ? Quelles commandes utilisez-vous pour vérifier cela ?

Pour consulter les événements du namespace, triés du plus ancien au plus récent : kubectl get events -n "namespace"

Pour afficher les logs du pod :  
kubectl logs "nom-du-pod" -n middle  
kubectl logs -f "nom-du-pod" -n middle