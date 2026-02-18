# TP Kubernetes - Orchestration de conteneurs

Ce dépôt contient l'environnement et les manifestes pour les TP 02 et TP 03 sur Kubernetes.

## Prérequis
* VirtualBox
* Vagrant

## Démarrage de l'environnement (TP 02)

1. Lancer la VM :
   ```bash
   vagrant up
   vagrant ssh



# Utilisation de la version 1.28.0 compatible avec le Minikube actuel
minikube start -p kubelabs --kubernetes-version v1.28.0 --vm-driver docker --cni calico

Appliquer le déploiement :

kubectl apply -f TP03/kuard-deployment.yaml


Vérifier les Pods :

kubectl get pods

# Redirection du port 80 du conteneur vers le 8081 de la VM
kubectl port-forward deployment/kuard --address 0.0.0.0 8081:80