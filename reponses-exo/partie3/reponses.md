# Cronjob

## Liste des commandes utilisées
kubectl apply -f TP/cronjob.yml

# HPA

## Liste des commandes utilisées
kubectl apply -f TP/hpa/application.yml -n default  
kubectl apply -f TP/hpa/hpa-application.yml -n default  

## Question 4 : Avant de poursuivre, expliquez le résultat souhaité.
Le résultat souhaité est que l'application déployée "mon-application" soit capable de gérer automatiquement la charge en ajustant le nombre de pods en fonction de l'utilisation des ressources du CPU et de la mémoire.

## Question 6 : Vérifiez le HPA. Quelle commande avez-vous utilisé ?
kubectl get hpa mon-application -n default

## Question 7 : Expliquer les différentes colonnes retournées par la colonne
- NAME : Nom de l'HPA
- REFERENCE : Référence du déploiement associé à l'HPA
- TARGETS : Cibles de ressources pour l'autoscaling (CPU, mémoire)
- MINPODS : Nombre minimum de pods que l'HPA peut créer
- MAXPODS : Nombre maximum de pods que l'HPA peut créer
- REPLICAS : Nombre actuel de pods gérés par l'HPA  
- AGE : Durée de fonctionnement de l'HPA depuis sa création

## Question 9 : Votre HPA fonctionne-t-il comme souhaité ? Ajustez le si nécessaire
Oui, l'HPA fonctionne comme souhaité. Il ajuste automatiquement le nombre de pods en fonction de l'utilisation des ressources.