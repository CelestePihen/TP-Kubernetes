# Partie 1

## Liste des commandes utilisées

kubectl create ns prod-app  
kubectl apply -f TP/prod-app/app-deployment.yml -n prod-app  
kubectl apply -f TP/prod-app/app-service.yml -n prod-app  
kubectl port-forward -n prod-app svc/app-service 8080:8080  

## Que remarquez-vous sur la valeur "Pod Name" affichée à chaque rafraîchissement ? Pourquoi cela se produit-il ?
À chaque rafraîchissement, le nom du pod affiché change car la page est servie par différents pods. Cele se produit car le Deployment possède 3 réplicas.

## ❓ Kubernetes définit 3 classes de Qualité de Service (QoS) : Guaranteed, Burstable, BestEffort. En vous basant sur la configuration que vous venez de mettre en place, à quelle classe appartient votre pod ? Pourquoi est-il crucial pour un développeur de viser au minimum la classe Burstable pour une application de production ?

Notre pod appartient à la classe "Burstable". Viser Burstable permet de garantir une meilleure stabilité et disponibilité de l'application en production, en évitant qu'elle soit parmi les premières à être exclues lorsque les ressources deviennent insuffisantes.

# Partie 2

## Liste de commandes utilisées

kubectl apply -f TP/prod-app/app-service.yml -n prod-app  
kubectl describe pod prod-app-6fdfb9f5c8-27fk2 -n prod-app  


## Expliquez la différence de conséquence entre un échec de la livenessProbe et un échec de la readinessProbe. Quel composant de Kubernetes cesse d'envoyer du trafic au pod dans le second cas ?
- livenessProbe : vérifie si le conteneur fonctionne correctement. En cas d'échec répété, le kubelet redémarre le conteneur pour tenter de rétablir son fonctionnement.

- readinessProbe : vérifie si le conteneur est prêt à recevoir du trafic. En cas d'échec, le conteneur n'est pas redémarré, mais le Pod est retiré des endpoints disponibles du Service jusqu'à ce qu'il soit de nouveau prêt.

# Partie 3

## Comment les readinessProbes que vous avez configurées à l'étape précédente sont-elles essentielles pour garantir un RollingUpdate sans aucune interruption de service ?

Les readinessProbes évitent d'envoyer des requêtes vers des Pods qui ne sont pas encore opérationnels, ce qui contribue à assurer une mise à jour sans interruption de service.

## À quoi sert ce preStop hook ? Dans un vrai scénario applicatif, que ferait une application pendant ce temps de "grâce" au lieu de simplement dormir ?

Le hook preStop prépare l'arrêt propre du conteneur. Ici, sleep 10 attend 10 secondes avant que Kubernetes envoie le signal d'arrêt, ce qui explique le statut Terminating pendant ce délai.

Dans une application réelle, ce temps servirait à terminer les requêtes en cours ou de sauvegarder les données. L'objectif est d'éviter les requêtes interrompues et les pertes de données.

# Partie 4

## Pourquoi cette approche est-elle considérée comme une bien meilleure pratique de sécurité et d'exploitation que d'inclure tous les outils de débogage (curl, ping, vim, etc.) directement dans l'image de votre application de production ?

Un conteneur éphémères permet de garder une image minimale car il y a moins d'outils et de dépendances, cela signifie moins de vulnérabilités potentielles et moins d'outils utilisables par un pirate.

Une image légère est également plus rapide à télécharger et plus simple à maintenir.