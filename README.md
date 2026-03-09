# TP Kubernetes avec Minikube

Ce TP consiste à déployer un service web Java Spring Boot dans
Kubernetes en utilisant Docker, Minikube et un Ingress pour exposer le
service.

------------------------------------------------------------------------

# Installation de Minikube

Documentation officielle :\
https://minikube.sigs.k8s.io/docs/start/

Minikube propose un tableau de bord web.

Lancer le dashboard :

    minikube dashboard

------------------------------------------------------------------------

# Télécharger ce projet

Ce projet contient un service web codé en Java Spring Boot.\
Le projet est déjà compilé et la version binaire est disponible.

Cloner le projet :

    git clone https://github.com/charroux/kubernetes-minikube

Accéder au dossier du service :

    cd kubernetes-minikube/MyService

------------------------------------------------------------------------

# Tester le projet avec Docker

## Construire l'image Docker

    docker build -t myservice .

Vérifier l'image :

    docker images

## Lancer le conteneur

    docker run -p 4000:8080 -t myservice

-   Port 8080 : port du service web
-   Port 4000 : port exposé sur la machine

Tester dans le navigateur :

http://localhost:4000

Arrêter avec :

CTRL + C

------------------------------------------------------------------------

# Gestion des conteneurs Docker

Voir les conteneurs :

    docker ps

Arrêter un conteneur :

    docker stop containerID

------------------------------------------------------------------------

# Publier l'image sur Docker Hub

Tag de l'image :

    docker tag imageID sabrinabela/myservice:latest

Connexion à Docker Hub :

    docker login

Push de l'image :

    docker push sabrinabela/myservice:latest

Image disponible sur :

https://hub.docker.com/r/sabrinabela/myservice

------------------------------------------------------------------------

# Création d'un déploiement Kubernetes

Vérifier le cluster :

    kubectl get nodes

Créer un déploiement :

    kubectl create deployment myservice --image=sabrinabela/myservice:latest

Vérifier les pods :

    kubectl get pods

Afficher les détails d'un pod :

    kubectl describe pods

------------------------------------------------------------------------

# Accéder à un conteneur dans un pod

Entrer dans un pod :

    kubectl exec -it podname -- /bin/bash

Nom du pod :

    kubectl get pods

Lister les fichiers :

    ls

Quitter :

    exit

------------------------------------------------------------------------

# Création d'un Service Kubernetes

Un service permet d'exposer un pod.

Créer le service NodePort :

    kubectl expose deployment myservice --type=NodePort --port=8080

Vérifier les services :

    kubectl get services

Accéder au service avec Minikube :

    minikube service myservice

------------------------------------------------------------------------

# Mise à jour de l'image Docker (version v2)

Reconstruire l'image :

    docker build -t sabrinabela/myservice:v2 .

Push de la nouvelle image :

    docker push sabrinabela/myservice:v2

Mettre à jour le déploiement :

    kubectl set image deployment/myservice myservice=sabrinabela/myservice:v2

Vérifier les pods :

    kubectl get pods

------------------------------------------------------------------------

# Création des fichiers Kubernetes

## Deployment

Fichier :

myservice-deployment.yml

Application :

    kubectl apply -f myservice-deployment.yml

------------------------------------------------------------------------

## Service

Fichier :

myservice-service.yml

Application :

    kubectl apply -f myservice-service.yml

Vérification :

    kubectl get services

------------------------------------------------------------------------

# Configuration de l'Ingress

L'Ingress permet d'accéder au service via une URL.

## Activer l'Ingress dans Minikube

    minikube addons enable ingress

Vérifier les pods ingress :

    kubectl get pods -n ingress-nginx

------------------------------------------------------------------------

# Création du fichier Ingress

Fichier :

ingress.yml

Contenu :

``` yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  ingressClassName: nginx
  rules:
    - host: myservice.info
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: myservice
                port:
                  number: 8080
```

Appliquer :

    kubectl apply -f ingress.yml

Vérifier :

    kubectl get ingress

------------------------------------------------------------------------

# Configuration DNS locale

Sous Windows, modifier le fichier :

C:`\Windows`{=tex}`\System32`{=tex}`\drivers`{=tex}`\etc`{=tex}`\hosts`{=tex}

Ajouter :

    127.0.0.1 myservice.info

------------------------------------------------------------------------

# Tunnel Minikube

Pour exposer l'Ingress :

    minikube tunnel

Laisser le terminal ouvert.

------------------------------------------------------------------------

# Tester l'accès au service

Dans le navigateur :

http://myservice.info

Le message du service doit apparaître.

------------------------------------------------------------------------

# Vérifications Kubernetes

Voir les pods :

    kubectl get pods

Voir les services :

    kubectl get services

Voir l'ingress :

    kubectl get ingress

------------------------------------------------------------------------

# Nettoyage des ressources

Supprimer le service :

    kubectl delete service myservice

Supprimer le déploiement :

    kubectl delete deployment myservice

------------------------------------------------------------------------

# Conclusion

Dans ce TP nous avons :

-   créé une image Docker d'un service Spring Boot
-   publié l'image sur Docker Hub
-   déployé l'application sur Kubernetes
-   exposé le service avec un Service NodePort
-   mis à jour l'application avec une nouvelle version
-   configuré un Ingress pour accéder au service via une URL
    personnalisée

Ce TP permet de comprendre le cycle complet de déploiement d'une
application conteneurisée dans Kubernetes avec Minikube.
